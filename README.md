# matrix.developers.italia.it

This repository contains the configuration for deploying a Matrix chat server on `matrix.developers.italia.it`.

The configuration uses the [matrix-docker-ansible-deploy playbook](https://github.com/spantaleev/matrix-docker-ansible-deploy).

## Prerequisites

- Git
- Ansible
- [just](https://github.com/casey/just)
- Open the ports listed in [prerequisites.md](https://github.com/spantaleev/matrix-docker-ansible-deploy/blob/master/docs/prerequisites.md)

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
3. **Get the vault file with secrets:**

   Get [vault.yml](https://github.com/teamdigitale/dpt-bogus/blob/main/matrix.developers.italia.it/)
   and save it to `inventory/host_vars/matrix.developers.italia.it/vault.yml` (you'll need access to the repo).

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
   ansible-playbook -i inventory/hosts setup.yml --ask-vault-pass --tags=install-all,ensure-matrix-users-created,start
   ```

## Maintenance

### ⏰ Update the playbook (once per month)

This should be done periodically as to not fall behind too much. Small frequent updates are
less risky and more likely to be rollback-able than big ones.

Nevertheless, updates can break things **proceed carefully and follow these steps:** ⚠️

1. **Move to the playbook submodule directory:**
   ```console
   cd matrix-docker-ansible-deploy/
   ```

2. **Check for changes:**

   Check for the changes and investigate if there's something potentially risky or
   not backwards compatible.

   ```console
   git fetch origin
   git shortlog HEAD..origin/master
   ```

4. **Check the CHANGELOG:**

   Check [the CHANGELOG](https://github.com/spantaleev/matrix-docker-ansible-deploy/blob/master/CHANGELOG.md) for
   breaking changes and adapt the local variables if there are any.


5. **Update the playbook submodule:**

   Move to the repo root:
   ```console
   cd ..
   ```

   Sync the submodule to the latest commit and push:
   ```console
   git submodule update --remote
   git commit -am "chore: update playbook to latest commit"
   git push
   ```

### 🔄 Update the server

1. **Pull the repo:**
   ```console
   git pull --recurse-submodules
   ```

2. **Move to the playbook directory:**
   ```console
   cd matrix-docker-ansible-deploy/
   ```

3. **Update the roles:**
   ```console
   just roles
   ```

4. **Update the installation :**

   Beware: this will result in a short downtime because **the services will be restarted**. ⚠️

   ```console
   just setup-all --ask-vault-pass
   ```
