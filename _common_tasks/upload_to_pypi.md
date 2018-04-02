---
title: Upload To PyPI
order: 1
---

Currently this is done manually.

As such, there may be differences between 
the latest version of the library on Github and the latest version on PyPI.

[There are various places where metadata is stored contained the library's 
version number. Ideally, at any given point in time, these should all 
be the same].

- Make sure you have followed the steps in "Getting Started -> Developer 
Computer Setup" (notably `pip install --requirement requirements-dev.txt` 
for installing *twine*)
- Pull down the latest code from `master` branch
- Create and checkout a new branch `git checkout -b version-bump`
- Bump `__version__` in `aiostorage/__init__.py`. This is **not** the 
version that will used when uploading to PyPI, rather it is the version 
found in the package metadata, e.g. `import aiostorage; aiostorage.__version__`
- Bump the version in `setup.py`. This is the version that will be 
uploaded to PyPI
- If using Git tags, update the version there too
- Commit changes, push up the branch and submit a pull request
- Wait for the pull request to be accepted
- Checkout `master` branch, pull down, delete `version-bump` branch
- Create a PyPI package (a `tar.gz` file, not to be confused with the Python
 package `aiostorage/aiostorage`) from the source files, `python setup.py 
 sdist` 
- If not already done, create a `.pypirc` file in the home directory:

```[distutils]
index-servers =
  pypi

[pypi]
username=<username>
password=<password>
```
(ask the project owner for the username and password).
- Upload to PyPI using *twine:*
```
twine upload dist/*.tar.gz && rm dist/*.tar.gz
```