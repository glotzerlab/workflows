# Workflows

`glotzerlab/workflows` houses reusable GitHub actions and workflows that are in common
use across glotzerlab software packages.

## setup-uv

`setup-uv` installs [uv] and *optionally* installs all the packages in a given
`lockfile`.

To generate a lockfile, run:
```bash
uv pip compile --python-version=3.13 requirements.in --output-file=requirements.txt
```
and add both `requirements.in` and `requirements.txt` to the git repository.

[Renovate] can keep the `requirements.txt` up to date.

In your action workflow, create a Python environment and then call setup-uv:
```yaml
steps:
- name: Checkout
  uses: actions/checkout@08c6903cd8c0fde910a37f88322edcfb5dd907a8 # v5.0.0
- name: Set up Python
  uses: actions/setup-python@e797f83bcb11b83ae66e0230d6156d7c80228e7c # v6.0.0
  with:
    python-version: "3.13"
- name: Set up Python environment
  uses: glotzerlab/workflows/setup-uv@1855eec25e87bdbc06359aa3adc355b59272cae3 # 0.7.0
  with:
    lockfile: "requirements.txt"
```

[uv]: https://github.com/astral-sh/uv
[Renovate]: https://docs.renovatebot.com

## setup-mdbook

`setup-mdbook` installs [mdbook].

In your action workflow:
```yaml
steps:
- name: Set up mdbook
  uses: glotzerlab/workflows/setup-mdbook@1855eec25e87bdbc06359aa3adc355b59272cae3 # 0.7.0
```

See [setup-mdbook/action.yaml] for all options.

[mdbook]: https://rust-lang.github.io/mdBook/
[setup-mdbook/action.yaml]: setup-mdbook/action.yaml

## setup-cargo-bundle-licenses

`setup-cargo-bundle-licenses` installs [cargo-bundle-licenses].

In your action workflow:
```yaml
steps:
- name: Set up cargo-bundle-licenses
  uses: glotzerlab/workflows/setup-cargo-bundle-licenses@1855eec25e87bdbc06359aa3adc355b59272cae3 # 0.7.0
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
  uses: glotzerlab/workflows/setup-row@1855eec25e87bdbc06359aa3adc355b59272cae3 # 0.7.0
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

`update-conda-lockfiles` is no longer maintained. Use [Pixi] and [Renovate] instead.

[Pixi]: https://pixi.sh

## update-uv-lockfiles

`update-uv-lockfiles` is no longer maintained. Use [Renovate] instead.
