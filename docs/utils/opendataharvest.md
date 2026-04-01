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
- **DCAT Harvester**: The main script (`DCAT_Harvester.py`) fetches data from specified data portals and validates harvested records against the configured Aardvark JSON schema.
- **GBL 1.0 to Aardvark Converter**: Converts metadata from GBL 1.0 schema to Aardvark schema (`gbl_to_aardvark.py`) for selected legacy repositories.
- **Aardvark Normalizer**: Updates harvested Aardvark records in place before indexing (`normalize.py`).
- **Weekly Combined Pipeline**: The `uwm:geocombine_pull_and_index` rake task pulls configured OGM repositories, harvests DCAT metadata, converts legacy records, normalizes harvested Aardvark metadata, indexes into Solr, and prunes stale Solr records.

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
   - On servers, the expected result is now Python 3.11 when `python3.11` is installed.

2. **Configuration**
   - Update `config/opendataharvest.yaml` with data portal URLs and any specific dataset handling rules.
   - Define default bounding boxes in `lib/opendataharvest/src/opendataharvest/data/default_bbox.csv`.
   - The default app-relative harvest root is `tmp/opengeometadata`.

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
- **Normalize harvested Aardvark metadata**
  ```ruby
  namespace :opendataharvest do
  #...
    desc "Normalize harvested Aardvark metadata"
    task :normalize_aardvark do
      sh "lib/opendataharvest/venv/bin/python3 lib/opendataharvest/src/opendataharvest/normalize.py"
    end
  #...
  end
  ```
- **Run the full weekly harvest/index pipeline**
  ```ruby
  bundle exec rake uwm:geocombine_pull_and_index
  ```
  This task pulls configured OGM repositories, runs `uwm:opendataharvest:harvest_dcat`, `uwm:opendataharvest:gbl1_to_aardvark`, `uwm:opendataharvest:normalize_aardvark`, `geocombine:index`, and `uwm:index:prune_stale`.

## Server checks

On a deployed server, these commands confirm the Python and ICU dependencies used by the normalization step:

```bash
cd /var/www/rubyapps/uwm-geoblacklight/current
lib/opendataharvest/venv/bin/python3 --version
which uconv
uconv --version
```

To confirm the active weekly schedule from the deployed app:

```bash
crontab -l | grep uwm:geocombine_pull_and_index
```

The current production cron entry runs the rake task directly rather than a wrapper shell script.

## Scheduled Tasks
- The combined weekly metadata refresh now runs on Wednesdays:
  ```ruby
    every :wednesday, at: "3:00am", roles: [:app] do
      rake "uwm:geocombine_pull_and_index"
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
│ │ │ ├── classify.py
│ │ │ ├── convert.py
│ │ │ ├── gbl_to_aardvark.py
│ │ │ ├── normalize.py
│ │ │ └── data/
│ │ │   ├── crosswalk.csv
│ │ │   ├── default_bbox.csv
│ │ │   └── agsl-opendata-harvest.json
│ │ ├── requirements.txt
│ │ └── setup_python_env.sh
│ └── venv/...
├── assets/...
└── tasks/...
```
