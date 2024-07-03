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
- Required libraries listed in `requirements.txt`:
  - `requests`
  - `lxml`
  - `pandas`
  - `pyyaml`

## Setup
1. **Environment Setup**
   - Run the Rake task `uwm:opendataharvest:setup_python_env` to create the Python virtual environment.
   - Dependencies are installed via `setup_python_env.sh`.

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
      sh "lib/opendataharvest/setup_python_env.sh"
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
      sh "lib/opendataharvest/venv/bin/python3 lib/opendataharvest/opendataharvest/DCAT_Harvester.py"
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
      sh "lib/opendataharvest/venv/bin/python3 lib/opendataharvest/gbl-1_to_aardvark/gbl_to_aardvark.py"
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
