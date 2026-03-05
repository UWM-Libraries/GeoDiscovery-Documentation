# GeoDiscovery Deployment Preflight – Step 0: Server Health

Before running a Capistrano deploy to `liblamp8`, perform the following checks to ensure the server is stable and unlikely to fail during deployment.

These steps help prevent common issues such as:

- Solr crashes
- Redis failures
- Sidekiq memory pressure
- Deploy failures caused by swap exhaustion

---

# 0.1 Basic host health

SSH into the server and verify general system status.

```bash
hostname -f
date
uptime
df -h
free -h
````

Confirm:

* Load averages are low
* Disk space is available
* Memory is not critically exhausted

Pay special attention to:

* `/var` (deploy path lives here)
* `/home` (can affect deploy user operations)

---

# 0.2 Check swap usage

```bash
swapon --show
```

If swap is **completely full**, deployments may trigger instability or service crashes.

---

# 0.3 Identify top memory consumers

```bash
ps aux --sort=-%mem | head -n 15
```

Typical large consumers on GeoDiscovery servers:

* `sidekiq`
* `solr`
* `Passenger RubyApp`
* `httpd`

If Sidekiq is consuming multiple GB of RAM, consider restarting it before deploy.

---

# 0.4 Verify application services are running

```bash
sudo systemctl status sidekiq --no-pager
sudo systemctl status solr --no-pager
sudo systemctl status httpd --no-pager
sudo systemctl status redis --no-pager
```

Expected state:

* `sidekiq` → active (running)
* `solr` → running
* `httpd` → running
* `redis` → running

---

# 0.5 Verify Solr responds

```bash
curl -sSf http://localhost:8983/solr/ >/dev/null && echo "solr ok"
```

Expected output:

```
solr ok
```

---

# 0.6 Check if Sidekiq is actively processing jobs

```bash
sudo journalctl -u sidekiq --since "30 min ago" --no-pager | tail -n 80
```

If there are **no recent entries**, it is safe to restart Sidekiq to clear memory pressure.

---

# 0.7 Restart Sidekiq (if memory usage is high)

This can reclaim several gigabytes of RAM before a deploy.

```bash
sudo systemctl restart sidekiq
```

Verify restart:

```bash
sudo systemctl status sidekiq --no-pager
sudo journalctl -u sidekiq --since "5 min ago" --no-pager
```

---

# 0.8 Re-check memory and swap

```bash
free -h
swapon --show
```

Goal:

* More available RAM
* Lower risk of deploy-triggered service crashes

---

Once the server is healthy and services are stable, proceed.

# Step 1 – Ruby / Bundler / Nokogiri readiness (prod)

Run on `liblamp8`:

```bash
ruby -v
bundle -v || bundler -v
which ruby
which bundle || which bundler
````

Then confirm Bundler config from the deployed app directory:

```bash
cd /var/www/rubyapps/uwm-geoblacklight/current
pwd
bundle config
```

Sanity-check Nokogiri loads (catches native extension / libxml2 issues):

```bash
cd /var/www/rubyapps/uwm-geoblacklight/current
bundle exec ruby -e 'require "nokogiri"; puts "nokogiri #{Nokogiri::VERSION}"'
```

**Expected/confirmed on `liblamp8`:**

* Ruby (RVM): `ruby 3.2.1`
* Bundler: `2.5.16`
* Bundler `deployment = true`
* Bundler `without = [:development, :test]`
* Nokogiri loads cleanly: `nokogiri 1.17.2`

---

# Step 2 – Node / Yarn / Rails tooling readiness (prod)

Verify JS toolchain is installed and on PATH:

```bash
node -v
npm -v
yarn -v || true
which node
which yarn || true
```

Confirm Rails is available in the deployed app:

```bash
cd /var/www/rubyapps/uwm-geoblacklight/current
bin/rails -v
```

Because Capistrano runs commands in a non-interactive shell, verify versions there too:

```bash
bash -lc 'node -v && npm -v && yarn -v'
```

**Expected/confirmed on `liblamp8`:**

* Node: `v20.20.0`
* npm: `10.8.2`
* yarn: `1.22.22`
* Rails: `7.2.2.1`
* Non-interactive shell sees node/npm/yarn correctly

---

# Step 3 – Redis + Solr health checks (prod)

## Redis

Verify Redis is up and responding:

```bash
redis-cli ping
```

Snapshot Redis memory use:

```bash
redis-cli info memory | head -n 40
```

**Expected/confirmed on `liblamp8`:**

* `PONG`
* Memory use ~`40MB` (healthy)

## Solr

Verify Solr responds on localhost:

```bash
curl -sSf http://localhost:8983/solr/ >/dev/null && echo "solr ok"
```

Check recent Solr log activity for obvious errors:

```bash
sudo tail -n 120 /var/solr/logs/solr.log
```

**Expected/confirmed on `liblamp8`:**

* `solr ok`
* Logs show normal query traffic (no obvious errors)


# Step 4 — Production Deployment (Tag-Based)

## 1. Quiet Sidekiq (production server)

Prevent Sidekiq from taking new jobs during deploy.

```bash
sudo systemctl kill -s TSTP sidekiq
sudo systemctl status sidekiq --no-pager
```

Sidekiq will finish current jobs but stop accepting new ones.

---

## 2. Deploy the tagged release (workstation)

Always deploy using an explicit tag.

```bash
cd ~/GeoDiscovery
git fetch --tags
bundle exec cap production deploy BRANCH=v4.5.5
```

Capistrano will:

- create a new release under `releases/`
- install gems
- install yarn packages
- compile assets
- update the `current` symlink
- restart Passenger

{: .warning }
> ### Bundler deployment failure: `Ignoring <gem> because its extensions are not built`
>
> During a Capistrano deployment, `bundle install` may fail with a series of warnings followed by `Bundler::GemNotFound` errors. The log output typically looks like:
>
> ```
> Ignoring bootsnap-1.18.4 because its extensions are not built.
> Ignoring msgpack-1.7.5 because its extensions are not built.
> ...
> Bundler::GemNotFound: Could not find tzinfo-2.0.6.gem for installation
> Bundler::GemNotFound: Could not find builder-3.3.0.gem for installation
> ```
>
> Although the errors appear to indicate missing gems, the underlying problem is usually an **inconsistent or partially built Bundler environment** on the server. This commonly occurs when:
>
> - A previous deployment was interrupted
> - Native extensions failed to compile
> - The Ruby or Bundler version changed
> - The shared bundle cache became corrupted
>
> In Capistrano deployments, gems are typically installed into a shared directory such as:
>
> ```
> /var/www/<application>/shared/bundle
> ```
>
> If this shared bundle becomes inconsistent, Bundler may repeatedly ignore gems whose native extensions are not compiled and ultimately fail to resolve required dependencies.
>
> See the related issue for remediation steps.
> 

---

## 3. Resume Sidekiq (production server)

After deploy completes:

```bash
sudo systemctl kill -s CONT sidekiq
sudo systemctl status sidekiq --no-pager
```

This allows Sidekiq to begin processing jobs again.

---

## 4. Post-deploy smoke checks

Verify application dependencies are healthy.

```bash
# confirm current release
readlink -f /var/www/rubyapps/uwm-geoblacklight/current

# check Solr
curl -sSf http://localhost:8983/solr/ >/dev/null && echo "solr ok"

# check Redis
redis-cli ping
```

Expected results:

```
solr ok
PONG
```

---

## 5. Basic application test

Verify the application is functioning:

- Load homepage
- Run a search
- Open a record page





