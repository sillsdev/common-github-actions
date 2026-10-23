# Common github actions for SIL

## kubectl

Built to be run in an appropriately authorized self-runner provided by LTOps.

### Example usage

```yaml

...
runs-on: [self-hosted, <some-app-tag>]

steps:
  - uses: sillsdev/common-github-actions/install-kubectl@v1
  - run: kubectl --context ${{ secrets.LTOPS_K8S_STAGING_CONTEXT }} get pods
...
```

## Discord Notification

The `discord-notification` action sends GitHub push and pull request notifications to a Discord channel using a Discord incoming webhook.

### Usage

Add a workflow to your repository under `.github/workflows/discord-notification.yml`:

```yaml
name: Discord Notifications

on:
  push:
    branches:
      - master
      - develop
  pull_request:
    types:
      - opened
      - reopened
      - closed

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - uses: sillsdev/common-github-actions/discord-notification@main
        with:
          webhook-url: ${{ secrets.DISCORD_WEBHOOK_URL }}
```

Replace the branch names and event types with those appropriate for your repository. For example, a repository using `main` as its default branch can configure `main` instead of `master`.

### Configuration

| Input         | Required | Description                                               |
| ------------- | -------- | --------------------------------------------------------- |
| `webhook-url` | Yes      | Discord incoming webhook URL for the destination channel. |

Configure the `DISCORD_WEBHOOK_URL` Actions secret in the consuming repository, or use an organization-level secret shared by selected repositories.

The action detects the GitHub event type automatically and formats the notification accordingly:

- **Push:** repository, branch, pusher, commit summaries, and a comparison link when available.
- **Pull request:** repository, pull request number and title, author, and event action.

### Notes

- Each consuming repository controls its own workflow triggers, including branches and pull request event types.
- The action does not require checking out the repository.
- Pull request workflows triggered from forks generally cannot access repository secrets. Notifications for those pull requests require additional consideration.
