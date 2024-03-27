---
draft: False
date: 2024-03-28
slug: pip install pain (why you should use different environments for different projects)
categories:
  - Python
  - Virtual Environments
  - poetry
authors:
  - Aviel
---
# pip install pain (why you should use different environments for different projects)

!!! important "pyenve + virtualenv + poetry = ❤️"
    I am always saying to my friends that all of those tools should have a total hour saved counter. I am sure that the number will be huge.

I think I spent more time debugging and fixing my Python environment than actually writing code. I've been using Python for a while now, and I've learned the hard way that managing dependencies can be a real pain. I've tried a bunch of different tools and methods to keep my projects clean and organized, but I've found that using virtual environments is the best way to go.

In this guide, we'll walk through setting up a Python project named `Hummus_optimization`. We'll manage Python versions with Pyenv, isolate our project environment using virtual environments, and handle dependencies with Poetry. This project will serve as a simple example to demonstrate optimizing a hummus recipe based on user preferences for texture and taste.

## Project Setup

### Step 1: Install pyenv

Ensure Pyenv is installed on your system to manage Python versions. Follow the installation instructions provided in the initial part of this guide.

## Installation on Linux and macOS:
```bash
curl https://pyenv.run | bash
```

After installing, add Pyenv to your shell by adding the following lines to your `.bashrc`, `.zshrc`, or` .profile` file:

```bash
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
```

## Installation on Windows:
For Windows, use pyenv-win:
```bash
pip install pyenv-win --target %USERPROFILE%\.pyenv
```
Then, add `%USERPROFILE%\.pyenv\pyenv-win\bin` and `%USERPROFILE%\.pyenv\pyenv-win\shims` to your system's environment variables.

### Side Quest: Install different Python versions

With Pyenv installed, you can now install any version of Python you need. For example, to install Python 3.9.1:

```bash
pyenv install 3.9.1
```
And to set it as the global version:

```bash
pyenv global 3.9.1
```

### Setting a Local Python Version for a Project:

Navigate to the project directory and set the local Python version:

```bash
pyenv local 3.9.1
```
This command sets the Python version to 3.9.1 specifically for the current project, overriding the global Python version.

### Step 2: Create the Project Directory

```bash
mkdir Hummus_optimization
cd Hummus_optimization
```

### Step 3: Set Up a Virtual Environment

Within the project directory, create a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # On Linux/macOS
.venv\\Scripts\\activate  # On Windows
```

### Step 4: Using Poetry for Dependency Management

Poetry is a tool for dependency management and packaging in Python. It allows you to declare the libraries your project depends on and it will manage (install/update) them for you.

## Installation:
```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

Or on Windows, you can use PowerShell:
```powershell
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

Use Poetry to initialize the project, which creates a `pyproject.toml` file:

```bash
poetry init --name Hummus_optimization --dependency numpy:^1.19.2 --dependency scipy:^1.5.2 --no-interaction
```

This command also adds `numpy` and `scipy` as project dependencies.

## Project Structure

Create the following project structure:

```bash 
Hummus_optimization/
│
├── hummus/
│   ├── __init__.py
│   └── optimizer.py
│
├── tests/
│   ├── __init__.py
│   └── test_optimizer.py
│
└── pyproject.toml
```

Use these commands to set up the structure:

```bash
mkdir hummus tests
touch hummus/__init__.py hummus/optimizer.py tests/__init__.py tests/test_optimizer.py
```

## Sample Code

Add this sample code to `hummus/optimizer.py`:

```python
# hummus/optimizer.py

def optimize_hummus(taste_preference, texture_preference):
    """
    Simulate optimizing a hummus recipe.
    """
    taste_score = 90 if taste_preference == "garlicky" else 70
    texture_score = 95 if texture_preference == "creamy" else 75
    
    return (taste_score + texture_score) / 2
```

## Running the Project

Activate the Poetry-managed virtual environment:

```bash
poetry shell
```

Create a script, e.g., `run_optimizer.py`, to test the optimizer:

```python
# run_optimizer.py

from hummus.optimizer import optimize_hummus

score = optimize_hummus("garlicky", "creamy")
print(f"Optimized Hummus Score: {score}")
```

Run the script:

```bash
python run_optimizer.py
```

### Expected Output
Optimized Hummus Score: 92.5



## Adding Dependencies

To add more dependencies:

```bash
poetry add scipy  # Example
```

After adding more dependencies to your Python project using Poetry, there are a few steps and best practices you should follow to ensure your project remains organized and your team can work efficiently. Here's what you should do:

### Step 5: Update `pyproject.toml` and `poetry.lock`

When you add a new dependency using Poetry, it automatically updates the pyproject.toml file and the poetry.lock file. The pyproject.toml file declares your project's dependencies, while the poetry.lock file ensures that the exact versions of the dependencies are locked, providing consistency across all environments.

### Step 6: Intall new dependencies

After adding the dependencies, run the following command to install them:
```bash
poetry install
```

This command reads the pyproject.toml file and installs the dependencies specified there, ensuring that everyone working on the project has the same set of dependencies.



