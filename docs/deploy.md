---
layout: default
title: Deploy The Application
nav_exclude: false
nav_order: 3
---

# Deploy the application

## Locally:

### Clone the repository

```bash
git clone git@github.com:UWM-Libraries/GeoDiscovery.git
```

```bash
cd GeoDiscovery
```

### Configure .env Files

1. Copy the example .env files:

    ```bash
    cp .example.env.test .env.test
    ```

    ```bash
    cp .example.env.development .env.development
    ```

1. After you copy the files, update them to include your database and solr connections
    * Solr lives on port 8983
    * Geoblacklight 3000

### Bundle dependencies

The application's [RubyGem](https://rubygems.org/) dependencies are listed in the project `Gemfile`. 

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

### Start the application on a local computer

```bash
bundle exec rake uwm:server
```

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
    
1. Set up shared directory to add database and solr credentials on the server that is hosting the instance of software (First time only)

1. Run capistrano command to deploy to directory.
    * `bundle exec cap development deploy` will deploy to liblamp-dev
    * `bundle exec cap production deploy` will deploy to liblamp

{: .warning }
Will likely break on first run for new servers and users.
