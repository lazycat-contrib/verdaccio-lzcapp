# Verdaccio (LazyCat LPK)

Private npm registry with an upstream cache.

- Upstream: https://github.com/verdaccio/verdaccio
- Image: `verdaccio/verdaccio:7.0.2` (Docker Hub, mirror mode, digest-matched)
- License: MIT (full text in `content/licenses/LICENSE`)
- Containers: single, no database

## Notes

- Port 4873, user 10001, entry `uid_entrypoint`.
- **`VERDACCIO_PROTOCOL` controls the listen protocol, not the public URL.** The LazyCat ingress
  speaks plain http to the container, so this must stay `http` — setting it to `https` makes
  Verdaccio serve TLS and the proxy cannot reach it. The external scheme comes from
  `VERDACCIO_PUBLIC_URL`, which is `https://<domain>`.
- `VERDACCIO_PUBLIC_URL` is required: registry metadata carries absolute tarball URLs, and
  without it Verdaccio advertises the container's internal address, so `npm install` downloads
  fail even though browsing works.
- Only `/verdaccio/storage` is mounted. The image ships its config at `/verdaccio/conf`; mounting
  that directory would replace it with an empty one and the container would not start.
- No deploy params: Verdaccio has no built-in admin account. The first user is created by
  registering in the UI or via `npm adduser`, with a password the operator chooses.
- Not yet verified on a real device.
