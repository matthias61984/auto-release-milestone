# Auto Release Milestone

A GitHub action that automatically drafts a GitHub release based on a newly closed milestone.

The issues that were closed as part of the milestone will be used as the basis to generate the release notes.

## Inputs

### `repo-token`

**Required** The `GITHUB_TOKEN` used to access the current repository through the GitHub REST API.

## Outputs

### `release_url`

The URL of the GitHub release that was drafted. Defaults to an empty string.

## Usage

Here's an exmaple of a workflow that listens for the `milestone: closed1 event and automatically creates a release draft with the issues that were closed as release notes. It also prints the URL of the relase page to the build log.

```yaml
name: Test
on:
  push:
    branches:
      - master
  milestone:
    types: [closed]
jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    steps:
      - name: Get the sources
        uses: actions/checkout@v1
      - name: Create a release draft for a milestone
        id: create-release-draft
        uses: ./
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
      - name: Print the URL of the release draft
        if: steps.create-release-draft.outputs.release-url != ''
        run: echo ${{ steps.create-release-draft.outputs.release-url }}
```
