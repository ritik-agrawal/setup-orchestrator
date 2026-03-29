# TECH-DECISIONS.md

| Field | Value |
|-------|-------|
| **Author** | Ritik Agrawal |
| **Version** | 1.0.0 |
| **Created At** | 2026-03-29 |
| **Updated At** | 2026-03-29 |
| **Reference Requirement Doc** | Local Distributed Systems Orchestrator v2 |

---

## Overview
Captures all architectural and technical decisions with reasoning.

---

## Decisions

### Language: Python
- Strong subprocess handling
- Ideal for system-level orchestration

### No Docker
- Avoid heavy resource usage
- Faster local execution

### CLI-Based Interface
- Lightweight and fast
- No UI complexity

### Process-Based Execution
- Direct OS-level control

### In-Memory + JSON Session File
- Fast runtime access
- Optional persistence

### Parallel Execution
- ThreadPoolExecutor for simplicity

### Logging Strategy
- File-based logging for persistence and debugging

### No Dependency Graph
- Simplicity over orchestration complexity

### Git Validation
- Fail fast if branch not available

---

## References
- Requirement Document (v2)
- DEFINITIONS.md
- COMMANDS.md

