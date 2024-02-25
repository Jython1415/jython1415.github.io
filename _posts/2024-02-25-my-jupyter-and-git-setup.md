---
title: "Collaborating with Jupyter Notebook using Git"
last_modified_at: 2024-02-25T12:05:00-05:00
categories:
  - Blog
tags:
  - Programming
  - Python
  - Jupyter
  - Git
---

Jupyter Notebook is an amazing solution for using Python for data science. git is what everyone uses for version control you gotta use that too. Now, how do we make them work together?

This is a solution that works for me, and I will probably come back to update this as I improve upon it. Here's my situation

- This solution should work for a workspace (directory) that is part of a larger repository
- The Python environments may be different for different directories in the repository
- Notebook output should not be saved in the repository (these are to be generated locally)
- Some parts of the metadata should be in the repository (e.g. kernel info) but other parts should be kept out of it (e.g. cell execution)

## `.gitattributes` file

```text
*.ipynb filter=filter-notebook
```

## `scripts/initialize.sh`

Sets up the filters

```bash
#!/bin/bash

git  config  filter.filter-notebook.clean  "./scripts/filter-notebook/clean.sh"
git  config  filter.filter-notebook.smudge  "./scripts/filter-notebook/smudge.sh"
```

## `scripts/filter-notebook/clean.sh`

```bash
#!/bin/bash

# This script...
# 1. Sets all cells.outputs to an empty array
# 2. Sets all cells.execution_count to null
# 3. Removes the "collapsed" key from all cells.metadata
# 4. Removes the "autoscroll" key from all cells.metadata
# 5. Removes all but the "name" and "version" keys from metadata.language_info if those keys exist
# 6. Removes all but the "language_info" and "kernelspec" keys from metadata
# 7. Writes parts of the metadata to stderr

jq --indent 1 \
'(.cells[] | select(has("outputs")) | .outputs) = [] '\
'| (.cells[] | select(has("execution_count")) | .execution_count) = null '\
'| .cells[].metadata |= del(.collapsed) '\
'| .cells[].metadata |= del(.autoscroll) '\
'| .metadata.language_info |= {name, version}'\
'| .metadata |= {language_info, kernelspec}'\
'| debug({"Kernel Name": .metadata.kernelspec.name, "Language Info": .metadata.language_info})'
```

## `scripts/filter-notebook/smudge.sh`

```bash
#!/bin/bash

cat
```

## `<workspace path>/scripts/initialize.sh`

```bash
#!/bin/bash

# Initialize the virtual environment
bash  scripts/reinstall-venv.sh
# Install the kernel for the environment
.venv/bin/python  -m  ipykernel  install  --user  --name  <kernel name>  --display-name  "<kernel display name>"
```

## `<workspace path>/scripts/reinstall-venv.sh`

```bash
#!/bin/bash

rm -r .venv
<python version you want> -m venv .venv
bash scripts/update.sh
```

## `<workspace path>/scripts/update.sh`

```bash
#!/bin/bash

source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
.venv/bin/python  -m  ipykernel  install  --user  --name  <kernel name>  --display-name  "<kernel display name>"
```

## `<workspace path>/scripts/update-requirements.sh`

```bash
#!/bin/bash

rm -r .venv
<python version you want> -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements-dev.txt
pip freeze > requirements.txt
bash scripts/update.sh
```
