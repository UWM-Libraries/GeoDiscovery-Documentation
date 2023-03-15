---
layout: default
title: Feature Development
nav_exclude: false
---

# Feature Development Workflow

## Create a new branch

```bash
git branch feature/*new-feature-name*
```

## Make changes in the newly created branch

{: .note }
> If you make changes to the gemfile run:
> 
> ```bash
> `bundle install`
> ```
> 
> and
> 
> ```bash
> `bundle update`
> ```
> 
> This will install depdnencies and modify the gemfile.lock file

Commit changes to your local branch:

```bash
git add .
git commit -m "notes about your commits"
```

## Run the ruby linter

This linter will check that our ruby code meets the language specifications

```bash
bundle exec standardrb
```

## Run the test suite

```bash
RAILS_ENV=test bundle exec rake ci
```

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
1. Follow the instructions to [deploy to your local machine](#ADDLINK)
1. Run the [linter](#ADDLINK) and [test suite](#ADDLINK)
1. Start the application and make sure it works
1. Review the changed files from the pull request in GitHub and make any comments.
1. Make revisions or encourage the person who opened the PR to make changes.
1. Merge the pull request into the main branch and delete the feature branch.

## Tag a release

Once the pull request is merged, we should [tag a release](https://git-scm.com/book/en/v2/Git-Basics-Tagging) using semantic tagging.

## Deploy

Deploy to development environment for further testing.





