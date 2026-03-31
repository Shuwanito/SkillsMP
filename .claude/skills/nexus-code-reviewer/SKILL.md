---
name: nexus-code-reviewer
description: "Deep code review agent for Python, FastAPI, React, and TypeScript projects. Detects bugs, security flaws, performance bottlenecks, architectural anti-patterns, and dependency risks. Returns structured JSON reports with severity, fix suggestions, and effort estimates."
license: proprietary
compatibility: "Any LLM agent with filesystem access. Works on Python 3.9+, Node 18+, TypeScript 5+, React 18+ codebases. Outputs JSON or Markdown."
metadata:
  department: development
  agents: ["cto", "backend", "qa"]
  price_per_execution: "$0.50"
  version: "2.0.0"
  publishable: true
  categories: ["code-quality", "security", "performance", "architecture"]
  trigger_keywords: ["review code", "code review", "PR review", "audit codebase", "check code quality", "find bugs", "refactor suggestions", "code smells", "technical debt"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Code Reviewer

Premium automated code review that catches what linters miss -- architectural problems, hidden bugs, security holes, and performance traps.

## When to Use This Skill

- **Pull request review**: Agent receives a diff or list of changed files
- **Codebase audit**: Agent receives a repository path for full-scan analysis
- **Refactoring guidance**: Agent needs to identify technical debt hotspots
- **Pre-deploy check**: Final quality gate before production deployment
- **Dependency risk check**: Evaluate third-party packages for CVEs and maintenance status

## Instructions

### Step 1: Gather Context

Read the target files or diff. Identify:
- Language and framework (Python/FastAPI, React/Next.js, TypeScript, etc.)
- Project structure (monorepo, microservices, single app)
- Existing linter config (.eslintrc, pyproject.toml, ruff.toml) if available

### Step 2: Run Analysis Passes

Execute these passes in order:

**Pass A -- Correctness**
- Null/undefined reference risks
- Off-by-one errors in loops and slices
- Race conditions in async code (missing await, unguarded shared state)
- Incorrect error handling (bare except, swallowed exceptions)
- Type mismatches (especially in TypeScript `any` usage)

**Pass B -- Security**
- SQL injection via string concatenation or f-strings in queries
- XSS vectors in React (dangerouslySetInnerHTML, unescaped user input)
- Hardcoded secrets (API keys, passwords, tokens in source)
- Insecure deserialization (pickle.loads, eval, exec)
- Missing input validation on API endpoints

**Pass C -- Performance**
- N+1 query patterns in ORM code (SQLAlchemy, Prisma, Django)
- Missing database indexes implied by query patterns
- Unbounded list operations (loading full tables into memory)
- Synchronous blocking calls inside async handlers
- Unnecessary re-renders in React (missing memo, unstable keys)

**Pass D -- Architecture**
- God classes/functions exceeding 200 lines
- Circular imports or circular dependencies
- Tight coupling between layers (business logic in route handlers)
- Missing abstraction (repeated patterns that should be shared)
- Violation of single responsibility principle

**Pass E -- Dependencies**
- Known CVEs in pinned versions (check against OSV/NVD)
- Unmaintained packages (no release in 12+ months)
- License conflicts (GPL in MIT-licensed projects)
- Duplicate dependencies serving the same purpose

### Step 3: Score and Classify

For each finding, assign:
- **Severity**: CRITICAL / HIGH / MEDIUM / LOW / INFO
- **Category**: correctness / security / performance / architecture / dependency
- **Impact** (1-10): How much damage if left unfixed
- **Effort** (1-10): How hard to fix (1 = one-line change, 10 = major refactor)
- **Priority**: Impact x (11 - Effort) -- higher means fix first
- **Confidence** (0.0-1.0): How certain the finding is real

### Step 4: Generate Output

Return a structured report:

```json
{
  "summary": {
    "files_reviewed": 12,
    "total_findings": 8,
    "critical": 1,
    "high": 2,
    "medium": 3,
    "low": 2,
    "overall_quality_score": 72
  },
  "findings": [
    {
      "id": "CR-001",
      "severity": "CRITICAL",
      "category": "security",
      "file": "app/api/users.py",
      "line": 45,
      "title": "SQL injection via f-string in query",
      "description": "User input is interpolated directly into SQL query without parameterization.",
      "current_code": "db.execute(f\"SELECT * FROM users WHERE id = {user_id}\")",
      "suggested_fix": "db.execute(\"SELECT * FROM users WHERE id = :id\", {\"id\": user_id})",
      "impact": 10,
      "effort": 2,
      "priority": 90,
      "confidence": 0.98
    }
  ],
  "recommendations": [
    "Add parameterized query linting rule to CI pipeline",
    "Consider SQLAlchemy ORM to prevent raw SQL injection patterns"
  ]
}
```

## Example Input/Output

**Input**: "Review the authentication module at /src/auth/ for security and correctness issues"

**Output**: Structured report identifying (example):
- CRITICAL: JWT secret loaded from environment without fallback validation -- app starts with empty secret in dev
- HIGH: Token expiry set to 30 days with no refresh rotation
- MEDIUM: Password reset endpoint lacks rate limiting
- LOW: Unused import of deprecated `jwt.decode` parameter

## Edge Cases

- **Minified/bundled code**: Skip analysis, report as non-reviewable
- **Generated code** (protobuf, OpenAPI stubs): Flag but classify findings as INFO only
- **Monorepo with multiple languages**: Run language-specific passes per subdirectory
- **Empty diff / no changes**: Return clean report with zero findings
- **Binary files in diff**: Skip with note, do not attempt to parse
- **Very large files (>5000 lines)**: Analyze in chunks, note that full-file patterns may be missed

## What Sets This Apart from Free Linters

- Detects **architectural** problems that ESLint/Ruff/Pylint cannot (circular deps, god objects, layer violations)
- Identifies **cross-file** patterns like N+1 queries that require understanding data flow
- Provides **fix suggestions** with actual code, not just warnings
- Scores **priority** so teams fix what matters first
- Checks **dependency health** beyond just CVEs (maintenance status, license risk)

## Pricing

- Per-execution: $0.50
- Outcome-based: Pay only for CRITICAL/HIGH findings confirmed valid
- Volume: 20% discount at 100+ executions/month
- Enterprise: Flat monthly rate with unlimited executions available

## Output Format

Default: JSON (as shown above). Set `format: "markdown"` for human-readable Markdown tables.
