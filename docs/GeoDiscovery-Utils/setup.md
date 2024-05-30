---
title: GeoDiscovery-Utils Setup
layout: default
nav_order: 1
parent: GeoDiscovery-Utils
---

# Set Up GeoDiscovery-Utils Locally

## Create a virtual environment with venv.

The venv directory is ignored by git.
This example will create the venv in the opendataharvest directory. 

`python -m venv ./opendataharvest/venv`

Activate the env. 
Make sure your IDE is pointed to the env as its python interpreter,
or run CLI Python in your terminal. 
Your command prompt should now be preceded by (venv) if it's activated.

Windows:

```cmd
# In cmd.exe
venv\Scripts\activate.bat
# In PowerShell
venv\Scripts\Activate.ps1
```

Unix: 

```bash
source ./opendataharvest/venv/bin/activate
```

### Install Dependencies

Once the venv is activated, `pip` will install to that directoy. Install the dependencies with:

```bash
pip install requests
```

Repeat for `pyyaml`, `python-dateutil`, and `jsonschema`
