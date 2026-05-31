# CI/CD Variables

## Definition

CI/CD variables store **configuration values** for pipelines.

## GitHub Actions

```yaml
# .github/workflows/ci.yml
env:
  NODE_VERSION: 18
  API_KEY: ${{ secrets.API_KEY }}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}
```

## Quick Revision

- Store secrets in CI/CD settings
- Use environment variables
- Never hardcode secrets
- Reference with `${{ secrets.NAME }}`

---

## Related Topics

- [[What-is-CICD]] - [[What-is-CICD|CI/CD]]
- [[What-is-GitActions]] - [[What-is-GitActions|GitHub Actions]]
- [[CI-CD-Variables]] - [[CI-CD-Variables|CI/CD variables]]
- [[Setup-CICD]] - [[Setup-CICD|Setting up CI/CD]]
