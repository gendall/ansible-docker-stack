# Docker Stack

A role to deploy a Docker Stack from a docker-compose.yml file in the playbook directory.

## Usage

The Docker registry defaults to GitHub Package Registry, but can be changed by passing a new hostname (e.g. docker.io) as the `docker_registry` variable. Credentials are password as environment variables:

```
DOCKER_USERNAME=mydockeraccount
DOCKER_PASSWORD=ah32hg3hrgrmbds
```

If a [Vaultenv](https://github.com/channable/vaultenv) secrets file named `secrets.conf` exists in the playbook directory, the role copies this to the server and creates a `.env` file containing [Vault](https://www.vaultproject.io/) credentials if passed as environment variables:

```
VAULT_ADDR=https://vault.company.com:443
VAULT_TOKEN=s.g23gberb32b322b23b4
```

The role will create Docker Secrets from any files found in `{{ inventory_dir }}/secrets`. Due to limitations of Docker Secrets rotation, the role removes and re-creates the stack on each update. This is so secrets are not in use when updated.

If a `PROJECT` environment variable is set, it will be used as the stack name. If `TAG` and `ENV` are set, they will be passed through to the Swarm.

## Usage

Include this role in a playbook using a [requirements.txt](https://galaxy.ansible.com/docs/using/installing.html#installing-multiple-roles-from-a-file) file.

### Example playbook

```yaml
- hosts: manager[0]
  roles:
    - docker-stack
```

## Deployment

This role will be automatically built and deployed to [Ansible Galaxy](https://galaxy.ansible.com/gendall) when a [Semver](https://semver.org) tag is pushed to the repo.
