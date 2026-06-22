# jenkins-docker-compose

> One-command Jenkins LTS setup with Docker Compose — persistent home volume and Docker-in-Jenkins support for CI/CD pipelines.

![Jenkins](https://img.shields.io/badge/Jenkins-LTS-D24939?logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Docker Compose](https://img.shields.io/badge/Docker%20Compose-2496ED?logo=docker&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)

A minimal, ready-to-run [Jenkins](https://www.jenkins.io/) deployment using Docker Compose. It runs the official `jenkins/jenkins:lts` image with a persistent home volume and mounts the host Docker socket so Jenkins pipelines can build and run Docker containers — ideal for getting a CI/CD server up in a single command.

## Features

- **Persistent data** — Jenkins configuration, jobs, and plugins are stored in the named `jenkins_home` volume and survive container restarts and recreation.
- **Docker-in-Jenkins** — the host Docker socket (`/var/run/docker.sock`) is mounted into the container, letting pipelines run `docker` commands directly.
- **Build flexibility** — the container runs as `root` with `privileged: true` for maximum compatibility with build tooling.
- **JNLP agent support** — port `50000` is exposed for connecting inbound (JNLP) build agents.
- **Setup wizard enabled** — first launch walks you through the standard Jenkins setup wizard.

## Prerequisites

- [Docker Engine](https://docs.docker.com/engine/install/)
- The [Docker Compose plugin](https://docs.docker.com/compose/install/) (`docker compose`)

## Quick Start

1. Start Jenkins:

   ```bash
   docker compose up -d
   ```

2. Open the web UI at [http://localhost:8080](http://localhost:8080).

3. Retrieve the initial admin password to unlock Jenkins:

   ```bash
   docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
   ```

4. Follow the setup wizard to install plugins and create your admin user.

## Ports

| Port    | Purpose                  |
| ------- | ------------------------ |
| `8080`  | Jenkins Web UI           |
| `50000` | JNLP (inbound) agents    |

## Configuration

The deployment is defined entirely in `docker-compose.yml`:

- **`JAVA_OPTS=-Djenkins.install.runSetupWizard=true`** — enables the Jenkins setup wizard on first launch.
- **`jenkins_home` volume** — mounted at `/var/jenkins_home` to persist all Jenkins data across container lifecycles.
- **`/var/run/docker.sock` mount** — exposes the host Docker daemon to Jenkins so pipelines can build and run containers.

## Managing the container

- View logs:

  ```bash
  docker compose logs -f jenkins
  ```

- Stop and remove the container:

  ```bash
  docker compose down
  ```

  Your data is preserved in the `jenkins_home` volume, so the next `docker compose up -d` resumes where you left off.

## Security

This setup runs Jenkins with `privileged: true`, as `root`, and with the host Docker socket mounted — which effectively grants the container root-level control over the host. Run it only on trusted hosts and networks, and do not expose it to the public internet without authentication and HTTPS.

See [SECURITY.md](SECURITY.md) for the full security policy and how to report vulnerabilities.

## Contributing

Issues and pull requests are welcome. Feel free to open an issue to report a bug or suggest an improvement.

## License

Released under the [MIT License](LICENSE).
