# syntax=docker/dockerfile:1

# DEV image for this demo-ui (a Vite 6 + React 19 SPA), used by the
# altered-dev-environment Aspire orchestrator (its `uniques-ui` service points at this
# Dockerfile). It runs the Vite dev server (HMR): the orchestrator bind-mounts demo-ui/
# at /app and keeps node_modules in a volume (npm ci into it on first start), so source
# edits are live.
#
# The build context is THIS folder (demo-ui/docker/dev/), so it ships no app source.
# The SPA talks to the uniques API straight from the browser (the API sets permissive
# CORS), so VITE_API_BASE_URL points at the browser-reachable API URL — no Vite proxy /
# CORS work needed. Vite reads VITE_-prefixed vars from the environment, so the
# orchestrator's env injection is enough.

FROM node:22-bookworm-slim

WORKDIR /app

COPY --link --chmod=755 docker-entrypoint.sh /usr/local/bin/docker-entrypoint

ENTRYPOINT ["docker-entrypoint"]

# Run Vite on the SAME port we publish (8004) so the HMR websocket — which the browser
# opens to the page's own host:port — lines up. --host binds 0.0.0.0; --strictPort
# fails loudly rather than drifting to another port.
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0", "--port", "8004", "--strictPort"]
