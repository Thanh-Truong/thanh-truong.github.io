---
title: 'Packaging Python Projects¶'
date: 2021-06-18
permalink: /posts/2021/06/package-your-python-code/
tags:
  - Python
  - PyPi
  - architecture
published: true
excerpt: ""
---

We need make sure to have a correct project structure, something looks like this

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

* List of commands 

```
python -m pip install --upgrade pip
python3 -m pip install --upgrade build
python3 -m build
```

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
