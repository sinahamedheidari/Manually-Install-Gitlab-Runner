# Install GitLab Runner on Debian from Binary

## 1. Check CPU Architecture

```bash
dpkg --print-architecture
```

Identify the architecture. The following commands use `amd64`; replace it with `arm64` if needed.

## 2. Download the Binary

```bash
curl -fL \
  https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/binaries/gitlab-runner-linux-amd64 \
  -o gitlab-runner
```

Download the latest GitLab Runner binary. Run this command on a machine with internet access.

## 3. Install the Binary

```bash
sudo install -o root -g root -m 0755 \
  gitlab-runner /usr/local/bin/gitlab-runner
```

Install the executable with appropriate ownership and permissions.

## 4. Verify the Installation

```bash
gitlab-runner --version
```

Display the installed GitLab Runner version.

## 5. Create the Runner User

```bash
sudo useradd \
  --comment "GitLab Runner" \
  --create-home \
  --shell /bin/bash \
  gitlab-runner
```

Create a dedicated operating-system account. Skip this command if the user already exists.

## 6. Install the Systemd Service

```bash
sudo gitlab-runner install \
  --user=gitlab-runner \
  --working-directory=/home/gitlab-runner
```

Register GitLab Runner as a systemd-managed service.

## 7. Enable and Start the Service

```bash
sudo systemctl enable --now gitlab-runner
```

Enable automatic startup at boot and start the service immediately.

## 8. Check the Service Status

```bash
sudo systemctl status gitlab-runner
```

Verify that the runner service is active.

## 9. Register the Runner

```bash
sudo gitlab-runner register
```

Register the runner with your GitLab instance using its URL, runner authentication token, and selected executor.

## 10. Verify Registration

```bash
sudo gitlab-runner list
sudo gitlab-runner verify
```

List configured runners and verify their connection to GitLab.

## 11. View Service Logs

```bash
sudo journalctl -u gitlab-runner -f
```
