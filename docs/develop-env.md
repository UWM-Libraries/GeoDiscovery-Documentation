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

### rbenv

Download and install the shell script used to install Rbenv:

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
```

You need to add $HOME/.rbenv/bin to your PATH environment variable to start using Rbenv.

```bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
```

Verify the installation by checking the version of Rbenv:

```bash
rbenv -v
```

### Ruby Bundler

```bash
sudo apt install ruby-bundler
```

### MySQL client

```bash
sudo apt-get install libmysqlclient-dev
```

### Ruby

List all of the versions of Ruby available for installation through Rbenv with:

```bash
rbenv install -l
```

Select a version of Ruby and install it using. At the time of writing, our gemfile specifies version 3.2.1

```bash
rbenv install 3.2.1
```

Set the newly installed version of Ruby as the global version:

```bash
rbenv global 3.2.1
```

Verify the installation by checking the current version of Ruby:

```bash
ruby --version
```

### Java Runtime and NodeJS

```bash
sudo apt install default-jre nodejs
```
