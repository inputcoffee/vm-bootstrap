# vm-bootstrap

A bootstrap script for fresh Ubuntu 24.04 servers. Sets up a secure, Docker-ready server in one command.

## What it does

- Updates all system packages
- Creates a `deploy` user and copies root's SSH keys to it
- Hardens SSH (disables root login and password authentication)
- Configures `ufw` firewall (allows SSH, HTTP, HTTPS)
- Enables automatic security updates
- Installs Docker and Docker Compose (from Docker's official repo)
- Installs Git
- Installs and enables fail2ban

## Usage

On a fresh Ubuntu 24.04 server, run as root:

```bash
curl -fsSL https://raw.githubusercontent.com/inputcoffee/vm-bootstrap/main/bootstrap26.sh | bash
```

## After running

1. **Open a new terminal** and verify you can SSH as the deploy user before closing your root session:
   ```bash
   ssh deploy@YOUR_SERVER_IP
   ```

2. Once confirmed, update your `~/.ssh/config` to use `deploy` as the default user for this server.

3. Root login is now disabled — all future SSH access is via the `deploy` user.

## SSH config entry

Add this to `~/.ssh/config` on each of your computers:

```
Host yourservername
  HostName YOUR_SERVER_IP
  User deploy
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
```

## Known issues

- Tested on Ubuntu 24.04 LTS only
- Uses `ssh` not `sshd` for the service name (Ubuntu 24.04 convention)
- Docker is installed from Docker's official apt repo, not Ubuntu's default repo

## Next steps after bootstrap

Clone your project repo and start your services:

```bash
su - deploy
git clone https://github.com/inputcoffee/n8n-server.git
cd n8n-server
cp .env.example .env
nano .env
docker compose up -d
```
