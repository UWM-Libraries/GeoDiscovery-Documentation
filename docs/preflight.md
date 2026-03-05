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

# Step 0 Complete

Once the server is healthy and services are stable, proceed.


