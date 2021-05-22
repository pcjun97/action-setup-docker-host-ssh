# Remote Docker Host (SSH)

Setup the connection to remote Docker daemon via SSH.
Adds private key, known hosts and ssh configuration file.
Optionally setups the DOCKER\_HOST environment variable.

## Input variables

See action.yml for more detailed information.

- `host` - Docker daemon host
- `user` - SSH user
- `key` - Content of SSH private key

Optional:

- `port` - SSH port, default to `22`
- `key_path` - Path to the SSH private key, default to `/home/runner/.ssh/id_rsa`
- `set_docker_host_env` - Whether the `DOCKER_HOST` env var should be updated,
default to `true`

## Output variables

- `docker-host` - The value of `DOCKER_HOST` in the format of
`ssh://<user>@<host>:<port>`

## Usage

Default - Configure env var `DOCKER_HOST` in every subsequent steps
to point to the Docker daemon host.

```yaml
- uses: pcjun97/action-setup-docker-host-ssh@master
  with:
    host: ${{ secrets.SSH_HOST }}
    port: ${{ secrets.SSH_PORT }}
    user: ${{ secrets.SSH_USER }}
    key: ${{ secrets.SSH_KEY }}

- run: |
  docker ps
```

Alternatively, set up the environment variable with per step basis.

```yaml
- uses: pcjun97/action-setup-docker-host-ssh@master
  id: docker-ssh
  with:
    host: ${{ secrets.SSH_HOST }}
    port: ${{ secrets.SSH_PORT }}
    user: ${{ secrets.SSH_USER }}
    key: ${{ secrets.SSH_KEY }}
    set_docker_host_env: 'false'

- run: |
    docker ps
  env:
    DOCKER_HOST: ${{ steps.docker-ssh.outputs.docker-host }}
```
