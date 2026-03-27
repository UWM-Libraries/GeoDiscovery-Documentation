---
title: Open Data Harvest Tool
layout: default
nav_order: 3
parent: GeoDiscovery Utilities
---

# OpenDataHarvest Documentation

## Overview
The OpenDataHarvest package is designed to automate the harvesting and conversion of metadata records. This includes scripts for managing the environment, running scheduled tasks, and handling specific data conversion needs.

## Functionalities
- **DCAT Harvester**: The main script (`DCAT_Harvester.py`) fetches data from specified data portals.
- **GBL 1.0 to Aardvark Converter**: Converts metadata from GBL 1.0 schema to Aardvark schema (`gbl_to_aardvark.py`).

## Dependencies
- Python 3.x
  - `uwm:opendataharvest:setup_python_env` now prefers `python3.11`, then `python3.10`, then `python3.9`, and finally falls back to `python3`.
  - On RHEL 8 servers, install Python 3.11 before rebuilding the venv when it is available:
    ```bash
    sudo dnf install -y python3.11 python3.11-pip
    ```
- ICU `uconv`
  - Title transliteration during metadata normalization requires the ICU `uconv` binary to be installed and on `PATH`.
  - On RHEL 8 servers, install it with:
    ```bash
    sudo dnf install -y icu
    ```
  - Verify the dependency with:
    ```bash
    which uconv
    uconv --version
    ```
- Required libraries listed in `requirements.txt`:
  - `requests`
  - `pyyaml`
  - `python-dateutil`
  - `jsonschema`

## Setup
1. **Environment Setup**
   - Run the Rake task `uwm:opendataharvest:setup_python_env` to create the Python virtual environment.
   - Dependencies are installed via `setup_python_env.sh`.
   - Verify the interpreter in the venv after setup:
     ```bash
     lib/opendataharvest/venv/bin/python3 --version
     ```

2. **Configuration**
   - Update `config.yaml` with data portal URLs and any specific dataset handling rules.
   - Define default bounding boxes in `default_bbox.csv`.

## Rake Tasks
- **Setup Python Environment**
  ```ruby
  namespace :opendataharvest do
  #...
    desc "Set up Python venv environment for opendataharvest"
    task :setup_python_env do
      sh "lib/opendataharvest/src/setup_python_env.sh"
    end
  #...
  end
  ```
- **Run DCAT Harvester**
  ```ruby
  namespace :opendataharvest do
  #...
    desc "Run the DCAT_Harvester.py Python script"
    task :harvest_dcat do
      sh "lib/opendataharvest/venv/bin/python3 lib/opendataharvest/src/opendataharvest/DCAT_Harvester.py"
    end
  #...
  end
  ```
- **Convert GBL 1.0 to Aardvark**
  ```ruby
  namespace :opendataharvest do
  #...
    desc "Run the conversion scripts on GBL 1.0 metadata institutions"
    task :gbl1_to_aardvark do
      sh "lib/opendataharvest/venv/bin/python3 lib/opendataharvest/src/opendataharvest/gbl_to_aardvark.py"
    end
  #...
  end
  ```

## Scheduled Tasks
- The DCAT Harvester script runs weekly:
  ```ruby
    every :monday, at: "3:30am", roles: [:app] do
      rake "uwm:opendataharvest:harvest_dcat"
    end
  ```

## Directory Structure

```
GeoDiscovery/lib/

├── opendataharvest/
│ ├── src/
│ │ ├── opendataharvest/
│ │ │ ├── DCAT_Harvester.py
│ │ │ ├── init.py
│ │ │ ├── convert.py
│ │ │ ├── gbl_to_aardvark.py
│ │ │ └── data/
│ │ │   ├── crosswalk.csv
│ │ │   └── default_bbox.csv
│ │ ├── requirements.txt
│ │ └── setup_python_env.sh
│ └── venv/...
├── assets/...
└── tasks/...
```
