# Ansible Role Skeleton (Team Standard)

This repository provides a pre-configured skeleton to be used with `ansible-galaxy init` for creating standardized Ansible roles.
This is usefull for creating standardized Ansible roles for a team.

## Role Directory Structure (Scaffolding)

```
myrole/
├── defaults/            # Default variables (lowest precedence)
│   └── main.yml
├── vars/                # Higher-precedence variables
│   └── main.yml
├── tasks/               # Main task logic (entry point: main.yml)
│   ├── main.yml
│   └── install.yml
├── handlers/            # Handlers triggered by notify
│   └── main.yml
├── templates/           # Jinja2 template files for config rendering
│   └── example.conf.j2
├── files/               # Static files to be copied
├── meta/                # Role metadata and dependencies
│   └── main.yml
├── tests/               # Basic Ansible test playbook
│   └── test.yml
├── molecule/            # Molecule testing scenario
│   └── default/
│       ├── converge.yml
│       └── molecule.yml
├── .yamllint            # YAML linting rules
├── .ansible-lint        # Ansible linting configuration
├── README.md.j2         # Auto-rendered README template
└── README.md            # Human-readable role documentation
```

### Role Directory Usage Order

1. `defaults/` → baseline vars
2. `vars/` → override vars
3. `tasks/main.yml` → imports other tasks (e.g., `install.yml`)
4. `handlers/` → used via `notify:` in tasks
5. `templates/` and `files/` → used by `template:` or `copy:` module
6. `meta/` → defines role dependencies or platform support
7. `tests/`, `molecule/` → optional testing structure

---

### Role Metadata Note for Contributors

Before uploading a role to your project repository:
- Delete **unused** folders like `molecule/`, `tests/`, `templates/`, or `files/` if not applicable.
- Delete `.yamllint`, `.ansible-lint` if the project already has root-level linting configs.
- Delete `README.md.j2` if the rendered README.md is already present or not needed.
- Clean and trim down the content according to the role's actual purpose.

##  How to Use

###  Clone the Skeleton Repo

```bash
git clone https://gitlab.company.com/devops/ansible-role-skeleton.git ~/ansible-role-skeleton
```

###  Generate a New Role

From a local path:

```bash
ansible-galaxy init myrole --role-skeleton ~/ansible-role-skeleton/ --init-path ./roles
```

Or from Git:

```bash
ansible-galaxy init myrole \
  --role-skeleton https://gitlab.company.com/devops/ansible-role-skeleton.git \
  --init-path ./roles
```

Use inside an existing project:

```bash
cd your-ansible-project/
ansible-galaxy init myrole --role-skeleton ~/ansible-role-skeleton/ --init-path ./roles
```

## Role Skeleton Justfile Integration

Place this `Justfile` in your Ansible project root or update the existing one:

```makefile
# Flexible version (requires all inputs)
init-role ROLE_NAME ROLE_PATH SKELETON_REPO:
    ansible-galaxy init {{ROLE_NAME}} \
      --role-skeleton {{SKELETON_REPO}} \
      --init-path {{ROLE_PATH}}
    echo "Role '{{ROLE_NAME}}' created at {{ROLE_PATH}}/{{ROLE_NAME}}"

# Default-path version (hardcoded for project structure)
init-role-default ROLE_NAME:
    ansible-galaxy init {{ROLE_NAME}} \
      --role-skeleton ~/ansible-role-skeleton \
      --init-path ./roles
    echo "Role '{{ROLE_NAME}}' created in ./roles/"
```

Usage:

```bash
just init-role ROLE_NAME=harden_os
```

##  Role Skeleton Structuree What's Included

- Standardized directory layout for tasks, handlers, defaults, vars, templates
- Molecule test scenario for Docker
- Linting rules via `.yamllint` and `.ansible-lint`
- `README.md.j2` template (auto-rendered)
- Example Jinja2 config template
- Minimal test playbook

## Notes

- `README.md.j2` is rendered into `README.md` by `ansible-galaxy init`
- `templates/` contains example Jinja2 files. You may delete it if unused
- Works inside existing projects by pointing `--init-path` to `./roles`
