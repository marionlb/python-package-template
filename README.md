# Template Python package

You never have to worry about CI, packaging or code quality again. By using this template, you will get
* a pre-filled CI with build, test and quality jobs
* packaging for free: just put your package code in `src/` and fill in `requirements.txt`, the template will do the rest
* commit hooks that will keep you from ever committing silly stuff again

## CI

The CI jobs are defined in [Gitlab-CI file](.gitlab-ci.yml) and use `tox` tasks defined in [the tox configuration file](tox.ini). 

All CI jobs run in isolated docker containers. 

### Tests phase

Job `pyXY` uses `pythonX.Y` to run pytest with coverage, 
after installing the `sdist` (source distribution) of your package. 

It will fail if: 
* the source distribution of your package fails
* the tests fail

It produces: 
* a log of the tests and a coverage report
* html coverage report

### Quality phase

Runs quality tests using
* the [black formatter][black]
* [flake8][] (configuration in [tox.ini][])
    
Black: 
```bash 
pip install black
black . 
```

Flake8 (options in [tox.ini][]) 
```bash 
pip install flake8
flake8 my_package/ tests/
```

### Miscellaneous
    
Includes: 
* Tests with any other `pyXY` interpreter
* Documentation generation. 

#### Documentation

The documentation generation uses [sphinx][] to create html files from the files 
in [docs/][]. 
 
* [docs/conf.py][] (required) is the sphinx configuration file. Edit it to change defaults or add plug-ins
* [docs/index.rst][] (required) is the documentation index file. 
* [docs/readme.rst][] demonstrates how to import a readme file in RestructuredText
* [docs/API_reference.rst] demonstrates how to display an API refenrence automatically generated from docstrings

### Deploy

On `master` branches, uses Gitlab Pages to deploy the generated documentation. 


## Packaging


1. Rename [my_package/][] and fill it with your source code
2. Edit parameters in [setup.cfg][]. Version can be in [setup.cfg][] or a
module variable in [src/\_\_init__.py][]`
3. Install your new package: 

```bash
pip install -r requirements.txt
# for a wheel install 
$ python setup.py install
# or through pip 
$ pip install .

# for a source distribution
$ python setup.py sdist

# for a dev/editable install 
$ python setup.py develop
# or through pip 
$ pip install -e .
```

Edit the version number of your package in [my_package/__init__.py][]
and in addition to being accessible by [setup.py][], it is accessible as `my_package.__version__`  
when `my_package` is installed. 

The tests configured in the CI will check if the package installation raises any errors. 

## Pre-commit hooks

[pre-commit][] insures that your code passes quality check before every commit. 

Install and initialize it in your repo with: 
```bash
pip install pre-commit
pre-commit install
```

The following hooks are configured in [.pre-commit-config.yaml][]:
* the [black][] formatter refuse the commit if `black` needs to make any formatting changes
* [flake8][] will refuse the commit if it fails the `flake8` checks (parameters are set in `setup.cfg`)

[flake8]: http://flake8.pycqa.org/en/latest/
[black]: https://github.com/ambv/black 
[pre-commit]: https://pre-commit.com/
[setup.cfg]: setup.cfg
[src/__init__.py]: my_package/__init__.py
 