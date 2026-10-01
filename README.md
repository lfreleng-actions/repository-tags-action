<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🏷️ Repository Tags

Fetches tags, counts them, identifies the latest/current tag, identifies the type.

## repository-tags-action

## Usage Example

<!-- markdownlint-disable MD046 -->

```yaml
steps:
  - name: "Get latest repository tag"
    uses: lfreleng-actions/repository-tags-action@main
```

<!-- markdownlint-enable MD046 -->

## Inputs

<!-- markdownlint-disable MD013 -->

| Name       | Required | Default            | Description                                                   |
| ---------- | -------- | ------------------ | ------------------------------------------------------------- |
| type       | False    | both               | Tag types to parse/accept [production/development/both]       |
| repository | False    | current            | Repository to checkout and fetch tags from                    |
| path       | False    | .                  | Relative path under $GITHUB_WORKSPACE to place the repository |

<!-- markdownlint-enable MD013 -->

A checkout is NOT performed if a repository is not provided/specified as input.

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Name        | Description                                       |
| ----------- | ------------------------------------------------- |
| tag_count   | The number of tags in this repository             |
| tag         | The current/latest tag from repository            |
| numeric_tag | The tag [stripped of any v/V prefix] e.g. 1.2.3   |
| semantic    | Set true if returned tag is semantic              |
| calver      | Set true if returned tag uses calendar versioning |

<!-- markdownlint-enable MD013 -->

## Implementation Details

This action does NOT use "fetch-tags: true" in the checkout step.

See: <https://github.com/actions/checkout/issues/1471>

The action uses the Git CLI to retrieve tags.

It fetches with `git fetch --tags --force`, so remote tags replace any local
tags of the same name that point elsewhere. Without `--force`, such a conflict
fails the fetch with `would clobber existing tag`. This can happen on a tag
push with older `actions/checkout` releases or in a reused workspace. Local
tags absent from the remote stay in place.
