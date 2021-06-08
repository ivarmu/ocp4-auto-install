Tower Objects Deploy
=========

Ansible playbook to create Tower Objects related to Ephemeral OCP4 environments based on VMware virtualization.

Requirements
------------

This role is tested on Ansible 2.9.

Dependencies
------------
N/A

Encrypted variables
----------------
All encrypted variables should be placed at "vars/configure_tower_vault.yml"

```
cat < EOF > vars/configure_tower_vault.yml
TODO:
EOF
```

Encrypt with ansible-vault
```
ansible-vault create vars/configure_tower_vault.yml
ansible-vault encrypt vars/configure_tower_vault.yml
ansible-vault edit vars/configure_tower_vault.yml
```

Playbooks:
-----------------

- playbooks/config-tower.yml
```
$ ansible-playbook playbooks/config-tower.yml --tags credentials,       -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags groups,            -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags hosts,             -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags inventories,       -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags inventory_source,  -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags job_templates,     -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags ldap,              -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags list_jobs,         -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags organizations,     -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags projects,          -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags roles,             -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags settings,          -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags teams,             -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags users,             -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass
$ ansible-playbook playbooks/config-tower.yml --tags workflow_templates -e {orgs: "OR-AL-TO-O-SGOP-DEF-COF-[SEGURIDAD OPERATIVA]", vars: ../vars} --ask-vault-pass

```

Playbook's example execution:
----------------------------

Variables
----------

Variables Must be set in yaml files with follow structure:

```
vars/configure_tower_{{ entity }}.yml
```

Vars files example:
```
vars/
TODO:
```

Git configuration to diff vaulted files
=========

If you want to diff vaulted files, you must name them with the pattern `*_credentials.yml` configure some thing into your local git configuration.

Content to be added to your local config file for git `${HOME}/.gitconfig` (alias section is not required):
```ini
[diff "ansible-vault"]
  textconv = ansible-vault view
[merge "ansible-vault"]
  driver = ./ansible-vault-merge %O %A %B %L %P
  name = Ansible Vault merge driver
[alias]
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
lg = !"git lg1"
```

Already provided files:

- `${REPO_ROOT}/.gitattributes`:
  > ```
  > *_credentials.yml filter=ansible-vault diff=ansible-vault merge=binary
  > ```

- `${REPO_ROOT}/ansible-vault-merge`:
  > ```bash
  > #!/usr/bin/env bash
  > set -e
  >
  > ancestor_version=$1
  > current_version=$2
  > other_version=$3
  > conflict_marker_size=$4
  > merged_result_pathname=$5
  >
  > ancestor_tempfile=$(mktemp tmp.XXXXXXXXXX)
  > current_tempfile=$(mktemp tmp.XXXXXXXXXX)
  > other_tempfile=$(mktemp tmp.XXXXXXXXXX)
  >
  > delete_tempfiles() {
  >     rm -f "$ancestor_tempfile" "$current_tempfile" "$other_tempfile"
  > }
  > trap delete_tempfiles EXIT
  >
  > ansible-vault decrypt --output "$ancestor_tempfile" "$ancestor_version"
  > ansible-vault decrypt --output "$current_tempfile" "$current_version"
  > ansible-vault decrypt --output "$other_tempfile" "$other_version"
  >
  > git merge-file "$current_tempfile" "$ancestor_tempfile" "$other_tempfile"
  >
  > ansible-vault encrypt --output "$current_version" "$current_tempfile"
  > ```

Finally, and to simplify your life, you can create the file `${REPO_ROOT}/.vault_password` containing the vault password and configure your `ansible.cfg` like follows:
```
vault_password_file = ./.vault_password
```
This way, the `git diff` command will not ask for the vault password multiple times.

License
-------

GPLv3

Author Information
------------------
silvinux - silvio@redhat.com
iaragone - iaragone@redhat.com

