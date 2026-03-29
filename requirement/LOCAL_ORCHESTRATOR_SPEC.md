# Local Distributed Systems Orchestrator

| Field | Value |
|-------|-------|
| **Author** | Ritik Agrawal |
| **Version** | 1.0.0 |
| **Created At** | 2026-03-26 |
| **Updated At** | 2026-03-26 |

## Overview
A local development orchestration system for managing multiple microservices with a modern web UI, supporting parallel builds, selective execution, and failure recovery.

---

## Requirements

### Core Functionality

#### 1. Project Selection
- UI to select multiple projects from a list
- Checkbox-based selection interface
- Support for running selected projects only

#### 2. Branch & Environment Configuration
- **Branch Selection**:
  - Individual branch specification per project
  - System handles git checkout before build
  
- **Environment Files**:
  - Specify env file path per project during setup
  - UI input for custom env file paths during setup

#### 3. Build Phase
- **Dockerfile Discovery**: Automatic or manual path specification (FIND-DOCKERFILE)
- **Parallel Builds**: Build n projects simultaneously for speed
- **Build Failure Handling**:
  - Print detailed logs for debugging
  - Failure of one project doesn't affect others
  - Track build status per project
  - **Resume Capability**: 
    - Already-built projects in current session skip rebuild
    - UI shows list of failed projects
    - Option to rebuild only failed projects
    - Loop until all succeed or user exits
  - **Session Reset**: Fresh restart on new session (explicit exit)

#### 4. Run Phase
- Triggered only after all selected projects build successfully
- **Parallel Execution**: Run all projects simultaneously
- **Run Failure Handling**:
  - Stream console logs for failed services
  - Failure of one doesn't affect others
  - UI reflects status of each project (running/failed)
  - Option to restart only failed projects
  - Real-time status updates

#### 5. Session Management
- Persist build/run state during active session
- "Exit" button to terminate session explicitly
- New session = fresh start (no cached state)

---

## Technical Approach

### Stack
- **UI**: Streamlit (Python-based, modern, fast development)
- **Orchestration**: Docker Compose + Python scripts
- **Container Management**: Docker
- **State Management**: JSON file for session persistence
- **Dockerfile Discovery**: Custom shell script

### Architecture

```
local-orchestrator/
├── ui/
│   └── app.py                    # Streamlit UI
├── orchestrator/
│   ├── builder.py                # Build logic with parallel execution
│   ├── runner.py                 # Run logic with parallel execution
│   ├── git_manager.py            # Git branch checkout logic
│   └── state_manager.py          # Session state persistence
├── scripts/
│   ├── find_dockerfile.sh        # Dockerfile discovery (FIND-DOCKERFILE)
│   ├── check_dependencies.sh     # Validate system dependencies
│   ├── install_dependencies.sh   # Install missing dependencies
│   └── setup.sh                  # Initial setup script
├── config/
│   ├── projects.yaml             # Project definitions & paths
│   └── session_state.json        # Build/run state tracking
├── docker-compose.template.yml   # Dynamic compose file generation
└── README.md                     # Setup instructions
```

### Key Components

#### 1. Streamlit UI (`ui/app.py`)
- Project selection (multi-select checkboxes)
- Branch configuration section:
  - Radio: "Same branch for all" with text input
  - Individual project branch inputs
- Environment file path inputs per project
- Action buttons: Build, Run, Rebuild Failed, Restart Failed, Exit
- Real-time status display with auto-refresh (2-5 seconds)
- Log viewer for failures

#### 2. Builder (`orchestrator/builder.py`)
- Parallel build execution using `docker-compose build --parallel`
- Build status tracking per project
- Failure isolation
- Resume logic (skip already-built projects)
- Log capture and display

#### 3. Runner (`orchestrator/runner.py`)
- Parallel container startup
- Health check monitoring
- Status tracking (running/failed/stopped)
- Selective restart capability
- Log streaming

#### 4. Git Manager (`orchestrator/git_manager.py`)
- Branch checkout before build
- Handle "same for all" vs project-specific logic
- Validation and error handling

#### 5. State Manager (`orchestrator/state_manager.py`)
- Persist session state to JSON
- Track: build status, run status, selected projects, branches, env paths
- Clear state on exit

#### 6. Dockerfile Discovery (`scripts/find_dockerfile.sh`)
- Search for Dockerfile in project directory
- Convention-based search: `./Dockerfile`, `./docker/Dockerfile`, etc.
- Return path or error

#### 7. Dependency Validation (`scripts/check_dependencies.sh`)
- Check Docker installation and daemon status
- Check Docker Compose installation and minimum version
- Check Python installation and minimum version (3.8+)
- Check Git installation
- Check pip availability
- Report all missing/outdated dependencies

#### 8. Dependency Installation (`scripts/install_dependencies.sh`)
- Auto-detect OS (Linux/macOS)
- Install Python 3.8+ if missing
- Install Docker if missing
- Install Docker Compose if missing
- Install pip if missing
- Install Python packages (streamlit, pyyaml, docker)
- Verify installations
- Handle errors with user guidance

### Configuration

#### `config/projects.yaml`
```yaml
projects:
  - name: user-service
    path: /path/to/user-service
    default_branch: main
    default_env: .env.user-service
  - name: order-service
    path: /path/to/order-service
    default_branch: main
    default_env: .env.order-service
```

#### `config/session_state.json`
```json
{
  "session_active": true,
  "selected_projects": ["user-service", "order-service"],
  "branch_config": {
    "same_for_all": true,
    "branch_name": "develop",
    "overrides": {}
  },
  "build_status": {
    "user-service": "success",
    "order-service": "failed"
  },
  "run_status": {
    "user-service": "running",
    "order-service": "stopped"
  }
}
```

---

## Development Timeline

### Phase 1: MVP (2-3 hours)
- Basic Streamlit UI with project selection
- Simple build button (sequential builds)
- Docker Compose integration
- Basic status display

### Phase 2: Core Features (2-3 hours)
- Branch selection logic
- Environment file configuration
- Parallel builds
- State management

### Phase 3: Failure Recovery (1-2 hours)
- Build failure handling
- Run failure handling
- Selective rebuild/restart
- Log streaming

**Total Estimated Time**: 6-8 hours

---

## Post-Development Enhancements

### Automated Dependency Management
To be implemented after core functionality is complete:

1. **System Validation** (`scripts/check_dependencies.sh`):
   - Verify Docker installation and running status
   - Verify Docker Compose with minimum version check
   - Verify Python 3.8+ installation
   - Verify Git installation
   - Verify pip availability
   - Generate detailed report of missing/outdated dependencies

2. **Automated Installation** (`scripts/install_dependencies.sh`):
   - OS detection (Linux distributions, macOS)
   - Automated installation of missing components
   - Version validation post-installation
   - User-friendly error messages and manual installation guidance
   - Rollback capability on installation failures

---

## Setup Requirements

### Prerequisites
- Docker & Docker Compose installed
- Python 3.8+
- Git
- Project repositories cloned locally

### Installation
```bash
cd /home/ritikagrawal/Documents/codebase/setup-repo

# Check if all dependencies are available
./scripts/check_dependencies.sh

# Install missing dependencies (if any)
./scripts/install_dependencies.sh

# Run setup
./scripts/setup.sh
```

### Usage
```bash
streamlit run ui/app.py
```

---

## Key Features Summary

✅ Beautiful modern UI (Streamlit)  
✅ Multi-project selection  
✅ Flexible branch configuration (same for all + overrides)  
✅ Custom env file paths  
✅ Parallel builds for speed  
✅ Parallel runs for efficiency  
✅ Build failure isolation & recovery  
✅ Run failure isolation & recovery  
✅ Resume from failures (no rebuild of successful projects)  
✅ Real-time status monitoring  
✅ Log streaming for debugging  
✅ Session persistence with explicit exit  
✅ Zero cost (all free tools)  
✅ Fast setup (< 1 hour for users)  
✅ Low development time (6-8 hours total)  

---

## Notes

- Dockerfile discovery uses convention-based search with fallback to manual specification
- Git operations happen before build phase
- Docker health checks determine "running" status
- Auto-refresh UI every 2-5 seconds for real-time updates
- Session state persists until explicit "Exit" action
- All builds and runs execute in parallel for maximum speed


## To Do
- Flow 
  - project setup flow