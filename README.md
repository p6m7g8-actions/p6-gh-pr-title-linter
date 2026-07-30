# p6m7g8-actions/p6-gh-pr-title-linter

- [p6m7g8-actions/p6-gh-pr-title-linter](#p6m7g8-actionsp6-gh-pr-title-linter)
  - [Usage](#usage)
  - [Accepted types](#accepted-types)
  - [Types that ship no release](#types-that-ship-no-release)
  - [Removed types](#removed-types)

## Usage

```yaml
      - name: Lint PR Title
        uses: p6m7g8-actions/p6-gh-pr-title-linter@main
        with:
          gh_token: ${{ secrets.GITHUB_TOKEN }}
```

## Accepted types

Every accepted type is listed with what `p6m7g8-actions/p6-gh-release` does
with it once the title lands on `main`.

| Type | Release |
| --- | --- |
| `feat` | minor |
| `fix` | patch |
| `chore` | patch, except `chore(release):` |
| `major` | major |
| `ci` | none |
| `docs` | none |
| `refactor` | none |
| `style` | none |
| `test` | none |

A `!` marker on any accepted type is a major bump, as is a `BREAKING CHANGE:`
footer in the commit body. So `fix!: drop the legacy input` is major, not patch.

## Types that ship no release

`ci`, `docs`, `refactor`, `style` and `test` are accepted and produce no tag.
That is deliberate, not a bug: none of them change what a consumer executes, so
there is nothing for a consumer to pin. If a change under one of these types
does alter behaviour, the type is wrong. Use `fix` or `feat` instead.

## Removed types

These were accepted but produced no release, which made the silence a trap
rather than a contract. Use the replacement.

| Removed | Why | Use instead |
| --- | --- | --- |
| `perf` | ships a real change consumers must be able to pin | `fix` or `feat` |
| `revert` | leaves reverted code in the latest tag | `fix` or `fix!` |
| `security` | a CVE patch that ships no tag is the worst case | `fix` |
| `hotfix` | synonym for `fix` that did not release | `fix` |
| `build` | duplicated `chore`, which does release | `chore` |
| `release` | reads as publishing, published nothing | `chore(release)` |
| `deps` | fleet already writes `chore(deps):`, which releases | `chore(deps)` |
| `merge queue` | a type may not contain a space | none needed |

Nothing in the queue path lints a title, so `merge queue` was never
load-bearing: both `build.yml` and `pull-request-lint.yml` skip the lint on
`merge_group` and on `gh-readonly-queue` pushes.

Keep this list and the `p6-gh-release` bump rules in sync. The release logic is
the anchored side, so change this action first.
