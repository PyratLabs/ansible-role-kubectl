# Ansible Role: kubectl

Ansible role for installing kubectl.

[![Build Status](https://www.travis-ci.org/PyratLabs/ansible-role-kubectl.svg?branch=master)](https://www.travis-ci.org/PyratLabs/ansible-role-kubectl)

## Requirements

This role has been tested on Ansible 2.7.0+ against the following Linux Distributions:

  - Amazon Linux 2
  - CentOS 8
  - CentOS 7
  - Debian 10
  - Fedora 29
  - Fedora 30
  - Fedora 31
  - Ubuntu 18.04 LTS

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

## Role Variables


| Variable                          | Description                                                                  | Default Value    |
|-----------------------------------|------------------------------------------------------------------------------|------------------|
| `kubectl_version`                 | Use a specific version of kubectl, eg. `1.17.0`. Specify `false` for latest. | `false`          |
| `kubectl_install_os_dependencies` | Allow role to install OS dependencies.                                       | `false`          |
| `kubectl_install_dir`             | Installation directory for kubectl.                                          | `$HOME/bin`      |
| `kubectl_projects_dir`            | Directory to put k8s projects. Specify `false` to skip.                      | `$HOME/projects` |
| `kubectl_projects`                | List of K8s projects to be cloned with `git`. See notes.                     | _NULL_           |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for installing to single user:

```yaml
- hosts: control_hosts
  roles:
     - { role: xanmanning.kubectl, kubectl_version: 1.17.0 }
```

Example playbook for installing the latest kubectl version globally:

```yaml
---
- hosts: control_hosts
  become: true
  vars:
    kubectl_install_os_dependencies: true
    kubectl_install_dir: /opt/kubectl/bin
    kubectl_projects_dir: /opt/kubectl/projects
  roles:
    - role: xanmanning.kubectl
```

### Note about `kubectl_projects`

This is a list of git repositories to be cloned into the projects directory.
If this is empty, no projects will be cloned.

Below is an example of a project:

```yaml
kubectl_projects:
    - name: kubernetes-examples                       # Directory name to clone into
      repo: git@github.com:kubernetes/examples        # Repository to clone
      update_repo: true                               # Always update local copy of repo
      version:  master                                # Check out this version of the repo
      force: false                                    # Discard any existing working copy of the repo
      key_file: "{{ ansible_user_dir }}/.ssh/id_rsa"  # Key file to use to clone the repo
      recursive: true                                 # Include submodules in clone
```

## License

[BSD 3-clause](LICENSE.txt)

## Author Information

[Xan Manning](https://xanmanning.co.uk/)
