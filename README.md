# Deploy Docker container
This [action](https://docs.github.com/en/actions) pulls a Docker image (and any necessary files) to a remote server via an SSH connection, optionally runs additional commands on the server, and starts up the container(s).

## Inputs

## `host`

**Required** The host name or IP address of the server to which this action will connect via SSH to deploy the image.

## `user`

**Required** The username to use when connecting to the server via SSH.

## `target`

**Required** The absolute filepath of the directory to which necessary files (e.g., docker-compose.yml) will be transferred/synced on the server.

## `files`

**Required** A space-delimited list of files to be transferred/synced to the server.

## `ssh-port`

**Required** The SSH port (e.g., 22) to use for connecting to the server.

## `ssh-private-key`

**Required** The private key generated on the server, used to authenticate the SSH connection.

## `ssh-auth-sock`

**Optional** The SSH_AUTH_SOCK environment variable, used to authenticate the SSH connection.
**Default** '/tmp/ssh_agent.sock'

## `deploy-command`

**Optional** The command to run on the server (through an SSH connection) to deploy the container.
**Default** `set -a; source .env; set +a; docker compose pull && docker compose up -d`

## Example usage
```
uses: iacobfred/deploy-docker-image-action@v1
with:
  host: 'server.com'
  user: 'jacob'
  target: '/var/www/server.com'
  files: |
    .env
    docker-compose.yml
    .config/nginx.conf
  ssh-port: '22'
  ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
  deploy-command: |
    set -a; source .env; set +a; 
    docker compose pull && docker compose up -d
```
