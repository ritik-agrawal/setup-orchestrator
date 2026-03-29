# COMMANDS.md

| Field | Value |
|-------|-------|
| **Author** | Ritik Agrawal |
| **Version** | 1.0.0 |
| **Created At** | 2026-03-29 |
| **Updated At** | 2026-03-29 |
| **Reference Requirement Doc** | Local Distributed Systems Orchestrator v2 |

---

## Overview
Defines CLI commands exposed by the orchestrator.

---

## Commands

### Start
```
orchestrator start
```

### Build
```
orchestrator build
```

### Run
```
orchestrator run
```

### Stop
```
orchestrator stop
```

### Restart
```
orchestrator restart <service>
```

### Status
```
orchestrator status
```

### Logs
```
orchestrator logs <service>
orchestrator logs <service> --tail
orchestrator logs <service> --error
```

### Exit
```
orchestrator exit
```

---

## Future Commands
- resume <session>
- list-sessions

---

## References
- Requirement Document (v2)
- DEFINITIONS.md
- TECH-DECISIONS.md

