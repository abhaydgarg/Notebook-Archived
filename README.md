# Notebook

[https://abhaydgarg.github.io/Notebook/](https://abhaydgarg.github.io/Notebook/)

## Installation

**Note:** Python version required = 3.9.0

```bash
1. Install pyenv.

2. Install Python 3.9.0 using pyenv.

3. Update the "home" path in "pyvenv.cfg" to Python 3.9.0.

4. Create venv `python3 -m venv ~/Notebook`

5. Activate venv `source bin/activate`

6. Reinstall all packages `pip3 install -r requirements.txt`
# Remember to activate venv in order to install the packages locally.
```

## Development

```bash
mkdocs serve --dirtyreload
```

## Build

```bash
mkdocs build
```

## Others

```bash
# output installed packages in file
pip3 freeze > requirements.txt
```
