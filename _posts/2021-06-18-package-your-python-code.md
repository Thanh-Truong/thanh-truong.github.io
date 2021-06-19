---
title: 'Packaging Python Projects'
date: 2021-06-18
permalink: /posts/2021/06/package-your-python-code/
tags:
  - Python
  - PyPi
  - architecture
published: true
excerpt: "It is a short note to show how to package a simple Python project, build it and how to upload it to the [Python Package Index](https://pypi.org/), which is a repository of software for the Python programming language. [PyPI](https://pypi.org/) helps you find and install software developed and shared by the Python community."
---
It is a short note to show how to package a simple Python project, build it and how to upload it to the [Python Package Index](https://pypi.org/), which is a repository of software for the Python programming language. [PyPI](https://pypi.org/) helps you find and install software developed and shared by the Python community.

There is also [Test Python Package Index](https://test.pypi.org/), which is similar to PyPI but is a stage zone, where people can test their packages before uploading them to the official [PyPI](https://pypi.org/).


# Project structure

The Python project should include necessary files and be structured to allow [`build`](https://pypi.org/project/build/) creating a package out of it.

Let's begin to structure our project like this

```
── pyproject.toml
├── requirements.txt
├── setup.py
├── src
│   └── sample
│       ├── __init__.py
│       ├── foo.py
│       ├── bar.py
│       └── main.py
├── sample.json
```

* `pyproject.toml` tells build tools (like `pip` and `build`) what is required to build your project. This tutorial uses `setuptools`, so open `pyproject.toml` and enter the following content:

````
[build-system]
requires = [
    "setuptools>=42",
    "wheel"
]
build-backend = "setuptools.build_meta"
````

* `setup.py`

* `.pypirc`file contains all authentication needed to identify with pypi.org or test.pypi.org

# Building package
* List of commands 

```
python -m pip install --upgrade pip
python3 -m pip install --upgrade build
python3 -m build
```
# Uploading to PyPI or Test PyPI

Install twine
```
python3 -m pip install --upgrade twine
```
Upload 
````
python3 -m twine upload --repository testpypi dist/*
````

* Install our package
````
pip install -i https://test.pypi.org/simple/ sample
````

# References
- https://packaging.python.org/tutorials/packaging-projects/
- https://packaging.python.org/specifications/pypirc/