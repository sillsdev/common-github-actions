# Common github actions for SIL

## kubectl

Built to be run in an appropriately authorized self-runner provided by LTOps.

### Example usage

```yaml
...
    steps:
      -
        uses: sillsdev/common-github-actions/kubectl@v1
        with:
          args: --context ${{ secrets.LTOPS_K8S_STAGING_CONTEXT }} get pods
...
```
