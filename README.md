# Common github actions for SIL

## kubectl

Built to be run in an appropriately authorized self-runner provided by LTOps.

### Example usage

```yaml
...
    runs-on: [self-hosted, <some-app-tag>]

    steps:
      -
        uses: sillsdev/common-github-actions/install-kubectl@v1
      -
        run: kubectl --context ${{ secrets.LTOPS_K8S_STAGING_CONTEXT }} get pods
...
```
