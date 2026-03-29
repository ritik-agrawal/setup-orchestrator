# DEFINITIONS.md

| Field | Value |
|-------|-------|
| **Author** | Ritik Agrawal |
| **Version** | 1.0.0 |
| **Created At** | 2026-03-29 |
| **Updated At** | 2026-03-29 |
| **Reference Requirement Doc** | Local Distributed Systems Orchestrator v2 |

---

## Overview
Defines all core system terminology for consistency.

---

## Definitions

### Project
A microservice or application with source code, optional build command, run command, and git repository.

### Build
The process of preparing a project for execution using a defined command.

### Run
Execution of a project as an OS-level process.

### Session
A runtime lifecycle instance of the orchestrator.

### Session File
A temporary JSON file storing runtime state.

### State
In-memory representation of all running services.

### Service Status
- NOT_STARTED
- BUILDING
- RUNNING
- FAILED
- STOPPED

### PID
Operating system process identifier.

### Log
Captured stdout/stderr of a process.

### Error Types
- GIT_ERROR
- BUILD_ERROR
- RUN_ERROR

---

## References
- Requirement Document (v2)
- COMMANDS.md
- TECH-DECISIONS.md

