# Cloudflared-for-LXC
This is the steps to use Cloudflared for LXC (Linux Container) / PCT (Proxmox Container)

## Installation

Run all this command below as a @root

1. **Repo Installation**

-  Add GPG Cloudflared

```bash
mkdir -p --mode=0755 /usr/share/keyrings
curl -fsSL https://pkg.cloudflare.com/cloudflare-public-v2.gpg \
  -o /usr/share/keyrings/cloudflare-public-v2.gpg
```

- Add Repo Cloudflared

```bash
echo 'deb [signed-by=/usr/share/keyrings/cloudflare-public-v2.gpg] https://pkg.cloudflare.com/cloudflared any main' \
  > /etc/apt/sources.list.d/cloudflared.list
```

- Install Cloudflared

```bash
apt update
apt install -y cloudflared
```

2. **Make a tunnel**

- Login

```bash
cloudflared tunnel login
```

- Make tunnel

```bash
cloudflared tunnel create <tunnel name>
```

- Configuration

```bash
nano /etc/cloudflared/config.yml
```

after that add this configuration

```
tunnel: <UUID>
credentials-file: /root/.cloudflared/<UUID>.json

ingress:
  - hostname: yourdomain.com
    service: http://<device ip>:<port>
  - service: http_status:404
```

U can look ur UUID with this command

```
cloudflared tunnel list
```

or if ur IPs is using SSL u can add this below in `/etc/cloudflared/config.yml`

```
    originRequest:
      noTLSVerify: true
```

the line must be equals to `hostname`

3. Register the DNS

- DNS Register

```bash
cloudflared tunnel route dns tunnel-name yourdomain.com
```

4. Cloudflared test

- Manual Run

```bash
cloudflared tunnel run
```

- Service Run Install

```bash
cloudflared service install
sudo systemctl restart cloudflared
```

## Cloudflared Update

```bash
apt update
apt install --only-upgrade cloudflared
```

## Cloudflared Service Configuration

This configuration below is the latest configuration in 7 February 2026

```bash
nano /etc/systemd/system/cloudflared.service
```

```
[Unit]
Description=cloudflared
After=network-online.target
Wants=network-online.target

[Service]
TimeoutStartSec=15
Type=simple
ExecStart=/usr/bin/cloudflared tunnel run
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
```
save it and

```bash
systemctl daemon-reexec
systemctl daemon-reload
systemctl stop cloudflared
systemctl start cloudflared
```

and check the status

```bash
systemctl status cloudflared
```
