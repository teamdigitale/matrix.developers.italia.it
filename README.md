# matrix.developers.italia.it

This repository contains the configuration for deploying a Matrix chat server on `matrix.developers.italia.it`.

The configuration uses the [matrix-docker-ansible-deploy playbook](https://github.com/spantaleev/matrix-docker-ansible-deploy).

## Prerequisites

- Git
- Ansible
- [just](https://github.com/casey/just)

## Getting Started

1. **Clone this repository:**
   ```console
   git clone --recurse-submodules git@github.com:teamdigitale/matrix.developers.italia.it.git
   ```

2. **Link the `inventory` directory:**
   ```console
   cd matrix.developers.italia.it/
   ln -s ../inventory matrix-docker-ansible-deploy/inventory
   ```

## First deployment

Your SSH key must be added to `authorized_keys` of root on the server.

1. **Move to the playbook directory:**
   ```console
   cd matrix-docker-ansible-deploy/
   ```

2. **Update the roles:**
   ```console
   just roles
   ```

3. **Install and start everything:**
   ```console
   ansible-playbook -i inventory/hosts setup.yml --tags=install-all,ensure-matrix-users-created,start
   ```

## Maintenance

### Updating

1. **Move to the playbook directory:**
   ```console
   cd matrix-docker-ansible-deploy/
   ```

2. **Update the roles:**
   ```console
   just roles
   ```

3. **Update the installation :**
   ```console
   just setup-all
   ```
