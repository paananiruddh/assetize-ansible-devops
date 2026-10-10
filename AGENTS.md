# AGENTS.md — Assetize fork of ibm-mas/ansible-devops

> **HARD RULE — never contribute AI-agent files upstream.**
> This file, `CLAUDE.md`, `.assetize/` and any other AI-agent file (`.claude/`,
> `.cursor*`, `.windsurf*`, `.aider*`, `GEMINI.md`, `.github/copilot-instructions.md`,
> agent prompts/rules/skills/memory/plans we add) are **fork-only**. Never include them
> in a commit, branch or PR targeting `ibm-mas/*`. Upstream's own `AGENT_INSTRUCTIONS.md`
> and `.bob/` belong to IBM — leave them alone unless the upstream change needs it.
> Enforced by a pre-push hook: run `.assetize/install-hooks.sh` once per clone. Never
> bypass it with `--no-verify`.

## What this repo is
- Fork of [`ibm-mas/ansible-devops`](https://github.com/ibm-mas/ansible-devops) (default
  branch `master`). Published upstream as the Ansible collection `ibm.mas_devops`.
- These roles do the real work for the MAS CLI (fork:
  `softwarecomparereview/assetize-mas-cli`, see its `AGENTS.md`). Every CLI Tekton task runs
  `ansible-playbook ibm.mas_devops.run_role` with `ROLE_NAME=<role>`.
- **Variable chain:** CLI param → Tekton param → UPPERCASE env var → role default
  `lookup('env', 'X')` in `roles/<role>/defaults/main.yml`.
- Upstream's own agent guide: `AGENT_INSTRUCTIONS.md`.

## Map (`ibm/mas_devops/`)
- `roles/` (~95 roles): dependencies (`db2`, `mongodb`, `kafka`, `sls`, `cert_manager`,
  `cp4d`, …), MAS core (`suite_install`, `suite_config`, `suite_verify`, …), apps
  (`suite_app_install`, `suite_app_config`, …), backup/restore (`suite_backup`,
  `suite_app_backup`, `suite_restore`, `suite_app_restore`, …), OCP provisioning.
- `playbooks/`: entry playbooks (incl. `run_role.yml`).
- `common_tasks/`, `common_vars/`, `plugins/` (custom modules/filters).
- Role docs: `roles/<role>/README.md` (mirrored into `docs/`).

## Local development
```bash
yamllint -c yamllint.yaml ibm/mas_devops/roles/<role>   # lint (yamllint lives in the CLI fork venv)
ansible-playbook --syntax-check ...                      # syntax checks; real runs need a cluster
```
- Cross-repo changes use the **same branch name** in cli / python-devops / ansible-devops.
- Commit/PR titles use `[patch]` / `[minor]` / `[major]`.
