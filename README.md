# Homelab AWX Ansible Deployment Repository

This repository stores standard playbooks, roles, and collections used by the homelab AWX instance. It uses standard layout practices to automatically sync dependencies into AWX.

## Overview

### Credentials
**Note:** The AWX built-in credential manager handles all the secrets (such as execution credentials, machine passwords, vault, API tokens) and provides them dynamically at playbook execution time. Avoid hardcoding plaintext credentials anywhere in the repository. Provide them securely via AWX.

### Directory Structure

- `/playbooks/`: Holds all top-level playbook `.yml` files. These are typically selected as the main job templates in AWX.
- `/roles/`: Custom Ansible roles that perform specific functions. Add custom tasks, defaults, and templates here. Place external role requirements in `requirements.yml`.
- `/collections/`: Collection definitions in `requirements.yml`. AWX will automatically parse these and install necessary collections (e.g. `community.general`, `ansible.windows`) upon project sync.
- `/inventory/`: For static inventories. In AWX, it is common to sync inventories using an inventory source from this repository or maintain it dynamically in AWX itself. Since you use the built-in credential manager, you will likely construct the inventory directly in AWX or provide static definition here.
- `/group_vars/` and `/host_vars/`: Global configuration variables applied automatically across playbooks.

## Getting Started / Testing

This repo includes default testing templates inside the `/playbooks/` folder:

1. **`test_debug.yml`**: A simple localhost verification. Runs quickly on the AWX controller itself. Confirms Git sync works.
2. **`test_ping.yml`**: Uses `ansible.builtin.ping` (or `win_ping`). Good for verifying that AWX can successfully connect to the target endpoints using credentials stored in AWX.
3. **`test_facts.yml`**: Gathers system facts on targets. Great for debugging connection capabilities and Python parsing dependencies.

To verify the setup:
1. In AWX, add a new **Project** pointing to this repository.
2. Select it to **Update on Launch** to pull `requirements.yml`.
3. Create a **Job Template** pointing to the Project and select one of the test playbooks.
4. Attach your target inventory and credentials to the template, and hit Launch!
