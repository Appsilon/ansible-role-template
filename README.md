# Ansible Role Template

[![Pipeline Status](https://github.com/Appsilon/ansible-role-template/actions/workflows/ci.yml/badge.svg)](https://github.com/Appsilon/ansible-role-template/actions/workflows/ci.yml)

This repository provides a base scaffolding template to use as a quick start of
all Ansible roles at @Appsilon. Includes best practices that should always be
used, like linter, tests, CI, etc.

## How to Use

1. Click "[Use this
   template](https://github.com/Appsilon/ansible-role-template/generate)" to
   create a new repository from it.
1. Update [`./meta/main.yml`](./meta/main.yml) with updated repository and role names.
1. Update [`./role_README.md`](./role_README.md) with updated to repository and role name.
1. Rename `./role_README.md` to `README.md`.
1. Start coding! You're free to change anything.

## Development

### Dependencies

To execute tests locally, it is necessary to install the following
dependencies:

* [Python](https://www.python.org/downloads/)
* [Molecule](https://molecule.readthedocs.io/en/latest/installation.html)

### Preparing the Environment

Create a Python environment:

```bash
python3 -m venv .venv
```

Activate the environment:

```bash
source .venv/bin/activate
```

Install molecule (and its dependencies) inside the environment:

```bash
python3 -m pip install -r requirements.txt
```

### Running

```bash
molecule test
```

To perform quick test after some modification:

```bash
molecule create
molecule converge
molecule verify
```

To log into the running instance for troubleshooting purposes:

```bash
molecule login
```

At the end of the test, destroy the environment:

```bash
molecule destroy
```

### Validation, Linters and Pull-Requests

We want to provide high quality code. For this reason we are using several
[pre-commit hooks](.pre-commit-config.yaml) and [GitHub Actions
workflow](.github/workflows/precommit.yaml). A pull-request to the `main`
branch will trigger these validations and lints automatically. Please check your
code before you will create pull-requests.

Before you can run hooks, you need to have the `pre-commit`
[installed](https://pre-commit.com#install).

```bash
pip install pre-commit
pre-commit install
```

If you are going to enforce [Conventional
Commits](https://www.conventionalcommits.org/) commit message style on the title
you will also need to
[install](https://jorisroovers.com/gitlint/#getting-started) `gitlint`.

```bash
pip install gitlint
```

You then need to install the pre-commit hook like so:

```bash
pre-commit install --hook-type commit-msg
```

> It's important that you run `pre-commit install --hook-type commit-msg`, even
> if you've already used `pre-commit install` before. `pre-commit install` does
> not install `commit-msg` hooks by default!

To manually trigger `gitlint` using `pre-commit` for your last commit message,
use the following command:

```sh
pre-commit run gitlint --hook-stage commit-msg --commit-msg-filename .git/COMMIT_EDITMSG
```

In case you want to change gitlint's behavior, you should either use a
`.gitlint` file (see
[Configuration](https://jorisroovers.com/gitlint/configuration)) or modify the
gitlint invocation in your `.pre-commit-config.yaml` file like so:

```yaml
-   repo: https://github.com/jorisroovers/gitlint
    rev:  # Fill in a tag / sha here
    hooks:
    -   id: gitlint
        args: [--contrib=CT1, --msg-filename]
```

See [pre-commit documentation](https://pre-commit.com/) and [GitHub Actions
documentation](https://docs.github.com/en/actions) for further details.
