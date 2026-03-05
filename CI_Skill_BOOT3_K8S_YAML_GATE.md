# CI Skill: boot3-k8s-yaml-gate

## Purpose
Run in CI to **scan and gate Kubernetes YAML / Helm values** during **Spring Boot 2 → 3** upgrades, preventing:
- **Double slashes `//` in request paths** (rewrite/join mistakes)
- **Trailing slash before query `/?`** (e.g., `/v1/xxx/?a=b`)
- **Prefix duplication** (stripPrefix + app base-path / forwarded prefix)
- **Broken probes** due to actuator path/security drift

## Scope
**Inputs**
- `k8s/**/*.yaml` (Deployment, Service, Ingress, APISIXRoute, Istio VirtualService, etc.)
- Helm values: `values*.yaml` (optional)

**Outputs**
1. **Gate Summary:** PASS/FAIL + counts
2. **Findings list:** `ruleId, severity, file, line, snippet, reason, fix`
3. **Auto-fix patch:** unified diff for fixable items
4. **Verification commands:** `kubectl` + `curl` checks

---

## Rule Set

### A) Path Hygiene (MUST FAIL)

#### R001 (FAIL) — `//` in path/rewrite/uri (excluding `https://`)
Fail if any **path-like** field contains `//` **except** the scheme portion of `https://`.

**Targets (typical)**
- Ingress: `spec.rules[].http.paths[].path`
- NGINX rewrite annotations (if used)
- APISIXRoute: `uri`, `upstream_path` (or equivalent)
- Istio VirtualService: `match.uri`, `rewrite.uri`

**Reason**
`//` often comes from prefix/path join logic and becomes **404/400** after Boot3 (stricter matcher/firewall).

**Fix**
Normalize path join:
- Base URL **must not end with** `/`
- Path **must start with** `/` and **must not end with** `/`
- Rewrite rules must guarantee no consecutive `/` in output

---

#### R002 (FAIL) — `/?` (trailing slash before query)
Fail if any literal string contains `/?`.

**Bad**
- `/v1/inhibition/create-auto-inhibition/?apName=RMS`

**Good**
- `/v1/inhibition/create-auto-inhibition?apName=RMS`

**Reason**
`/?` changes the request path (`.../` vs `...`) and often becomes **404** under stricter path matching.

**Fix**
Remove the trailing slash before `?`.

---

#### R003 (FAIL by default) — trailing slash in route paths
Fail if any route path ends with `/` unless allowlisted.

**Default allowlist**
- `/`
- `/actuator`
- `/actuator/health`
- `/actuator/health/*`

**Reason**
Boot3 path matching is stricter; trailing slashes cause route mismatch and “works in some envs” behavior.

**Fix**
Standardize: **no trailing slash** in API paths.

---

#### R004 (FAIL) — prefix duplication risk
Fail if:
- Gateway/Ingress uses `stripPrefix` or rewrite that removes a prefix **AND**
- App also sets `server.servlet.context-path` or `spring.webflux.base-path`, **OR**
- Gateway injects `X-Forwarded-Prefix` while app also hardcodes base-path/context-path

**Reason**
High probability of:
- double prefix
- `//` in final path
- inconsistent redirects / callback URLs

**Fix**
Choose **one** place to own prefix:
- Either gateway manages prefix (strip/rewrite + forwarded headers)
- Or app owns context-path/base-path (gateway passes through)

---

### B) Probes / Actuator (recommended gate)

#### P001 (WARN / optionally FAIL) — missing `startupProbe`
**Reason**
Avoid killing cold starts before the app is ready.

**Fix**
Add `startupProbe` hitting `/actuator/health` (or your base-path).

---

#### P002 (WARN / optionally FAIL) — probes not using Boot3 health groups
Recommend:
- readiness: `/actuator/health/readiness`
- liveness: `/actuator/health/liveness`

**Fix**
Update probe paths accordingly.

---

#### P003 (WARN) — probe path inconsistent with gateway rewrite/prefix
**Reason**
Health checks pass in Pod-direct but fail via gateway, or vice versa.

**Fix**
Ensure probe route matches the actual runtime path after rewrite/prefixing.

---

### C) Forwarded Headers / Redirect Consistency

#### H001 (WARN) — gateway uses prefix but forwarded headers not set
**Reason**
Redirects, links, and absolute URLs may break.

**Fix**
Set consistent forwarded headers behavior (gateway-side) OR configure app to respect them.

---

#### H002 (WARN/FAIL) — both forwarded-prefix and hardcoded base-path
**Reason**
Duplication and path corruption.

**Fix**
Pick one owner: gateway or app.

---

## Claude Prompt (drop-in skill prompt for CI)

```text
You are an SRE/Platform CI reviewer. Scan all Kubernetes YAML and Helm values in this repo and enforce these gates for Spring Boot 2 -> 3 upgrades.

MUST FAIL:
- R001: Any path/rewrite/uri/upstream_path contains double slashes '//' (exclude 'https://' scheme)
- R002: Any string contains '/?' (trailing slash before query), e.g. '/v1/xxx/?a=b'
- R004: Prefix duplication risk: stripPrefix/rewrite removes a prefix AND the app also uses context-path/base-path OR gateway injects X-Forwarded-Prefix while app hardcodes base path

DEFAULT FAIL (unless allowlisted):
- R003: Any route path ends with '/' (allowlist only '/', '/actuator', '/actuator/health', '/actuator/health/*')

RECOMMENDED WARN/FAIL:
- P001: Missing startupProbe
- P002: readiness/liveness probes not using '/actuator/health/readiness' and '/actuator/health/liveness'
- P003: probe path inconsistent with gateway rewrite/prefix

Output format:
1) Gate Summary: PASS/FAIL + counts
2) Findings: each with ruleId, severity, file, line, snippet, reason, fix
3) Auto-fix patch: provide unified diff for fixable issues (remove '/?' and trailing '/', obvious '//')
4) Verification: provide kubectl port-forward + curl commands and gateway-entry curl commands
Before reporting, list all YAML files scanned.
```