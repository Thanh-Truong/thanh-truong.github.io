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

## `pyproject.toml` 
`pyproject.toml` tells build tools (like `pip` and `build`) what is required to build your project. This tutorial uses `setuptools` 

````
[build-system]
requires = [
    "setuptools>=42",
    "wheel"
]
build-backend = "setuptools.build_meta"
````

## `setup.py` 
This is a heart of packaging our Python project as it contains metadata and drives the build

```
import setuptools
from os import path

__version__ = "0.0.3"
# Read the contents of README.md
this_directory = path.abspath(path.dirname(__file__))
with open(path.join(this_directory, 'README.md'), encoding='utf-8') as f:
    long_description = f.read()

setuptools.setup(
    name='pvclient',
    version=__version__,
    description="This package allows interacting with Azure Purview's REST API.",
    long_description=long_description,
    long_description_content_type='text/markdown',
    url='https://github.com/Thanh-Truong/purview',
    author='Thanh Truong',
    author_email='someone@gmail.com',
    license='MIT',
    install_requires = ['pyapacheatlas==0.6.0','azure-identity==1.6.0','azure-mgmt-resource==18.0.0','azure-mgmt-purview==1.0.0b1'],
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    entry_points={
        'console_scripts': [
            'purview = pvclient.purview:main'
        ],
    },
    package_dir={"pvclient": "src"},
    packages=setuptools.find_packages(where="src"),
    python_requires=">=3.6"
)
```

The above was an example from one of mine packages and it is quite self-explanatory. There are some notes here:
  * `install_requires` contains all required packages or dependencies. It is important to lock in specific versions so your code will not break.

  * `entry_points`or `console_scripts`. While you can do much more with `console_scripts`, this allows specifying the entry point to your "executable" program. In this example, user can invoke `purview` which is equivalent to `pvclient.purview:main`. To further explain, we have 
    * `pvclient` as a package
    * `purview`as a Python file
    * finally `main` is the main entry

## `.pypirc`
This file contains all authentication needed to identify with pypi.org or test.pypi.org

# Building package

List of commands 

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

# Install our package
```
pip install -i https://test.pypi.org/simple/ sample
```

# References
- https://packaging.python.org/tutorials/packaging-projects/
- https://packaging.python.org/specifications/pypirc/