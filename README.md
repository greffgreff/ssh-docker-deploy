# SSH Docker Deploy GitHub Action

This GitHub Action allows you to deploy a Docker container to any remote server via SSH. It supports any Docker registry and allows triggering repository dispatch events for integration with other workflows.

## Features

- Deploys a Docker container to a remote server via SSH.
- Supports any Docker registry.
- Automatically validates Docker image format.
- Stops and removes existing Docker containers (if they exist).
- Optionally triggers a repository dispatch event after deployment.
- Can be used with any cloud service or self-hosted machine with SSH access.

## Inputs

| Name               | Description                                                                  | Required | Default        |
|--------------------|------------------------------------------------------------------------------|----------|----------------|
| `target`           | Docker image to deploy (e.g., `ghcr.io/owner/repository:tag`)                | `true`   |                |
| `ssh_private_key`  | SSH private key for accessing the remote server                              | `true`   |                |
| `ssh_host`         | Remote server host (IP address or domain)                                    | `true`   |                |
| `ssh_user`         | Username for SSH login                                                       | `true`   |                |
| `docker_username`  | Username for Docker registry login                                           | `true`   |                |
| `docker_token`     | Access token for Docker registry login                                       | `true`   |                |
| `container_name`   | Name of the Docker container                                                 | `true`   | `container`    |
| `container_port`   | Port to run the container on the remote server                               | `false`  | `8080`         |
| `trigger_event`    | Whether to trigger a deploy event                                            | `false`  | `false`        |
| `event_name`       | Custom event name to trigger if `trigger_event` is enabled                   | `false`  | `docker-deploy`|

## Usage

Below is an example of how to use this action in your workflow.

### Example Workflow

```yaml
name: Deploy Docker Image

on:
  repository_dispatch:
    types: [new-production-image, rollback-production-image]

jobs:
  deploy:
    name: Deploy Docker Image via SSH
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy Docker Image
        uses: your-repo/ssh-docker-deploy@v1
        with:
          target: 'ghcr.io/owner/repository:tag'
          ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }}
          ssh_host: 'your-remote-host.com'
          ssh_user: 'your-username'
          docker_username: ${{ secrets.DOCKER_USERNAME }}
          docker_token: ${{ secrets.DOCKER_TOKEN }}
          container_name: 'my-container'
          container_port: '8080'
          trigger_event: 'true'
          event_name: 'deploy-to-production'
```
