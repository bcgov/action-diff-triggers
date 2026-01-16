
## ⚠️ BREAKING CHANGES in v1.0

- **`diff_branch` replaced with `ref`, which supports branches, commit SHAs, tags, and relative refs (e.g., `HEAD^`)**

---

[![MIT License](https://img.shields.io/github/license/bcgov/quickstart-openshift.svg)](/LICENSE.md)
[![Lifecycle](https://img.shields.io/badge/Lifecycle-Experimental-339999)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

<!-- Badges -->
[![Issues](https://img.shields.io/github/issues/bcgov-nr/action-diff-triggers)](/../../issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/bcgov-nr/action-diff-triggers)](/../../pulls)
[![MIT License](https://img.shields.io/github/license/bcgov-nr/action-diff-triggers.svg)](/LICENSE)
[![Lifecycle](https://img.shields.io/badge/Lifecycle-Experimental-339999)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

# Diff File Changes with Triggers

Check triggers against a diff of changed files. Supports PR events (including fork PRs), push events, workflow_dispatch, and other GitHub Actions events. For Actions with trigger parameters, like builders and deployers.

## Features

- ✅ **Fork PR Support**: Automatically handles fork and non-fork PRs
- ✅ **Push Event Support**: Compare HEAD vs HEAD^ in push events
- ✅ **Flexible Ref Comparison**: Compare against any ref (branch, commit SHA, HEAD^, etc.)
- ✅ **Smart Path Matching**: Uses git pathspec matching for accurate trigger detection
- ✅ **Space Handling**: Properly handles trigger paths containing spaces

# Usage

```yaml
- uses: bcgov-nr/action-diff-triggers@vX.Y.Z
  with:
    ### Required

    # Paths used to check against file change (diff)
    # Supports quoted strings with spaces: ('backend/' 'my path/file.txt')
    triggers: ('backend/' 'frontend/')

    ### Optional

    # Reference to compare against
    # - PR events: defaults to base repo default branch
    # - Other events (push, workflow_dispatch, etc.): defaults to HEAD^
    ref: main  # or 'HEAD^', commit SHA, branch, tag
```

# Output

Returns `triggered: true` if triggers fire, `triggered: false` otherwise.

# Examples

## Pull Request Event (Typical Pattern)

Check if files have changed, then do something else. This is useful for cases like builds, where a job is usually only needed conditionally.

Please replace `@vX.Y.Z` with the latest version number.

```yaml
on:
  pull_request:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  check:
    name: Check Triggers Against Diff
    outputs:
      triggered: ${{ steps.test.outputs.triggered }}
    runs-on: ubuntu-22.04
    steps:
      - uses: bcgov-nr/action-diff-triggers@vX.Y.Z
        id: test
        with:
          triggers: ('backend/' 'frontend/')
  
  build:
    name: Build if Triggered
    needs: [check]
    if: needs.check.outputs.triggered == 'true'
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - name: Build
        run: |
          echo "Building because triggers matched!"
```

## Push Event

Compare current commit to previous commit (HEAD vs HEAD^):

```yaml
on:
  push:

jobs:
  check:
    runs-on: ubuntu-22.04
    steps:
      - uses: bcgov-nr/action-diff-triggers@vX.Y.Z
        id: test
        with:
          triggers: ('backend/' 'frontend/')
          # ref defaults to HEAD^ for push events
```

## Workflow Dispatch Event

Works with manual triggers and other events. Defaults to comparing HEAD vs HEAD^:

```yaml
on:
  workflow_dispatch:

jobs:
  check:
    runs-on: ubuntu-22.04
    steps:
      - uses: bcgov-nr/action-diff-triggers@vX.Y.Z
        with:
          triggers: ('backend/')
          # ref defaults to HEAD^ for non-PR events
          # Can override: ref: main
```

## Compare Against Specific Commit

```yaml
- uses: bcgov-nr/action-diff-triggers@vX.Y.Z
  with:
    triggers: ('backend/')
    ref: abc123def456  # Compare against specific commit
```

## Fork PR Support

The action automatically handles fork PRs in `pull_request_target` context - no configuration needed!

```yaml
on:
  pull_request_target:  # Works with fork PRs!

jobs:
  check:
    runs-on: ubuntu-22.04
    steps:
      - uses: bcgov-nr/action-diff-triggers@vX.Y.Z
        with:
          triggers: ('backend/')
```

<!-- # Acknowledgements

This Action is provided courtesty of the Forestry Suite of Applications, part of the Government of British Columbia. -->
