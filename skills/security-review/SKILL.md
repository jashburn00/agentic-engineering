---
name: security-review
description: Review a code change for security vulnerabilities — the application-security lens. Trace untrusted input to dangerous sinks and check the change against common vulnerability classes (OWASP/CWE), reporting findings with severity and remediation. Standalone and agent-agnostic; use before a change lands, especially when it touches input, auth, data, or external surfaces.
---

# Security Review

Read the change — the diff plus enough surrounding code to trace data flow — and look for vulnerabilities it introduces or exposes. Report findings; do not rewrite code unless asked.

This is the application-security lens: vulnerabilities *in the code*. It is separate from code review (`review` — correctness/quality) and from operational/supply-chain safety (safe pushes, secret redaction, signing — not covered here).

The goal is not a novel protocol but a **deliberate, repeatable, portable** pass, so security scrutiny happens every time rather than only when the model thinks of it.

## How to review

Trace **untrusted input → dangerous sink**: where does data from outside (user, network, file, env, another service) enter, and does it reach somewhere it can do harm without validation, encoding, or authorization? Focus on what the change touches; don't audit the whole repo.

## Vulnerability classes to check

- **Injection** — SQL, command, code/eval, XSS, template, LDAP, path/argument. Is input parameterized or escaped?
- **AuthN / AuthZ** — missing or wrong authentication; broken access control; missing ownership/permission checks; privilege escalation.
- **Secrets** — hardcoded credentials, keys, or tokens; secrets leaked to logs, errors, or committed files.
- **Crypto** — weak or homemade algorithms, static IVs, weak randomness where security depends on it, improper cert/TLS handling.
- **Sensitive data** — PII or credentials exposed in responses, logs, or error messages; missing redaction.
- **SSRF / open redirect** — user-controlled URLs reaching server-side requests or redirects.
- **Path / file** — traversal, unrestricted upload, unsafe file permissions.
- **Deserialization / parsing** — untrusted deserialization, XXE, decompression bombs.
- **Input validation** — missing bounds/type/format checks; integer overflow; unchecked size.
- **Dependencies** — newly added or upgraded packages: known-vulnerable, unmaintained, or untrusted source.
- **Resource / DoS** — unbounded loops, allocations, or recursion driven by attacker-controlled input.

## Calibrate

- Prefer real, exploitable findings over theoretical ones; name the attack path.
- Rate severity by impact × exploitability; mark low-confidence items as such rather than omitting or overstating them.
- Don't invent issues to look thorough. Silence is valid when the change is safe.

## Report

Emit findings, each with:
- `class` — the vulnerability class above.
- `severity` — critical | high | medium | low.
- `location` — file:line or scope.
- `summary` — the vulnerability and its attack path, briefly.
- `remediation` — how to fix it.

Close with a verdict: **clear** (no exploitable findings) or **vulnerabilities-found** (worst first). Treat critical and high as blockers.
