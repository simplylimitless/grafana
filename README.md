# homelab-grafana

Grafana instance for a homelab monitoring dashboard. Docker-based, scraping from the same `homelab-prometheus` stack and exposing dashboards via `localhost:3000`.

## Quick start

### Deploy from GitHub Container Registry

```bash
docker login ghcr.io -u YOUR_GITHUB_USERNAME   # password: a GitHub PAT with `packages` scope (create one at https://github.com/settings/tokens, not the fine-grained type)
docker pull ghcr.io/simplylimitless/homelab-grafana:latest
docker run -p 3000:3000 --name grafana ghcr.io/simplylimitless/homelab-grafana:latest
```

### Or build locally

```bash
docker build -t homelab-grafana .
docker run -p 3000:3000 --name grafana homelab-grafana
```

Browse to `http://localhost:3000` (default credentials: `admin/admin`).

> **Note:** After setting up the `GHCR_PAT` secret, re-run the workflow from the Actions tab or push a test commit if you don't see the image published yet.

### CI/CD

Pushing to `main` triggers an automatic build (see [`.github/workflows/docker.yml`](.github/workflows/docker.yml)). Each push is tagged with both `latest` and a numeric build ID for rollback:

```bash
docker pull ghcr.io/simplylimitless/homelab-grafana:3
```

**GHCR write permission:** The workflow uses a PAT stored in the repository secret `GHCR_PAT` to push to GitHub Container Registry. Create one at https://github.com/settings/tokens (classic tokens, not fine-grained — that scope isn't available there) with the **`packages`** scope ticked, then add it as a repo secret named `GHCR_PAT`. The automatic `GITHUB_TOKEN` doesn't grant GHCR package write permissions, so this PAT is required.
