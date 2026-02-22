# TID Issuer Vagrant Lab

Local multi-VM environment used for TID Issuer deployment and Ansible testing.

## Topology

- `control` (`192.168.56.10`): control node
- `api-server` (`192.168.56.101`): API host
- `infra-server` (`192.168.56.102`): infrastructure host
- `front-server` (`192.168.56.103`): frontend host

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

Destroy lab:

```bash
vagrant destroy -f
```

## SSH Behavior

`scripts/key.sh` creates a control-node keypair and propagates the public key to the other nodes for passwordless SSH between lab machines.

## Notes

- `.vagrant/` and generated key artifacts are gitignored.
- This repo provides infrastructure only; deployment is handled by `tid-issuer-ansible`.
