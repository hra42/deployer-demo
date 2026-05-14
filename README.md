# deployer-demo

The canonical test target for [deployer](https://github.com/hra42/deployer) — a small SvelteKit app that ships with everything deployer needs to deploy it: an `adapter-node` build, a `Dockerfile`, and a Traefik-ready `docker-compose.yml`.

Stack: SvelteKit (Svelte 5) · Tailwind v4 · shadcn-svelte · TypeScript · Bun · mode-watcher for dark mode.

## Deploy it

Prerequisites on the deploy host (see [Host requirements](https://github.com/hra42/deployer#host-requirements) in the deployer README for the full list):

- Docker + Compose v2
- A running Traefik instance attached to an external Docker network (default name: `web`)
- SSH access for the deployer user

On your laptop:

1. **Fork or push this repo to GitHub.** deployer clones over HTTPS using a token, so it needs to be a real repo it can reach.

2. **Install deployer:**

   ```sh
   curl -fsSL https://deployer.hra42.lol/install | sh
   ```

3. **Run setup once** to write `~/.deployer.yml` (SSH host, key path, GitHub token, Cloudflare credentials):

   ```sh
   deployer setup
   ```

4. **Edit `docker-compose.yml`** in this repo and replace `deployer-demo.example.com` in the Traefik `Host(...)` label with the domain you actually want to deploy to. Commit and push.

5. **Deploy:**

   ```sh
   deployer deploy --repo <your-gh-user>/deployer-demo --domain <your-domain>
   ```

   The `--domain` value must match the `Host(...)` label in `docker-compose.yml`.

Expected output is the six-phase summary (connect → sync → validate → containers → DNS → Zero Trust). Once it finishes, the page at your domain is the landing page in `src/routes/+page.svelte`.

## What's wired up for deployer

- **`svelte.config.js`** — uses `@sveltejs/adapter-node` so the build produces a runnable Node server at `build/index.js`.
- **`Dockerfile`** — multi-stage: builds with bun, runs with `node:22-slim` on `PORT=8080`, `HOST=0.0.0.0`.
- **`docker-compose.yml`** — service `app`, joins external `web` network, declares the Traefik labels deployer expects (`Host(...)` rule, `websecure` entrypoint, `le` cert resolver, port `8080`).
- **`.dockerignore`** — keeps the build context small.

If you renamed `traefik_network` in your `~/.deployer.yml`, update both occurrences of `web` in `docker-compose.yml` to match.

## Local development

```sh
bun install
bun run dev      # http://localhost:5173
bun run build    # produces ./build
bun run preview  # serves the production build
bun run check    # svelte-check, 0 errors expected
```

To smoke-test the production server the way the container will run it:

```sh
bun run build
PORT=8080 HOST=127.0.0.1 node build
# → Listening on http://127.0.0.1:8080
```
