---
layout: default
title: Setting up a development environment
nav_exclude: false
nav_order: 1.99
---

# Setting up a Development environment on a Windows 10 computer

## Install Ubutnu using the Windows Subsystem for Linux (WSL)

Full instructions here: https://learn.microsoft.com/en-us/windows/wsl/install

1. Run PowerShell as an administrator.

1. Run `wsl --install`. This will instal Ubutnu by default. See the link above to specify a different distro.

1. You will need to restart and then run Ubuntu. Setup a UNIX user and password.

1. Follow the [Development Instructions](./develop.md) after installign depdnencies...

## Install dependencies

### rvm - Ruby Version Manager:

Install rvm

See [Repo](https://github.com/rvm/ubuntu_rvm)

### Ruby Bundler

```bash
sudo apt install ruby-bundler
```

### MySQL client

```bash
sudo apt-get install libmysqlclient-dev
```

### Ruby

List all of the versions of Ruby available for installation through rvm with:

```bash
rvm list known
```

Select a version of Ruby and install it using rvm. At the time of writing, our gemfile specifies version 3.2.1

```bash
rvm install ruby-23.2.1
```

Set the newly installed version of Ruby as the global version:

```bash
rvm --default use 3.2.1
```

Verify the installation by checking the current version of Ruby:

```bash
ruby --version
```

### Java Runtime and NodeJS

```bash
sudo apt install default-jre nodejs
```

### Chrome Browser

This is required to run the CI tests.git 

```bash
sudo apt-get install libxss1 libappindicator1 libindicator7
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome*.deb
```
