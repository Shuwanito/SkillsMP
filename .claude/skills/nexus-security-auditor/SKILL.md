---
name: nexus-security-auditor
description: "Comprehensive application security audit agent. Performs OWASP Top 10 analysis, dependency CVE scanning, authentication/authorization review, API security testing, infrastructure config audit, and generates remediation playbooks with severity-ranked findings."
license: proprietary
compatibility: "Any LLM agent with filesystem and web access. Audits web apps (Python, Node.js, Java, Go, .NET), APIs (REST, GraphQL), infrastructure configs (Docker, K8s, Terraform), and CI/CD pipelines."
metadata:
  department: cybersecurity
  agents: ["cyber-red", "cyber-blue"]
  price_per_execution: "$2.00"
  version: "2.0.0"
  publishable: true
  categories: ["security", "compliance", "penetration-testing", "vulnerability-assessment"]
  trigger_keywords: ["security audit", "vulnerability scan", "OWASP check", "penetration test", "security review", "CVE scan", "auth review", "security headers", "compliance check", "threat model"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Security Auditor

Enterprise-grade application security auditing that combines OWASP methodology, real-time CVE intelligence, and infrastructure hardening checks into a single structured assessment.

## When to Use This Skill

- **Pre-launch security review**: Full audit before deploying a new application
- **Dependency vulnerability scan**: Check all packages for known CVEs
- **Auth system review**: Evaluate authentication and authorization flows
- **API security assessment**: Test REST/GraphQL endpoints for common attack vectors
- **Infrastructure config audit**: Review Docker, Kubernetes, Terraform for misconfigurations
- **Compliance preparation**: Pre-check for SOC2, HIPAA, or GDPR technical requirements

## Instructions

### Step 1: Identify Attack Surface

Map the application's attack surface by reading:
- Entry points: API routes, form handlers, WebSocket endpoints, file upload paths
- Auth mechanisms: JWT, session cookies, OAuth, API keys
- Data stores: databases, caches, file systems, external APIs
- Infrastructure: Docker configs, cloud provider configs, reverse proxy settings
- Dependencies: package.json, requirements.txt, go.mod, pom.xml

### Step 2: Execute Audit Modules

**Module 1 -- OWASP Top 10 (2021) Checks**

| ID | Check | What to Look For |
|----|-------|-------------------|
| A01 | Broken Access Control | Missing auth on endpoints, IDOR, path traversal, CORS misconfiguration |
| A02 | Cryptographic Failures | Weak hashing (MD5/SHA1 for passwords), missing TLS, exposed secrets |
| A03 | Injection | SQLi, NoSQLi, command injection, LDAP injection, template injection |
| A04 | Insecure Design | Missing rate limiting, no account lockout, predictable tokens |
| A05 | Security Misconfiguration | Debug mode on, default credentials, verbose errors, directory listing |
| A06 | Vulnerable Components | Known CVEs in dependencies, outdated frameworks |
| A07 | Auth Failures | Weak password policy, missing MFA support, session fixation |
| A08 | Data Integrity Failures | Insecure deserialization, unsigned updates, CI/CD pipeline injection |
| A09 | Logging Failures | No audit trail, logging sensitive data, missing alerting |
| A10 | SSRF | Unvalidated URL fetching, internal network access via user input |

**Module 2 -- Dependency CVE Scan**
- Parse lockfiles (package-lock.json, poetry.lock, Cargo.lock, etc.)
- Cross-reference each package+version against OSV.dev and NVD databases
- Flag: CVE ID, CVSS score, affected version range, fix version if available

**Module 3 -- Authentication & Authorization Deep Dive**
- Token lifecycle: generation, validation, expiry, revocation
- Session management: secure flags, SameSite, HttpOnly, expiry
- Role-based access: privilege escalation paths, missing checks on state-changing operations
- Password handling: hashing algorithm, salt usage, reset flow security

**Module 4 -- API Security**
- Input validation on all endpoints (type, length, format)
- Rate limiting and throttling configuration
- Error responses (do they leak internal details?)
- GraphQL-specific: introspection enabled, query depth limits, batching abuse

**Module 5 -- Infrastructure Hardening**
- Docker: running as root, exposed ports, secrets in build layers, base image age
- Kubernetes: pod security policies, network policies, RBAC config, secret management
- Terraform: public S3 buckets, open security groups, unencrypted storage
- CI/CD: secret injection methods, branch protection, artifact signing

### Step 3: Risk Classification

For each finding:
- **Severity**: CRITICAL / HIGH / MEDIUM / LOW / INFO
- **CVSS Score** (where applicable): 0.0-10.0
- **Exploitability**: How easy to exploit (Trivial / Moderate / Difficult / Theoretical)
- **Business Impact**: Data breach / Service disruption / Reputation / Compliance violation
- **Remediation Effort**: Quick fix / Moderate / Significant refactor

### Step 4: Generate Remediation Playbook

```json
{
  "audit_summary": {
    "scope": "Full application + infrastructure",
    "components_audited": ["API (42 endpoints)", "Auth (JWT + OAuth)", "Deps (187 packages)", "Docker config"],
    "risk_rating": "HIGH",
    "critical": 2,
    "high": 5,
    "medium": 8,
    "low": 4,
    "pass_rate": "68%"
  },
  "findings": [
    {
      "id": "SA-001",
      "owasp": "A03:2021",
      "severity": "CRITICAL",
      "cvss": 9.8,
      "title": "NoSQL injection in user search endpoint",
      "location": "api/routes/users.py:89",
      "description": "MongoDB query constructed with unsanitized user input allows arbitrary query operators via $gt, $ne.",
      "proof_of_concept": "POST /api/users/search {\"username\": {\"$ne\": \"\"}} returns all users",
      "remediation": "Use pymongo parameter binding or validate input against allowed string pattern.",
      "effort": "Quick fix",
      "references": ["https://owasp.org/Top10/A03_2021-Injection/"]
    }
  ],
  "remediation_priority": [
    "1. Fix SA-001 (NoSQLi) -- immediate, blocks deployment",
    "2. Rotate exposed API key SA-003 -- immediate",
    "3. Enable rate limiting SA-007 -- this sprint"
  ]
}
```

## Example Input/Output

**Input**: "Audit the FastAPI backend at /backend/app/ focusing on authentication and API security"

**Output**: Report identifying (example):
- CRITICAL: Admin endpoint /api/admin/users accessible without authentication middleware
- HIGH: JWT tokens signed with HS256 using a 16-character secret (brute-forceable)
- HIGH: No rate limiting on /api/auth/login (credential stuffing risk)
- MEDIUM: CORS allows wildcard origin in production config
- LOW: Health check endpoint exposes framework version in response headers

## Edge Cases

- **No auth system present**: Note as CRITICAL design gap, skip auth-specific modules
- **Static sites / SPAs without backend**: Focus on dependency scan and client-side security only
- **Microservices**: Audit each service independently, plus inter-service communication security
- **Legacy codebases with deprecated packages**: Flag but note that some CVEs may not be exploitable in context
- **Encrypted/obfuscated config files**: Report as unable to audit, request decrypted versions
- **Multiple environments (dev/staging/prod)**: Always audit production config; note if only dev config is available

## What Sets This Apart

- Goes beyond simple CVE lists -- includes **exploitability assessment** and **proof-of-concept** descriptions
- Covers the full stack: code, dependencies, infrastructure, and CI/CD pipeline
- Generates a **prioritized remediation playbook**, not just a list of problems
- Maps findings to **OWASP categories** for compliance reporting
- Provides **business impact** context so non-technical stakeholders understand the risk

## Pricing

- Per-execution: $2.00
- Outcome-based: $5.00 per confirmed CRITICAL finding (pay only for real issues)
- Volume: 20% discount at 100+ executions/month
- Enterprise: Unlimited monthly audits with SLA on response time

## Output Format

Default: JSON. Supports `format: "markdown"` for readable reports or `format: "sarif"` for CI/CD integration.
