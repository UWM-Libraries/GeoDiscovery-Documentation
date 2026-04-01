---
layout: default
title: Setting up a development environment
nav_exclude: false
nav_order: 1.8
---

# Setting up a development environment (WSL Ubuntu)

This page covers local development setup on Windows using WSL Ubuntu.
For production deployment instructions, see [Deploy the application](deploy).

## Install WSL / Ubuntu

Use the current Microsoft WSL instructions: https://learn.microsoft.com/en-us/windows/wsl/install

In short:

1. Install WSL (from an elevated PowerShell):

```bash
wsl --install
```

2. Launch Ubuntu after reboot.
3. Create your UNIX username and password.

## Install base apt dependencies

Update package lists, then install the dependencies used by the current local setup:

```bash
sudo apt update
sudo apt install -y \
  build-essential git curl unzip pkg-config \
  rustc libssl-dev libyaml-dev zlib1g-dev libgmp-dev \
  default-libmysqlclient-dev libpq-dev postgresql-client \
  libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev \
  imagemagick libvips shared-mime-info \
  openjdk-17-jre-headless
```

## Install mise

Install and activate `mise` for Bash:

```bash
curl https://mise.run | sh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(mise activate bash)"' >> ~/.bashrc
source ~/.bashrc
```

Verify installation:

```bash
mise --version
mise doctor
```

## Runtime setup model

`mise` is the preferred runtime manager for local development.

- Global Ruby can be installed with `mise` if you want a default outside projects.
- GeoDiscovery uses project-local runtime versions declared in `mise.toml`.
- Current project runtime targets are:
  - `ruby = "3.2.1"`
  - `node = "20"`

The repository also includes `.ruby-version` and Ruby pinning in `Gemfile` for compatibility, but `mise.toml` is the preferred multi-runtime local-dev setup.

## Clone the repository

```bash
git clone git@github.com:UWM-Libraries/GeoDiscovery.git
cd GeoDiscovery
mise install
```

## Configure env files

Copy the local env files:

```bash
cp .example.env.test .env.test
cp .example.env.development .env.development
```

Local development expects:

- Solr on port `8983`
- Rails on port `3000`
- A Redis URL may be present in `.env.development`, but local bootstrap is focused on Solr + Rails unless your feature specifically needs Redis

## Install app dependencies

After copying the `.env` files, bootstrap the app dependencies:

```bash
bundle install
corepack enable
yarn install
```

Notes:

- `package.json` pins Yarn via the `packageManager` field, so use Corepack-managed Yarn instead of globally installed Yarn.
- Node `20` is intentional for this project; the frontend dependency tree requires newer Node than `18`.
- `bin/setup` now runs the JavaScript dependency install for you, but `yarn install` is still the direct command to use when frontend packages or `yarn.lock` change.

## Database setup (SQLite for local dev/test)

```bash
bundle exec rake db:create
bundle exec rake db:migrate
```

For a one-command bootstrap, you can also run:

```bash
bin/setup
```

`bin/setup` now installs both Ruby and JavaScript dependencies before preparing the local database.

## Start the app

```bash
bundle exec rake uwm:server
```

This task starts the local app stack plus the local Solr wrapper. Java must be installed or Solr startup will fail.

If the Rails app opens at `http://localhost:3000` but Solr Admin does not open from the Windows side at `http://localhost:8983/solr/`, WSL may not be forwarding port `8983` cleanly. In that case, open the Solr Admin UI from inside WSL instead:

```bash
xdg-open http://127.0.0.1:8983/solr/
```

This uses the Linux-side browser integration and bypasses the Windows localhost forwarding issue.

## Run tests

Run tests explicitly in the test environment so commands do not run against development data:

```bash
RAILS_ENV=test bundle exec rake test
```

```bash
RAILS_ENV=test bundle exec rails test:system
```

Accessibility checks now run in the system test suite with `axe-core`, including coverage for the homepage and search results.

## Browser requirement for system tests

GeoDiscovery system tests use Capybara with a Chrome-family browser.
Local system-test execution requires Chrome or Chromium installed and discoverable on `PATH`.
Package names can vary by Ubuntu release.

```bash
which google-chrome || which chromium || which chromium-browser
```
