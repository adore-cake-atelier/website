# Adore Cake Atelier

Website for [Adore Cake Atelier](https://www.instagram.com/adorecakeatelier/) — custom cakes for every occasion.

Live at http://204.168.195.80/ (domain + HTTPS not set up yet).

## Local development

This is a static site — no build step. To preview locally:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Deployment

Hosted on the same Hetzner VPS used for the Telegram proxy (`mtg` container), as a
second, independent Docker container — the two don't interfere with each other.

- Server: `204.168.195.80`, repo checked out at `/opt/adorecakeatelier-website`
- Served by `nginx:alpine`, container name `adorecakeatelier-website`, mapped to host port 80
- Port 443 is already used by the `mtg` Telegram proxy container, so this site is
  plain HTTP for now (no domain or HTTPS configured yet)

To redeploy after pushing changes to `main`:

```bash
ssh root@204.168.195.80 '
  cd /opt/adorecakeatelier-website && git pull &&
  docker restart adorecakeatelier-website
'
```

(`docker restart` is enough since the container mounts the repo directory directly —
no rebuild needed for plain HTML/CSS changes.)

### Future: domain + HTTPS

`adorecakeatelier.com` (registered at GoDaddy) is not yet pointed at this server.
Plan is to put it behind Cloudflare (free tier) once ready, since port 443 on the
server is already taken by the Telegram proxy — Cloudflare terminates HTTPS at its
edge and forwards to this server over port 80, so no server-side changes needed.
