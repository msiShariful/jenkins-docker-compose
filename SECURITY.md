# Security Policy

## Supported Versions

This project is actively maintained on the `master` branch. Security fixes are applied to the latest version only.

| Version | Supported |
| ------- | --------- |
| latest (`master`) | :white_check_mark: |
| older commits | :x: |

## Reporting a Vulnerability

If you discover a security vulnerability, please report it privately:

- Use GitHub's [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability) ("Report a vulnerability" under the **Security** tab), **or**
- Email the maintainer at **nipon@6amtech.com**.

Please include steps to reproduce, affected versions, and potential impact. You can expect an initial response within **5 business days**. Please do not open a public issue for security reports.

## Security Considerations

- The Jenkins container runs with `privileged: true` as `root` and mounts the host Docker socket (`/var/run/docker.sock`). This grants the container effective root-level control over the host — run it only on trusted hosts and networks.
- Do not expose port 8080 to the public internet without authentication and HTTPS; place Jenkins behind a reverse proxy with TLS.
- Complete the setup wizard and create a strong admin account on first launch. Keep the `jenkins/jenkins:lts` image and all plugins updated, as plugins are a common source of vulnerabilities.
