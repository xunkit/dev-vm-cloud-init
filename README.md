# dev-vm-cloud-init

Cloud-init template for spinning up disposable Ubuntu dev VMs with [Multipass](https://canonical.com/multipass), with software install handed off to Ansible.

Companion to [Making Ephemeral Dev VMs with cloud-init](https://xuankiet.com/blog/making-ephemeral-dev-vms-with-cloud-init).

## Requirements

- [Multipass](https://canonical.com/multipass/install) installed
- An SSH public key (typically `~/.ssh/id_ed25519.pub`)

## Quick start

1. Open `cloud-init.yaml` and paste your SSH public key into the `ssh_authorized_keys` field.

2. Launch:

```
   multipass launch --name dev --cloud-init cloud-init.yaml --cpus 2 --memory 4G --disk 20G
```

3. Connect:

```
   ssh ubuntu@dev.mshome.net
```

   (`dev.mshome.net` works out of the box on Hyper-V. On other backends, run `multipass info dev` to get the VM's IP.)

## What's in here

- **`cloud-init.yaml`** — bootstraps the user, SSH key, and Ansible itself.
- **`ansible/setup.yml`** — the playbook that installs the toolchain on first boot.

The playbook installs:

- **Container & orchestration**: Docker, kubectl, Helm
- **Infrastructure**: Terraform
- **Shell essentials**: git, curl, wget, jq, yq, unzip, tree, htop, tmux
- **Better CLI defaults**: bat, ripgrep, fd-find

## Tear down

```
multipass delete dev --purge
```
