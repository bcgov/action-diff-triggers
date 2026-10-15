# Diff File Changes with Triggers (Moved)

> [!IMPORTANT]
> **This action has moved to [`bcgov/actions/diff-triggers`](https://github.com/bcgov/actions/tree/main/diff-triggers).** This repository is retiring and will be archived. Update workflows to the new path, pinned to a [release](https://github.com/bcgov/actions/releases) (not `@main`):
>
> ```yaml
> - uses: bcgov/actions/diff-triggers@vX.Y.Z
>   id: triggers
>   with:
>     triggers: |
>       backend/
>       frontend/
> ```
>
> | Old input | New input |
> | --- | --- |
> | `github_token` | `token` |
>
> `triggers`, `ref` and `annotations` keep their names. Legacy parenthesized `triggers` lists are still accepted.
