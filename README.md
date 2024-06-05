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
- uses: actions/checkout@a5ac7e51b41094c92402da3b24376905380afc29 # v4.1.6
- uses: actions/setup-python@82c7e631bb3cdc910f68e0081d67478d79c6982d # v5.1.0
  with:
    python-version: "3.12"
- name: Set up Python environment
  uses: glotzerlab/workflows/setup-python@<insert hash of tagged version here> # v0.1.0
  with:
    lockfile: "requirements.txt"
```

[uv]: https://github.com/astral-sh/uv
