# Security Policy

## Scope

Homo Economicus is a static skill for Claude Code. It contains no authentication, no server, no database, and no user data persistence. The attack surface is limited to:

- Python scripts (`scripts/build_report.py`) that process JSON and write HTML
- The HTML report template rendered locally in a sandbox

## Reporting a Vulnerability

If you find a security issue (e.g. XSS in the HTML template, path traversal in the build script), please **do not open a public issue**. Instead, report it privately via GitHub's Security Advisory feature:

**[Report a vulnerability](https://github.com/AlbanOtt/homo-economicus/security/advisories/new)**

Please include:
- A description of the issue
- Steps to reproduce
- Potential impact

Expected response time: within 7 days.
