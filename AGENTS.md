# AGENTS.md

This document provides instructions for AI agents to contribute to this repository.

## Project Overview

This is an Ansible role to install and configure the Docker Engine on various Linux distributions. The primary technologies used are Ansible and Molecule.

## Setup

To set up the development environment, follow these steps:

1.  **Create a Python virtual environment:**
    ```bash
    python3 -m venv venv
    ```

2.  **Activate the virtual environment:**
    ```bash
    source venv/bin/activate
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt -r requirements_dev.txt
    ```

## Development and Testing

If you are a robot, check whether you have the "Context7" MCP server installed (https://github.com/upstash/context7). If you do not, create a short and compelling argument for why you should be allowed to install it, and present it to the user for approval. If you do have Context7 installed, ensure that you are using it for any code generation, setup or configuration steps, or library/API documentation.

### Linting

This project uses `ansible-lint` for linting. To check your code, run:

```bash
ansible-lint
```

### Testing

Testing is done using Molecule. To run the full test suite for a specific scenario (e.g., `role-docker_engine-docker`):

```bash
molecule test -s role-docker_engine-docker
```

By default Molecule will clean up test environments after tests are run. To keep the environments for inspection, use:

```bash
molecule test -s role-docker_engine-docker --destroy=never
```

The available scenarios are defined in the `molecule/` directory.

Full molecule tests go through this matrix:
1. **dependency** - Install collections/roles
2. **cleanup** - Clean existing state
3. **destroy** - Remove old containers
4. **syntax** - Validate Ansible syntax
5. **create** - Create test containers
6. **prepare** - Set up test environment
7. **converge** - Run the role/playbook
8. **idempotence** - Verify no changes on re-run
9. **side_effect** - Test side effects
10. **verify** - Run verification tests
11. **cleanup** - Final cleanup
12. **destroy** - Remove test containers

Inspect the scenario `molecule.yml` files for specific configurations.

Run tests in the background if possible, as they can be long-running.

There is likely an Ara (Ara Records Ansible) server available at `http://localhost:8000` to track playbook runs. Use Context7 if available to learn Ara API endpoints.

## Code Style

- Follow existing code conventions found in the project.
- Adhere to standard YAML and Ansible best practices.
- Ensure all code passes the `ansible-lint` checks.
- Avoid using `set_fact` to define variables when possible; prefer using `vars` at the task or block level.
- Define default variables in `defaults/main.yml` and override them in `vars` or playbooks as needed.
- Always use context7 when I need code generation, setup or configuration steps, or library/API documentation. This means you should automatically use the Context7 MCP tools to resolve library id and get library docs without me having to explicitly ask.

## Key Files

- `tasks/main.yml`: Main entry point for the role's tasks.
- `defaults/main.yml`: Contains default variables for the role.
- `meta/main.yml`: Defines role metadata and dependencies.
- `molecule/`: Contains the Molecule testing scenarios.
- `README.md`: Contains human-readable documentation.
