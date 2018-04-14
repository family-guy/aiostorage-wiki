---
title: Project Setup
order: 2
---

- Login to Github and fork the *aiostorage* repository `https://github
.com/family-guy/aiostorage`
- Clone the forked repository to your local machine `git clone https://github.com/<your-username>/aiostorage.git`
- Add *aiostorage* as a remote repository `git remote add upstream https://github.com/family-guy/aiostorage.git`
- Ensure you have the latest *aiostorage* code `git fetch upstream` and 
merge into your master branch `git checkout master; git merge upstream/master` 
- From the project root, `mkdir dist`
- Create an environment file `dist/environment.yml`:

```
name: aiostorage
dependencies:
  - python=3.6.3
  - ipython=6.1.0
  - pip:
    - aiohttp<3.0,>=2.0
```
- Create a conda environment `conda env create --name aiostorage --file 
dist/environment.yml`
- Activate the environment `source activate aiostorage`
- Install packages for development work `pip install --requirement 
requirements-dev.txt`
- Install *aiostorage* `pip install --editable .`
- Check the linting `flake8`
- In `aiostorage/settings.py`, enter your Backblaze credentials and test 
bucket ID
- Run the tests `pytest`
- Run `scripts/example.py`; the files should upload around twice as fast 
compared with `pytest` 