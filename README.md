# Workflows

`glotzerlab/workflows` houses reusable GitHub actions and workflows that are in common
use across glotzerlab software packages.

## setup-uv

`setup-uv` installs [uv] and *optionally* installs all the packages in a given
`lockfile`.

To generate a lockfile, run:
```bash
uv pip compile --python-version 3.12 --python-platform linux requirements.in > requirements.txt
```
and add both `requirements.in` and `requirements.txt` to the git repository.

In your action workflow, create a Python environment and then call setup-uv:
```yaml
steps:
- name: Checkout
  uses: actions/checkout@d632683dd7b4114ad314bca15554477dd762a938 # v4.2.0
- name: Set up Python
  uses: actions/setup-python@f677139bbe7f9c59b41e40162b753c062f5d49a3 # v5.2.0
  with:
    python-version: "3.12"
- name: Set up Python environment
  uses: glotzerlab/workflows/setup-uv@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
  with:
    lockfile: "requirements.txt"
```

[uv]: https://github.com/astral-sh/uv

## setup-mdbook

`setup-mdbook` installs [mdbook].

In your action workflow:
```yaml
steps:
- name: Set up mdbook
  uses: glotzerlab/workflows/setup-mdbook@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
```

See [setup-mdbook/action.yaml] for all options.

[mdbook]: https://rust-lang.github.io/mdBook/
[setup-mdbook/action.yaml]: setup-mdbook/action.yaml

## setup-cargo-bundle-licenses

`setup-cargo-bundle-licenses` installs [cargo-bundle-licenses].

In your action workflow:
```yaml
steps:
- name: Set up mdbook
  uses: glotzerlab/workflows/setup-cargo-bundle-licenses@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
```

See [setup-cargo-bundle-licenses/action.yaml] for all options.

[cargo-bundle-licenses]: https://github.com/sstadick/cargo-bundle-licenses
[setup-cargo-bundle-licenses/action.yaml]: setup-cargo-bundle-licenses/action.yaml

## setup-row

`setup-row` installs [row].

In your action workflow:
```yaml
steps:
- name: Set up row
  uses: glotzerlab/workflows/setup-row@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
```

See [setup-row/action.yaml] for all options.

[row]: https://row.readthedocs.org
[setup-row/action.yaml]: setup-row/action.yaml

## stale

To reuse the standard stale workflow, create a workflow `stale.yaml` with the
contents:
```yaml
name: Close stale issues and PRs

on:
  schedule:
    - cron: '0 19 * * *'

  workflow_dispatch:

jobs:
  stale:
    uses: glotzerlab/workflows/.github/workflows/stale.yaml@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
```

## update-conda-lockfiles

To automatically update conda lock files monthly:
1. Prepare a directory with the base `environment.yaml` and a script that updates all
  lock files (see https://github.com/glotzerlab/hoomd-blue/tree/trunk-minor/.github/workflows/environments
  for an example).
2. Ask an organization admin to install the pull request submitter bot.
3. Create a workflow `update-conda-lockfiles.yaml` with the contents:

  ```yaml
  name: Update conda lockfiles

  on:
    schedule:
      - cron: '0 12 1 * *'

    workflow_dispatch:

  jobs:
    update:
      uses: glotzerlab/workflows/.github/workflows/update-conda-lockfiles.yaml@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
      secrets: inherit
      with:
        branch: <name of default branch>
  ```

## update-uv-lockfiles

To automatically update uv lock files monthly:
1. Prepare a directory with the base `requirements-*.in` files and a script that updates
   all lock files (see
   https://github.com/glotzerlab/rowan/tree/master/.github/workflows for an example).
2. Ask an organization admin to install the pull request submitter bot.
3. Create a workflow `update-uv-lockfiles.yaml` with the contents:

  ```yaml
  name: Update uv lockfiles

  on:
    schedule:
      - cron: '0 12 1 * *'

    workflow_dispatch:

  jobs:
    update:
      uses: glotzerlab/workflows/.github/workflows/update-uv-lockfiles.yaml@1747bc5c994ec280440dd051f2928791407692c8 # 0.5.1
      secrets: inherit
      with:
        branch: <name of default branch>
  ```
