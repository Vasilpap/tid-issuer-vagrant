# TID Issuer Vagrant Lab

Local multi-VM environment used for TID Issuer deployment and Ansible testing.

## Topology

Default (`TID_SCENARIO=split`):

- `control` (`192.168.56.10`): control node
- `api-server` (`192.168.56.101`): API host
- `infra-server` (`192.168.56.102`): infrastructure host
- `front-server` (`192.168.56.103`): frontend host

Docker-env scenario (`TID_SCENARIO=docker-env`):

- `control` (`192.168.56.10`): control node
- `docker-env` (`192.168.56.103`): single host for full-stack Docker Compose deployment

All VMs use `ubuntu/jammy64`.

## Prerequisites

- Vagrant
- VirtualBox
- vagrant-hostmanager plugin (used by this `Vagrantfile`)

## Usage

```bash
vagrant up
vagrant status
vagrant ssh control
```

Run the single-VM docker-env scenario:

```bash
TID_SCENARIO=docker-env vagrant up
TID_SCENARIO=docker-env vagrant status
TID_SCENARIO=docker-env vagrant ssh control
```

Scenario selection uses VM `autostart` flags. The non-selected scenario VMs remain defined but are not started by default.

When switching scenarios, stop or destroy VMs from the previous one first to avoid IP conflicts (`front-server` and `docker-env` both use `192.168.56.103`).

Destroy lab:

```bash
vagrant destroy -f
```

For the docker-env scenario:

```bash
TID_SCENARIO=docker-env vagrant destroy -f
```

## Deploy with Ansible

From the `control` VM (inside the `tid-issuer-ansible` directory):

- Split scenario:

```bash
ansible-playbook -i hosts.yaml playbooks/site.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
```

- Docker-env scenario:

```bash
ansible-playbook -i hosts.yaml playbooks/docker-env.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
```

## SSH Behavior

`scripts/key.sh` creates a control-node keypair and propagates the public key to the other nodes for passwordless SSH between lab machines.

## Notes

- `.vagrant/` and generated key artifacts are gitignored.
- This repo provides infrastructure only; deployment is handled by `tid-issuer-ansible`.

## Architecture and Diagrams

Cross-repository diagrams (API DTO class diagram, system architecture, deployment diagram) and the complete technology inventory are documented in `../WORKSPACE_ARCHITECTURE.md`.
