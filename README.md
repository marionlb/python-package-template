# Template Python package

You never have to worry about CI, packaging or code quality again. By using this template, you will get
* packaging for free: just put your package code in `src/` and fill in `requirements.txt`, the template will do the rest
* a pre-filled CI with build, test and quality jobs
* commit hooks that will keep you from ever committing silly stuff again

## Packaging

1. Package code in `src/`
2. Edit parameters in [setup.cfg][]. Version can be in [setup.cfg][] or a
module variable in [src/\_\_init__.py][src/__init__.py]`
3. Install your new package: 
```bash
$ pip install -r requirements.txt
$ python setup.py
```

For a dev-mode (editable) install, use: 
```bash
$ pip install -e .
```

## CI

The CI jobs are defined in `.gitlab-ci.yml` and use `make` tasks defined in Makefile. 

Currently, there are 3 jobs: 
* **build**: tries to install the package in the CI docker image, 
will fail if a required package is missing in `requirements.txt` or 
for various packaging errors 
* **test**: runs tests (pytest by default). will fail if tests fail or no tests are found
* **quality**: runs quality tests using
    * the [black formatter][black]
    * [flake8][] (parameters in `setup.cfg`)
  
Job logic is in a `Makefile` so CI-jobs can easily be run from non-Gitlab-CI environments, 
e.g. locally or porting to Github
  
## Pre-commit hooks

Running `make devdep` will install [pre-commit][] and the following hooks configured in `.pre-commit-config.yaml`:
* the [black][] formatter refuse the commit if `black` needs to make any formatting changes
* [flake8][] will refuse the commit if it fails the `flake8` checks (parameters are set in `setup.cfg`)

[flake8]: http://flake8.pycqa.org/en/latest/
[black]: https://github.com/ambv/black 
[pre-commit]: https://pre-commit.com/
[setup.cfg]: setup.cfg
[src/__init__.py]: my_package/__init__.py
 