# TODO.md

| Field | Value |
|-------|-------|
| **Author** | Ritik Agrawal |
| **Version** | 1.0.0 |
| **Created At** | 2026-03-29 |
| **Updated At** | 2026-03-29 |
| **Reference Requirement Doc** | Local Distributed Systems Orchestrator v2 |

---

## Overview
Tracks all pending design decisions before implementation begins.

---

## Pending Decisions

### Core Execution
- Should build phase be mandatory or optional per project?
- Should failed services auto-restart or require manual restart?

### Logging
- Log format standardization (JSON vs plain text)
- Log rotation strategy (needed or not?)

### Session Management
- Should session recovery (resume) be supported?
- Max number of concurrent sessions?

### CLI UX
- Interactive mode required or not?
- Should commands support batching (multiple services)?

### Git Handling
- Strategy for dirty repositories (fail vs stash vs warn)

### Error Handling
- Retry strategy for transient failures?

---

## References
- Requirement Document (v2)
- DEFINITIONS.md
- COMMANDS.md
- TECH-DECISIONS.md

