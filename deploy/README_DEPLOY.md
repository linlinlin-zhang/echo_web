# Ubuntu 22.04 Deployment (No Domain)

This repo is already the web project root.

This folder includes two scripts:

- `bootstrap_ubuntu.sh`: first-time setup and deploy
- `update_ubuntu.sh`: pull latest code and redeploy

## 1) Coexist with an existing website (recommended)

If your server already serves another site on port 80, deploy this app on port 8080:

```bash
sudo mkdir -p /var/www
sudo chown -R $USER:$USER /var/www

git clone https://github.com/linlinlin-zhang/echo_web.git /var/www/echo_web
cd /var/www/echo_web
chmod +x deploy/bootstrap_ubuntu.sh deploy/update_ubuntu.sh
APP_ROOT=/var/www/echo_web BRANCH=main SERVER_IP=39.108.139.62 APP_PORT=3001 NGINX_PORT=8080 REMOVE_NGINX_DEFAULT_SITE=false bash deploy/bootstrap_ubuntu.sh
```

Open:

- `http://39.108.139.62:8080`

## 2) Replace old site on port 80 (optional)

Only use this if you want to switch traffic to this app:

```bash
APP_ROOT=/var/www/echo_web BRANCH=main SERVER_IP=39.108.139.62 APP_PORT=3000 NGINX_PORT=80 REMOVE_NGINX_DEFAULT_SITE=true bash deploy/bootstrap_ubuntu.sh
```

Open:

- `http://39.108.139.62`

## 3) Update deployment after new commits

```bash
cd /var/www/echo_web
APP_ROOT=/var/www/echo_web BRANCH=main bash deploy/update_ubuntu.sh
```

## Optional variables

You can override defaults:

- `APP_NAME` (default: `echo-web`)
- `APP_PORT` (default: `3000`)
- `APP_ROOT` (default: `/var/www/echo`)
- `PROJECT_SUBDIR` (default: `web`, auto-falls back to `APP_ROOT` when `package.json` exists there)
- `BRANCH` (default: `main`)
- `NODE_MAJOR` (default: `20`)
- `NGINX_PORT` (default: `80`)
- `REMOVE_NGINX_DEFAULT_SITE` (default: `false`)
- `REPO_URL` (required only when `APP_ROOT` is not already a git repository)
