---
tags: [flux, secrets]
---
# Secrets mit SOPS (age)

## Setup

```bash
age-keygen -o age.agekey
kubectl create secret generic sops-age \
  --namespace=flux-system \
  --from-file=age.agekey=age.agekey
```

`.sops.yaml` im Root des GitOps-Repos:

```yaml
creation_rules:
  - path_regex: .*.yaml
    encrypted_regex: ^(data|stringData)$
    age: age1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## Verschlüsseln & bearbeiten

```bash
sops --encrypt --in-place secret.yaml
sops secret.yaml        # im Editor entschlüsselt bearbeiten
```

## Kustomization mit Decryption

```yaml title="snippets/flux/kustomization-sops.yaml"
--8<-- "snippets/flux/kustomization-sops.yaml"
```

!!! danger
    Den privaten `age.agekey` niemals committen.
