---
layout: default
title: Deploy application
nav_exclude: false
---

# Deploy the application

Deploying locally will be `bundle exec rake uwm:server`

* Solr lives on port 8983
* Geoblacklight 3000

## Deploy to the liblamp-dev or liblamp:

1. Deploy Public SSL Key to server to enable password-less login.
    * Run `eval "$(ssh-agent -s)"` This starts the ssh-agent.
    * Run `ssh-add ~/.ssh/id_rsa` This makes Capistrano recognize your SSL key. If this isn’t run, you’ll get an error: “fatal: Could not read from remote repository.” [More Info](https://forum.upcase.com/t/capistrano-cannot-read-from-remote-repository/3009/3)
    * Add public key to /home/ad.uwm.edu/<Username>/.ssh/authorized_keys as the geoblacklight user on the remote server
    <p>
1. Set up shared directory to add database and solr credentials on the server that is hosting the instance of software (First time only)

1. Run capistrano command to deploy to directory.
    * `bundle exec cap development deploy` will deploy to liblamp-dev
    * `bundle exec cap production deploy` will deploy to liblamp

Will likely break on first run for new servers and users.
