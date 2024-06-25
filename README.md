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
  uses: actions/checkout@a5ac7e51b41094c92402da3b24376905380afc29 # v4.1.6
- name: Set up Python
  uses: actions/setup-python@82c7e631bb3cdc910f68e0081d67478d79c6982d # v5.1.0
  with:
    python-version: "3.12"
- name: Set up Python environment
  uses: glotzerlab/workflows/setup-uv@ea2e25d07af862a1c696a932c2bd6b242d142049 # 0.2.0
  with:
    lockfile: "requirements.txt"
```

[uv]: https://github.com/astral-sh/uv

## setup-mdbook

`setup-mdbook` installs [mdbook] and *optionally* installs mdbook plugins:
* [mdbook-linkcheck]

In your action workflow:
```yaml
steps:
- name: Set up mdbook
  uses: glotzerlab/workflows/setup-mdbook@ea2e25d07af862a1c696a932c2bd6b242d142049 # 0.2.0
```

See [setup-mdbook/action.yaml] for all options.

[mdbook]: https://rust-lang.github.io/mdBook/
[mdbook-linkcheck]: https://github.com/Michael-F-Bryan/mdbook-linkcheck
[setup-mdbook/action.yaml]: setup-mdbook/action.yaml

## setup-cargo-bundle-licenses

`setup-cargo-bundle-licenses` installs [cargo-bundle-licenses].

In your action workflow:
```yaml
steps:
- name: Set up mdbook
  uses: glotzerlab/workflows/setup-cargo-bundle-licenses@ea2e25d07af862a1c696a932c2bd6b242d142049 # 0.2.0
```

See [setup-cargo-bundle-licenses/action.yaml] for all options.

[cargo-bundle-licenses]: https://github.com/sstadick/cargo-bundle-licenses
[setup-cargo-bundle-licenses/action.yaml]: setup-cargo-bundle-licenses/action.yaml

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
    uses: glotzerlab/workflows/.github/workflows/stale.yaml@ea2e25d07af862a1c696a932c2bd6b242d142049 # 0.2.0
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
      uses: glotzerlab/workflows/.github/workflows/update-conda-lockfiles.yaml@ea2e25d07af862a1c696a932c2bd6b242d142049 # 0.2.0
      secrets: inherit
      with:
        branch: <name of default branch>
  ```

