---
title: Editing This Documentation
layout: default
nav_order: 999
---

# Updating Geodiscovery Documentation Site

These instructions are for making updates to this documentation site.

For general information about how to use the template, see the [documentation for Just the Docs](https://just-the-docs.github.io/just-the-docs/).

## Minor updates:

1. (This step first time only.) Clone the repository to your computer
    * Open GitBash and navigate to the directory where you want to store the repo.
    * Clone the repo, keeping the default name: `git clone git@github.com:UWM-Libraries/GeoDiscovery-Documentation.git`
1. (Skip this step the first time.) If you already have the repository cloned on your computer, use `git fetch` to fetch any updates hosted on github and then use `git pull` to pull down any existing changes to your local clone.
1. Open the cloned directory with your favorite code editor and make your edits
    * Save changes locally.
    * Make commits to the local clone as you go.
1. For minor changes, push to the main branch.

## Major Updates:

1. (This step first time only.) Clone the repository to your computer.
    * Open GitBash and navigate to the directory where you want to store the repo.
    * Clone the repo, keeping the default name: `git clone git@github.com:UWM-Libraries/GeoDiscovery-Documentation.git`
1. (Skip this step the first time.) If you already have the repository cloned on your computer, use `git fetch` to fetch any updates hosted on github and then use `git pull` to pull down any existing changes to your local clone.
1. Start a new branch: `git branch new-feature-branch`
1. Commit changes to the new branch in your local clone
1. Push changes to github and make a Pull Request to merge your new branch with the main branch.
1. have someone review the pull request and approve merge.
1. After the PR is merged, delete your new branch.
