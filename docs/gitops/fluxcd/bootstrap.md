---
tags: [flux]
---
# Bootstrap

## GitHub

```bash
export GITHUB_TOKEN=<token>
flux bootstrap github \
  --owner=<user-or-org> \
  --repository=<repo> \
  --branch=main \
  --path=clusters/production \
  --personal
```

## Generisches Git (z. B. Gitea, GitLab self-hosted)

```bash
flux bootstrap git \
  --url=ssh://git@git.example.local/infra/fleet.git \
  --branch=main \
  --path=clusters/production
```

## Upgrade

```bash
flux install --export > clusters/production/flux-system/gotk-components.yaml  # oder: bootstrap erneut ausführen
```

## Gotchas / Learnings

!!! note "TODO"
    Eigene Erfahrungen ergänzen (Token-Scopes, Deploy-Keys, Air-Gapped, ...).
