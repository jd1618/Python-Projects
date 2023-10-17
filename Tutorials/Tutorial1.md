---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.15.2
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Python Project Tutorial 1


## Virtual environments 


A *virtual environment* (VE) is a space where you can install Python libraries separate from your installation of Python. The benefit of using a VE is that it allows you to download specific libraries for different projects you are working on. This is particularly useful since it prevents you from downloading multiple versions of the same library in a single place (in the event that this happens, Python would be unable to distinguish between the different versions). Moreover, there is no limitation on the number of VEs you can have (including for a single project), making them a powerful tool for any Python developer.


To create a VE, you can execute the following line of code in the command line (make sure you are in the desired project directory):

```python
python -m venv VE_name # Change the name of VE_name to
                       # the name you want for the VE
```

You can then activate the VE by running the following:

```python
VE_name\Scripts\activate 
```

When this VE is activated you can proceed to install the relevant libraries for the Python project as follows (the selection of libraries is just an example):

```python
python -m pip install numpy pandas matplotlib scipy statsmodels seaborn plotly
```

Once this has been achieved, you can then write a code file that has access to the installed libraries:

```python
import numpy as np
import pandas as pd 
import matplotlib.pyplot as plt
import scipy as sc
import statsmodels.api as sm
import seaborn as sns
import plotly.express as px
```

## Poetry


A useful package manager for VEs is *Poetry*. Using Poetry means you do not need to manually create your VEs or pip install your Python libraries. Moreover, it is a great tool to manage you project dependencies, as it works to find a combination of installations that work well together (these are stored in a so-called lock file). Furthermore, it provides an intuitive alternative to building Python packages and distributing them.


The installation of Poetry is recommended through the use of the Poetry installer (as opposed to pip), since this isolates Poetry from your other installations. The installation can be executed in the command line as follows:

```python
# Windows device:
(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | py - 

# Apple device:
curl -sSL https://install.python-poetry.org | python3 - 
```

Alternatively, this can be done using pip:

```python
python -m pip install --user poetry
```

To test that the installation was successful, you can check the version of the Poetry we have installed:

```python
poetry --version
```

Depending on how Poetry was installed, you can update the version in the following way:

```python
# Poetry installer method:
poetry self update 

# pip install method:
python -m pip install --upgrade poetry 
```

To create a new project with Poetry, you can simply write in the command line:

```python
poetry new demo
```

This will create a directory called demo with the following file structure:

```python
demo
├── demo
│   └── __init__.py
├── pyproject.toml
├── README.rst
└── tests
    ├── __init__.py
    └── test_demo.py
```

The demo directory is where the project is. Inside, you have a demo folder, with an \_\_init\_\_.py file for convenience. In addition, pyproject.toml stores the metadata for the project, the README.rst file should provide a brief description of the project, and the tests folder stores the unit tests for the Python project.


The TOML file is a simple file type that is easy to read and write. Here, the pyproject.toml file contains the following:

```python
[tool.poetry]

# Metadata for the project:
name = "demo"
version = "0.1.0"
description = ""
authors = ["Your name <your@e-mail.address>"]

# Project dependencies (required to run software):
[tool.poetry.dependencies]
python = "^3.10"

# Dependencies belonging to project developers 
# (needed for development and testing):
[tool.poetry.dev-dependencies]
pytest = "^5.2"

# Settings for the build system:
[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

To add and install packages, you can either edit the pyproject.toml file, or use the command poetry:

```python
poetry add <package name> # e.g., <package>=requests
```

The second approach additionally looks for a suitable version that does not conflict with other dependencies, directly installs in the project VE and creates/updates the lock file called poetry.lock. Furthermore, this command creates a VE outside of the project directory if this is the first time you have used the project. If you want to move the VE inside the project directory (including for future projects), you can execute:

```python
poetry config virtualenvs.in-project true 
```

All dependencies of the requests package (all versions) will be installed and can be found in the lock file, however, when inspecting the pyproject.toml, only the requested package will be visible. The purpose of the lock file is that all versions of the requests package are the same when recreating the VE. 


Despite developer dependencies not being needed to run the project, you can add them via:

```python
poetry add --dev <package name> 
```

You can remove packages/libraries from a project with

```python
poetry remove <name>
    
poetry remove --dev <package name> # removes developer dependencies
```

If you already have an existing project that has used Poetry, you can install all dependencies at onces using:

```python
poetry install # installs all packages from the lock file (if existing), 
               # otherwise, installs packages in a newly created lock file
```

You can run a program in the project VE as well:

```python
poetry run python run.py

poetry run pytest # to run pytest as a developer dependency
```

You can also activate the VE in a separate shell if we execute:

```python
poetry shell
```

Even though it is useful to have packages stored in a lock file so that our projects always work as expected (due to packages remaining the same version as at the inception of the project), there might still be instances where you want to update packages for project use. To do this, you can simply use the command:

```python
poetry update # updates dependencies in the VE and the poetry.lock file

poetry update package_name # updates a specific package

poetry add package_name@latest # updates the package to the latest version 
                               # (or replace latest with version number)
```

The command:

```python
poetry build 
```

creates two files in a new directory called dist. The first file is a compiled package, while the second file contains the source code for the package. Following this, you can publish with an access token with the sequence of commands:

```python
poetry config pypi-token.pypi <token> # creates access token instead
                                      # of username and password
poetry publish # publish project
```

Some common Poetry commands include:

```python
poetry --version # shows current version of Poetry

poetry new <name> # creates new Poetry project

poetry init # convert existing project to new project

poetry add <package> # add package 

poetry remove <package> # remove package

poetry show # list packages installed

poetry export -f <filename> # export dependency list

poetry install # install all dependecies of current Poetry project

poetry run <command> # run command in project VE

poetry shell # activate VE in new shell
```

Sometimes, it is useful to export project dependencies to a .txt file when collaborating with those who do not use Poetry:

```python
poetry export -f requirements.txt > requirements.txt
```
