---
layout: default
title: Feature Development
nav_exclude: false
nav_order: 3
---

# Feature Development Workflow

First, you will need to [clone the repository](deploy/#clone-the-repository)
and [configure the .env files](deploy/#configure-env-files).
You may wish to go through the entire [deploy](deploy) cycle to ensure
you're starting with a working application to develop on.

## Create a new branch

```bash
git branch new-branch-name
```

{: .note-title }
> Branch Names
> 
> Use consistent branch naming for more opaque pull requests and flow.
>
> For new features use:
> 
> `feature/feature-name`
>
> e.g. `feature/new-button`
> 
> For bug fixes use:
> 
> `bugfix/bug-name`
>
> e.g. `bugfix/homepage-map`
> 
> Pull requests automatically created by Dependabot will use:
>
> `dependabot/bundler/gem-being-updated`
>
> .e.g `dependabot/bundler/sidekiq-7.0.7`
> 
> Using these consistently will help us keep track of branches and why they were created

## Make changes in the newly created branch

Commit changes to your local branch:

```bash
git add .
git commit -m "notes about your commits"
```

{: .note-title }
> bundle update & install
>
> If you make changes to the gemfile run:
> 
> ```bash
> bundle install
> ```
> 
> and
> 
> ```bash
> bundle update
> ```
> 
> This will install depdnencies and modify the gemfile.lock file.
> Do this before pushing to ensure all changes are captured in your push.

## Run the ruby linter

This linter will check that our ruby code meets the language specifications

```bash
bundle exec standardrb
```

if the linter detects problems, you can usually fix them with

```bash
bundle exec standardrb --fix
```

## Run the test suite

```bash
RAILS_ENV=test bundle exec rake ci
```

{: .warning }
> Ensure that the server is running when you run the tests.
> You may need to open a second terminal to do this.

Green lines are passed tests, red are failed and will tell you which line broke .

{: .note }
See the [tests directory](https://github.com/UWM-Libraries/GeoDiscovery/tree/main/test) in the repo.

## Push your branch to GitHub

```bash
git push
```

## Open a Pull Request 

Open a pull request in git hub.

Assign a reviewer. It's a good idea to have someone else inspect any changes you made.

The review process might look like this:

1. Pull the new feature branch to your local machine
1. Follow the instructions to [deploy to your local machine](https://uwm-libraries.github.io/GeoDiscovery-Documentation/docs/deploy.html#locally)
1. Run the [linter](#run-the-ruby-linter) and [test suite](#run-the-test-suite)
1. Start the application and make sure it works
1. Review the changed files from the pull request in GitHub and make any comments.
1. Make revisions or encourage the person who opened the PR to make changes.
1. Merge the pull request into the main branch and delete the feature branch.

## Tag a release

Once the pull request is merged, we should [tag a release](https://git-scm.com/book/en/v2/Git-Basics-Tagging) using semantic tagging.

## Deploy

[Deploy to development](https://uwm-libraries.github.io/GeoDiscovery-Documentation/docs/deploy.html#deploy-to-the-liblamp-dev-or-liblamp) environment for further testing.





