# Cloudflared-for-LXC
This is the steps to use Cloudflared for LXC (Linux Container) / PCT (Proxmox Container)

### Installation

1. Repo Installation

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

2. Make a tunnel

- Login

```bash
cloudflared tunnel login
```


