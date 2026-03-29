# Local Distributed Systems Orchestrator (v2)

| Field | Value |
|-------|-------|
| **Author** | Ritik Agrawal |
| **Version** | 2.0.0 |
| **Created At** | 2026-03-26 |
| **Updated At** | 2026-03-29 |

---

## Overview

A lightweight **local process orchestrator** designed to manage multiple microservices without Docker. The system focuses on developer efficiency by enabling parallel builds, process-based execution, structured logging, and failure recovery — all through a CLI-driven interface and reusable core library.

---

## Design Philosophy

- **No Docker**: Avoid heavy resource usage and improve responsiveness
- **CLI-first approach**: Library + command-line interface instead of UI
- **Process-based execution**: Run services using native commands
- **Ephemeral state**: In-memory state with optional session file persistence
- **Failure isolation**: One service failure does not impact others
- **Extensibility via strategies**: Build, run, and logging behaviors are pluggable

---

## Core Features

### 1. Project Selection
- Select one or multiple services via CLI or config
- Supports partial execution

---

### 2. Git Integration
- Automatic branch checkout before build
- Flow:
  - `git fetch`
  - validate branch existence
  - checkout branch
- If branch is unavailable:
  - mark service as **GIT_ERROR**
  - skip build and run

---

### 3. Build Phase (Optional)
- Each service defines its own build command
- Supports parallel execution
- Failure handling:
  - Isolated per service
  - Logs captured
  - Failed services skipped in run phase

---

### 4. Run Phase (Process-Based)
- Executes services using native commands
- Parallel execution using thread pool
- Tracks:
  - PID
  - status (running/failed/stopped)
- Failure handling:
  - Detect via process exit
  - Restart capability (manual)

---

### 5. Logging System
- All service outputs are piped to log files

#### Structure
```
logs/
  <service-name>/
    run.log
    error.log
```

#### Features
- Timestamped logs
- Service-prefixed entries
- CLI access for logs

#### CLI Commands
```
orchestrator logs <service>
orchestrator logs <service> --tail
orchestrator logs <service> --error
```

---

### 6. Session Management (Ephemeral)

#### Characteristics
- In-memory state (primary)
- JSON session file (secondary)
- File deleted on exit by default

#### File Location
```
~/.local-orchestrator/session_<id>.json
```

#### Sample Structure
```json
{
  "session_id": "abc123",
  "created_at": "2026-03-29T10:15:00",
  "projects": {
    "user-service": {
      "status": "running",
      "pid": 12345,
      "branch": "develop",
      "last_error": null
    }
  }
}
```

#### CLI Flag
```
orchestrator start --keep-session
```

---

### 7. Failure Recovery

#### Build Failures
- Logged and isolated
- Can re-run build for failed services

#### Run Failures
- Detected via process exit
- Manual restart supported

---

## Architecture

```
local-orchestrator/
├── core/
│   ├── orchestrator.py
│   ├── executor.py
│   ├── git_manager.py
│   ├── logger.py
│   ├── state_manager.py
│   └── strategies/
│       ├── build_strategy.py
│       ├── run_strategy.py
│       └── log_strategy.py
├── cli/
│   └── main.py
├── config/
│   └── projects.yaml
├── logs/
└── README.md
```

---

## Configuration

### `projects.yaml`

```yaml
projects:
  - name: user-service
    path: /path/to/user-service
    branch: develop
    build_command: "npm run build"
    run_command: "npm start"
    env_file: .env

  - name: order-service
    path: /path/to/order-service
    branch: main
    run_command: "java -jar app.jar"
```

---

## Execution Flow

### 1. Start Session
- Initialize state
- Create session file

### 2. Git Phase
- Fetch and checkout branch
- Fail fast if invalid

### 3. Build Phase (Optional)
- Execute build commands in parallel
- Track success/failure

### 4. Run Phase
- Start processes in parallel
- Capture PID and logs

### 5. Monitoring
- Track process status using `poll()`
- Update state

### 6. Exit
- Stop all processes
- Delete session file (unless retained)

---

## State Management

### In-Memory Model
```python
{
  "user-service": {
    "status": "running",
    "pid": 12345
  }
}
```

### Responsibilities
- Track lifecycle
- Sync with session file
- Provide runtime status

---

## CLI Interface

### Commands
```
orchestrator start
orchestrator build
orchestrator run
orchestrator status
orchestrator stop
orchestrator restart <service>
orchestrator logs <service>
```

---

## Process Management

### Execution Model
- Each service runs as independent OS process
- Managed via `subprocess.Popen`

### Parallelism
- Implemented using `ThreadPoolExecutor`

### Monitoring
- Use `process.poll()` to detect failures

---

## Error Classification

- **GIT_ERROR**
- **BUILD_ERROR**
- **RUN_ERROR**

---

## Future Enhancements

- Auto-restart policies
- Session resume
- Interactive CLI mode
- Plugin system for strategies
- Health check support (port/process based)

---

## Key Advantages

✅ Lightweight (no Docker)
✅ Fast startup and execution
✅ Language-agnostic service support
✅ Strong logging and observability
✅ Failure isolation
✅ CLI + library flexibility

---

## Notes

- Assumes services can run independently
- No dependency graph enforced
- Logs are primary debugging tool
- Designed for local developer environments

---

## To Do

- Define project setup flow
- Implement executor module
- Implement state manager
- Add CLI argument parsing

