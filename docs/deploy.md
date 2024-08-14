---
layout: default
title: Deploy The Application
nav_exclude: false
nav_order: 2
---

# Deploy the application

## On a local computer:

### Clone the repository

```bash
git clone git@github.com:UWM-Libraries/GeoDiscovery.git
```

```bash
cd GeoDiscovery
```

{: .note }
Ensure you've set up Git in your development environment and have set up your SSH key on GitHub.

### Configure .env Files

Copy the example .env files:

    ```bash
    cp .example.env.test .env.test
    ```

    ```bash
    cp .example.env.development .env.development
    ```

After you copy the files, update them to include your database and solr connections
* Solr port 8983
* Geoblacklight port 3000

### Bundle dependencies

The application's [RubyGem](https://rubygems.org/)
dependencies are listed in the project
[`Gemfile`](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/Gemfile). 

Bundle the gems via this command:

```bash
bundle install
```

If the `Gemfile` has changed recently, you might need to run an additional command to get all the latest updates:

```bash
bundle update
```

### Database initialization

You'll need a local database for development and test modes. Both can just use sqlite.

```bash
bundle exec rake db:create
```

```bash
bundle exec rake db:migrate
```

### Start the application

```bash
bundle exec rake uwm:server
```

You should now be able to interact with the application in the browser at
`http://localhost:3000`

or via the command line by running `rails console` in the GeoDiscovery directory
while the app is running.

It's a good idea to [run tests](develop.html#run-the-test-suite) to make sure everything was installed and updated correctly.

You can now do [feature development](develop) on your local machine to test out changes or new features.

## Deploy to the liblamp-dev or liblamp:

1. Create a SSL Key pair or check if you have existing ssh keys

    ```bash
    ls -al ~/.ssh
    ```

    If you get `...no such file or directory` you likley have no SSH keys set up.

    ```bash
    ssh-keygen -t rsa -C "your@email.address"
    ```

    Save the keys in the default directory, likley `~/.ssh`


1. Deploy Public SSL Key to server to enable password-less login.

    Start the ssh-agent:

    ```bash
    eval "$(ssh-agent -s)"
    ```
    
    Adding SSH private keys into the SSH authentication agent:

    ```bash
    ssh-add ~/.ssh/id_rsa
    ```
    
    {: .note }
    This makes Capistrano recognize your SSL key. If this isn’t run, you’ll get an error: “fatal: Could not read from remote repository.” [More Info](https://forum.upcase.com/t/capistrano-cannot-read-from-remote-repository/3009/3)

    ```bash
    cat ~/.ssh/id_rsa.pub
    ```

    Add public key to /home/ad.uwm.edu/<Username>/.ssh/authorized_keys as the geoblacklight user on the remote server

    {: .note }
    > You can use RSA style SSH keys or ed25519 style.
    >
    > To use ed25519 style, simply replace `rsa` with `ed25519` in the steps above.
    > 
    > We added some gems to the gemfile to allow this functionality:
    > 
    > ```ruby
    > # ED SSH Key support
    > gem "ed25519", ">=1.2", "< 2.0"
    > gem "bcrypt_pbkdf", "~> 1.0", "< 2.0"
    > ```
    
1. Set up shared directory to add database and solr credentials on the server that is hosting the instance of software (First time only)

1. Run capistrano command to deploy to directory.

{: .warning }
> Will likely break on first run for new servers and users. This is normal, read error logs and warnings to troubleshoot dependecy issues, etc.
    
Deploy to liblamp-dev:

```bash
bundle exec cap development deploy
```

Deploy to liblamp:

```bash
bundle exec cap production deploy
```

These commands will run [Capistrano](dependencies.html#capistrano) and deploy the application to development
or production respectively.

If it's sucessful, it will be saved in the current-release directory found at /var/www/rubyapps/uwm-geoblacklight/current/.
The directory is actually a shortcut to /var/www/rubyapps/uwm-geoblacklight/releases/_latest_ where _latest_ is represented
by a numerical version number according to it's creation date, e.g. `20240612212046/`

{: .note }
> OpenGeoMetadata files stored in /tmp/opengeometadata are not moved over to the new release as part of the deploy process.
> [.env.production](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/.example.env.production)
> should be modified to point [GeoCombine's](dependencies/.html#geocombine)
> OGM_PATH environment variable to /var/www/rubyapps/uwm-geoblacklight/shared/tmp/opengeometadata.
>
> ```bash
> OGM_PATH=/var/www/rubyapps/uwm-geoblacklight/shared/tmp/opengeometadata
> ```

{: .note }
> If you get the following error:
>  ERROR linked file
> /var/www/rubyapps/uwm-geoblacklight/shared/config/blacklight.yml
> does not exit...
> There are three files in that dir that need to
> be copied over. 
> 1. "config/blacklight.yml"
> 2. "config/database.yml"
> 3. "config/master.key"

### Configure Cron jobs with Whenever

Since schedule.rb, the file that Whenever uses to set up Cron jobs, is committed to version control,
we might need to modify it depending on the environment you're deploying to.

For example, I do not want the Cron job that pulls down metadata from OpenGeoMetadata to run on the 
development site while testing new functionality for GeoCombine. 
You can edit the [schedule.rb](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/config/schedule.rb)
file in the current release on LibLamp-Dev or LibLamp and then write the changes to the system's
crontab by running...

```bash
whenever --write-crontab
```

or for short,

```bash
whenever -w
```

{: .note }
> The Readme file on the Whenever gem's repository implies that `Whenever --update-crontab` is the command to use, but when I used
> `Whenever -help`, the appropriate command was the one used above. Not sure why there is this inconsistency. You can always
> double check the system's crontab at /var/spool/cron/geoblacklight to make sure your changes are reflected.
> 

## Redis and Sidekiq

The GBL application enques jobs to Sidekiq from a Redis list, sidekiq is supposed to be monitoring redis for new jobs.

You can check if things are working with `bundle exec sidekiqmon` which should bring up an overview of sidekiq's status.
If that doesn't work, it's likley that either Sidekiq or Redis is not running.

You can see if the redis service is failing with `systemctl --failed` and check to see if it's there.
If it is showing failed, restart redis with `sudo systemctl restart redis`.

You can run sidekiq with `bundle exec sidekiq` but daemonizing it makes more sense:

### Daemonizing sidekiq:
based on [this blog](https://deepakdargade.medium.com/sidekiq-in-production-8adf2e6b9f7c)

- Create a new file for Sidekiq service:

    `sudo nano /lib/systemd/system/sidekiq.service`

- Add the following content in the service file:

    ```shell
    #
    # This file tells systemd how to run Sidekiq as a 24/7 long-running daemon.
    #
    # Customize this file based on your bundler location, app directory, etc.
    #
    # If you are going to run this as a user service (or you are going to use capistrano-sidekiq)
    # Customize and copy this to ~/.config/systemd/user
    # Then run:
    #   - systemctl --user enable sidekiq
    #   - systemctl --user {start,stop,restart} sidekiq
    # Also you might want to run:
    #   - loginctl enable-linger username
    # So that the service is not killed when the user logs out.
    #
    # If you are going to run this as a system service
    # Customize and copy this into /usr/lib/systemd/system (CentOS) or /lib/systemd/system (Ubuntu).
    # Then run:
    #   - systemctl enable sidekiq
    #   - systemctl {start,stop,restart} sidekiq
    #
    # This file corresponds to a single Sidekiq process.  Add multiple copies
    # to run multiple processes (sidekiq-1, sidekiq-2, etc).
    #
    # Use `journalctl -u sidekiq -rn 100` to view the last 100 lines of log output.
    #
    [Unit]
    Description=sidekiq
    # start us only once the network and logging subsystems are available,
    # consider adding redis-server.service if Redis is local and systemd-managed.
    After=syslog.target network.target
    
    # See these pages for lots of options:
    #
    #   https://www.freedesktop.org/software/systemd/man/systemd.service.html
    #   https://www.freedesktop.org/software/systemd/man/systemd.exec.html
    #
    # THOSE PAGES ARE CRITICAL FOR ANY LINUX DEVOPS WORK; read them multiple
    # times! systemd is a critical tool for all developers to know and understand.
    #
    [Service]
    #
    #      !!!!  !!!!  !!!!
    #
    # As of v6.0.6, Sidekiq automatically supports systemd's `Type=notify` and watchdog service
    # monitoring. If you are using an earlier version of Sidekiq, change this to `Type=simple`
    # and remove the `WatchdogSec` line.
    #
    #      !!!!  !!!!  !!!!
    #
    Type=notify
    NotifyAccess=all
    # If your Sidekiq process locks up, systemd's watchdog will restart it within seconds.
    WatchdogSec=10
    
    WorkingDirectory=/var/www/rubyapps/uwm-geoblacklight/current
    # If you use rbenv:
    # ExecStart=/bin/bash -lc 'exec /home/deploy/.rbenv/shims/bundle exec sidekiq -e production'
    # If you use the system's ruby:
    # ExecStart=/usr/local/bin/bundle exec sidekiq -e production
    # If you use rvm in production without gemset and your ruby version is 2.6.5
    # ExecStart=/home/deploy/.rvm/gems/ruby-2.6.5/wrappers/bundle exec sidekiq -e production
    # If you use rvm in production with gemset and your ruby version is 2.6.5
    # ExecStart=/home/deploy/.rvm/gems/ruby-2.6.5@gemset-name/wrappers/bundle exec sidekiq -e production
    # If you use rvm in production with gemset and ruby version/gemset is specified in .ruby-version,
    # .ruby-gemsetor or .rvmrc file in the working directory
    ExecStart=/usr/local/rvm/bin/rvm in /var/www/rubyapps/uwm-geoblacklight/current do bundle exec sidekiq -e production
    
    # Use `systemctl kill -s TSTP sidekiq` to quiet the Sidekiq process
    
    # Uncomment this if you are going to use this as a system service
    # if using as a user service then leave commented out, or you will get an error trying to start the service
    # !!! Change this to your deploy user account if you are using this as a system service !!!
    # User=deploy
    # Group=deploy
    # UMask=0002
    
    # Greatly reduce Ruby memory fragmentation and heap usage
    # https://www.mikeperham.com/2018/04/25/taming-rails-memory-bloat/
    Environment=MALLOC_ARENA_MAX=2
    
    # if we crash, restart
    RestartSec=1
    Restart=always
    
    # output goes to /var/log/syslog (Ubuntu) or /var/log/messages (CentOS)
    StandardOutput=syslog
    StandardError=syslog
    
    # This will default to "bundler" if we don't specify it
    SyslogIdentifier=sidekiq
    
    [Install]
    WantedBy=multi-user.target
    # Uncomment this and remove the line above if you are going to use this as a user service
    # WantedBy=default.target
    ```

{: .note-title }
> Customizations in service file
>
> `WorkingDirectory=/var/www/rubyapps/uwm-geoblacklight/current`
>`ExecStart=/usr/local/rvm/bin/rvm in /var/www/rubyapps/uwm-geoblacklight/current do bundle exec sidekiq -e production`

- Reload the systemctl daemon for the new created service

    `sudo systemctl daemon-reload`

- Enable the sidekiq service:

    `sudo systemctl enable sidekiq`

- Now we can start the Sidekiq service:

    `sudo systemctl start|stop|restart sidekiq`

If you have completed the last step, you have successfully created the sidekiq service and you can start using this.



