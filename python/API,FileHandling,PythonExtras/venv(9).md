# Virtual Environments
# What is a virtual environment?
A **virtual environment** is an isolated Python environment that allows you to install packages separately from your system Python installation. Each virtual environment has its own Python interpreter and its own set of installed packages, completely separate from other projects and your system Python.

Think of it like a separate workspace for each project. You can have different versions of the same package in different virtual environments without conflicts.

```bash
# Create a virtual environment
python -m venv <environment_name>

# Activate it (on Windows)
<environment_name>\Scripts\activate

# Activate it (on macOS/Linux)
source <environment_name>/bin/activate
```
# Why this matters

Virtual environments are essential for Python development because they solve several critical problems:

- **Dependency isolation**: Different projects can use different versions of the same package without conflicts
- **Reproducibility**: You can ensure everyone working on a project uses the same package versions
- **Clean system**: Keeps your system Python installation clean and avoids *"dependency hell"*
- **Version management**: Test your code with different package versions easily
- **Deployment**: Match your development environment to production

Without virtual environments, installing packages *globally* can lead to:
- Version conflicts between projects
- Breaking system tools that depend on specific package versions
- Difficulty reproducing bugs (works on your machine, but not others')
- Uninstalling a package breaking other projects

# Creating a virtual environment
Python 3.3+ includes the `venv` module for creating virtual environments. The process is straightforward:

## Basic creation
```bash
python -m venv venv
```
This creates a new directory called `venv` (you can name it anything) containing:
- A copy of the Python interpreter
- A `pip` installation for installing packages
- Scripts to activate/deactivate the environment

## Specifying Python version
If you have multiple Python versions installed, you can specify which one to use:

```bash
# Use Python 3.11 specifically
python3.11 -m venv venv

# Or use the full path
/usr/bin/python3.11 -m venv venv
```
## Custom location
You can create the virtual environment in any location:

```bash
# Create in a specific directory
python -m venv /path/to/myproject_env

# Create in the current directory with a custom name
python -m venv .venv
```
# Activating a virtual environment
Once created, you need to **activate** the virtual environment before using it. Activation modifies your shell's PATH so that commands like `python` and `pip` point to the virtual environment's versions. 

## On Windows (Command Prompt)
```bash
venv\Scripts\activate
```
## On Windows (PowerShell)
```bash
venv\Scripts\Activate.ps1
```
If you get an execution policy error, run:

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
## On macOS and Linux (wsl as well)
```bash
source venv/bin/activate
```
## Verifying activation
When activated, you'll see the environment name in your prompt:
```bash
(venv) $ python --version
Python 3.11.5

(venv) $ which python
/path/to/venv/bin/python
```
# Deactivating a virtual environment
To exit the virtual environment and return to your system Python:

```python
deactivate
```
# Installing packages in a virtual environment

Once activated, use `pip` normally—packages install into the virtual environment:
```bash
# Activate first
source venv/bin/activate

# Install packages
pip install requests
pip install flask==2.3.0
pip install numpy pandas matplotlib

# List installed packages
pip list

# Show package information
pip show requests
```
All packages install into the virtual environment's `site-packages` directory, completely isolated from your system Python.

# Managing dependencies

## Creating a requirements file

Save your project's dependencies to a file:
```bash
pip freeze > requirements.txt
```
This creates a `requirements.txt` file listing all installed packages and their versions:

```bash
requests==2.31.0
flask==2.3.0
numpy==1.24.3
pandas==2.0.3
```
## Installing from requirements.txt

Others (or you on a different machine) can recreate the environment:

```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate

# Install all dependencies
pip install -r requirements.txt
```
This ensures everyone uses the same package versions.

### best practices with git

it is best that you dont send the `venv` folder to github. (venv files can get and there is no reason to have it on github)
so each person who clones the code will have to create a venv for themself and load the required packages from the `requirements.txt` file

to prevent this create a `.gitignore` file
(this is a hidden file where you can tell git not to add or commit files or folders)

you need to define the location of the files and folders by their relitive path or their absolute path

```gitignore
# .gitignore
venv/
```






















