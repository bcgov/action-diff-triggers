[![MIT License](https://img.shields.io/github/license/bcgov/quickstart-openshift.svg)](/LICENSE.md)
[![Lifecycle](https://img.shields.io/badge/Lifecycle-Experimental-339999)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

<!-- Badges -->
[![Issues](https://img.shields.io/github/issues/bcgov-nr/action-diff-triggers)](/../../issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/bcgov-nr/action-diff-triggers)](/../../pulls)
[![MIT License](https://img.shields.io/github/license/bcgov-nr/action-diff-triggers.svg)](/LICENSE)
[![Lifecycle](https://img.shields.io/badge/Lifecycle-Experimental-339999)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

# Diff File Changes with Triggers

Check triggers against a diff of changed files.  For Actions with trigger parameters, like builders and deployers.

# Usage

```yaml
- uses: bcgov-nr/action-diff-triggers@vX.Y.X
  with:
    ### Required

    # Paths used to check against file change (diff)
    triggers: ('./backend/' './frontend/)

    ### Usually a bad idea / not recommended

    # Branch to diff against, defaults to the default branch
    diff_branch: main
```

# Output

The build will true if triggers fire, false otherwise.

# Example, Typical Pattern

Check if files have changed, then do something else.  This is useful for cases like builds, where a job is usually only needed conditionally.

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
          triggers: ('backend/' 'frontend')
  
  results:
    name: Results of Trigger Action
    needs: [check]
    runs-on: ubuntu-22.04
    steps:
      - name: If True
        if: needs.check.outputs.triggered == 'true'
        uses: bcgov-nr/action-diff-triggers@vX.Y.Z
        run: |
          echo "Yes, there are triggered changes!"

      - name: If True
        if: needs.check.outputs.triggered == 'false'
        uses: bcgov-nr/action-diff-triggers@vX.Y.Z
        run: |
          echo "No, there are not triggered changes!"
```

<!-- # Acknowledgements

This Action is provided courtesty of the Forestry Suite of Applications, part of the Government of British Columbia. -->
