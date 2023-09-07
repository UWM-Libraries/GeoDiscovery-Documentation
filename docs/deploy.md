---
layout: default
title: Deploy The Application
nav_exclude: false
nav_order: 3
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

It's a good idea to [run tests](develop/#run-the-test-suite) to make sure everything was installed and updated correctly.

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
    
    Deploy to liblamp-dev:

    ```bash
    bundle exec cap development deploy
    ```

    Deploy to liblamp:
    
    ```bash
    bundle exec cap production deploy
    ```

{: .warning }
> Will likely break on first run for new servers and users.
