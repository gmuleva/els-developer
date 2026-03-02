# els-developer
# ELS Developer

Python Flask application with full CI pipeline.

## CI Flow

1. **Unit Tests** → pytest with coverage
2. **Semantic Versioning** → driven by commit messages (Angular convention)
3. **Docker Build & Push** → `gouravmuleva/elsargo:<version>`
4. **Helm values update** → auto-updates `tag` in `gmuleva/els-argo1/helm/values.yaml`

## Commit Convention

| Commit prefix | Version bump |
|---|---|
| `fix: ...` | patch (1.0.1 → 1.0.2) |
| `feat: ...` | minor (1.0.1 → 1.1.0) |
| `feat!:` / `BREAKING CHANGE` | major (1.0.1 → 2.0.0) |

## Required GitHub Secrets

| Secret | Description |
|---|---|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub access token |
| `GH_PAT` | GitHub PAT with `repo` scope (to push to els-argo1) |
```

---

## 🔐 GitHub Secrets to Configure

Go to `els-developer` → **Settings → Secrets → Actions** and add:

| Secret Name | Value |
|---|---|
| `DOCKERHUB_USERNAME` | `gouravmuleva` |
| `DOCKERHUB_TOKEN` | Generate at hub.docker.com → Account Settings → Security |
| `GH_PAT` | Generate at GitHub → Settings → Developer Settings → Personal Access Tokens (needs `repo` scope) |

---

## 🔄 Full Flow Summary
```
push to main
    │
    ▼
pytest (unit tests + coverage)
    │
    ▼
semantic-release dry-run → calculates new version (e.g. 1.0.2)
    │
    ▼
docker build + push → gouravmuleva/elsargo:1.0.2
    │
    ▼
GitHub Release created (tag v1.0.2)
    │
    ▼
Clone els-argo1 → update helm/values.yaml tag: "1.0.2" → push
    │
    ▼
ArgoCD detects change → auto-syncs → deploys new image 🚀