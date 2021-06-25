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

Let's begin to structure our project like this. Reference to [samplepackage](https://github.com/Thanh-Truong/samplepackage)

{% highlight shell %}
── foo
│   ├── __init__.py
│   └── bar
│       ├── __init__.py
│       └── main.py
├── requirements.txt
├── samples
└── setup.py
{% endhighlight %}

## `setup.py` 
This is a heart of packaging our Python project as it contains metadata and drives the build

{% highlight shell %}
import setuptools
from os import path
from foo import __version__

# Read the contents of README.md
this_directory = path.abspath(path.dirname(__file__))
with open(path.join(this_directory, 'README.md'), encoding='utf-8') as f:
    long_description = f.read()

setuptools.setup(
    name='samplepackage',
    version=__version__,
    description="This is a sample package",
    long_description=long_description,
    long_description_content_type='text/markdown',
    url='https://github.com/Thanh-Truong/samplepackage',
    author='Thanh Truong',
    author_email='tcthanh@gmail.com',
    license='MIT',
    packages=setuptools.find_packages(),
    install_requires = ['requests'],
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    entry_points={
        'console_scripts': [
            'samplepackage = foo.bar.main:main'
        ],
    }
)
{% endhighlight %}

The above was an example from one of mine packages and it is quite self-explanatory. There are some notes here:
  * `install_requires` contains all required packages or dependencies. It is important to lock in specific versions so your code will not break.

  * `entry_points`or `console_scripts`. While you can do much more with `console_scripts`, this allows specifying the entry point to your "executable" program. In this example, user can invoke `samplepackage` which is equivalent to `foo.bar.main:main`. To further explain, we have 
    * `foo` as a package
    * `bar`as another package
    * finally `main:main` is the main entry
  

## `.pypirc`
This file resides at `~/.pypirc` containing all authentication needed to identify with pypi.org or test.pypi.org. One needs to create accounts on the two sites and fills in the `.pyirc` with account details. There are also ways to authenticate using token, but I don't find it particularly useful if you build code from your local machine.

{% highlight shell %}
[distutils]
index-servers =
  pypi
  pypitest

[pypi]
repository=https://pypi.python.org/pypi
username=
password=

[pypitest]
repository=https://testpypi.python.org/pypi
username=
password=
{% endhighlight %}

# Building package

List of commands 

{% highlight shell %}
python -m pip install --upgrade pip
python3 -m pip install --upgrade build
python3 -m build
{% endhighlight %}

# Uploading to PyPI or Test PyPI

Install [twine](https://pypi.org/project/twine/), which is a utility for publishing Python packages on PyPI. It provides build system independent uploads of source and binary distribution artifacts.

{% highlight shell %}
python3 -m pip install --upgrade twine
{% endhighlight %}
Upload 

{% highlight shell %}
python3 -m twine upload --repository testpypi dist/*
{% endhighlight %}

After that, you should be able to view your newly uploaded package at https://pypi.org/ or https://test.pypi.org/

# Install our package

{% highlight shell %}
pip install -i https://test.pypi.org/simple/ sample
{% endhighlight %}

# References
- [https://packaging.python.org/tutorials/packaging-projects/](https://packaging.python.org/tutorials/packaging-projects/)
- [https://packaging.python.org/specifications/pypirc/](https://packaging.python.org/specifications/pypirc/)
