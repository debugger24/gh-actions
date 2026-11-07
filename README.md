# gh-actions

Shared reusable GitHub Actions workflows.

## docker-publish

Builds a multi-arch image and pushes it to `ghcr.io/<caller repo>/<service>`.

| Ref | Tags | Push |
|-----|------|------|
| `main` | `latest`, short SHA | yes |
| `release/x` | `x` | yes |
| anything else | `feature` | no |

```yaml
build-publish:
  permissions:
    contents: read
    packages: write
  strategy:
    matrix:
      include:
        - { service: backend, context: ., dockerfile: services/backend/Dockerfile }
  uses: debugger24/gh-actions/.github/workflows/docker-publish.yaml@v1
  with:
    service: ${{ matrix.service }}
    context: ${{ matrix.context }}
    dockerfile: ${{ matrix.dockerfile }}
    # build-args: |
    #   KEY=value
```

The caller job must grant `packages: write`.

## helm-check

Installs Helm and kubeconform, then runs `make helm-check` in the caller repo
(the caller's Makefile defines what is linted and validated).

```yaml
helm-validation:
  uses: debugger24/gh-actions/.github/workflows/helm-check.yaml@v1
```
