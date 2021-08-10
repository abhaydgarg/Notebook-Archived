# Notebook

```bash
# development
mkdocs serve --dirtyreload
# build
mkdocs build
# create venv
python3 -m venv ~/Dev/ag/Notebook
# venv activate
source bin/activate
# venv deactivate
deactivate
# output installed packages in file
pip3 freeze > requirements.txt
# Reinstall all packages
# Remeber to activate venv in order
# to install the packages in locally
pip3 install -r requirements.txt
```
