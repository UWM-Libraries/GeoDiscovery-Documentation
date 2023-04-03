---
title: Using GeoCombine for OpenGeoMetadata
layout: default
nav_order: 1
parent: GeoCombine
---

# OpenGeoMetadata and GeoCombine

See the
[documentation page](https://opengeometadata.org/)
for OpenGeoMetadata

See the
[repository](https://github.com/OpenGeoMetadata/GeoCombine)
for GeoCombine

## Clone all OGM repos to /tmp:

```ruby
bundle exec rake geocombine:clone
```

## Clone a specific OGM repo to /tmp:

```ruby
bundle exec rake geocombine:clone[edu.stanford.purl]
```

## Index cloned repos into Solr

```ruby
bundle exec rake geocombine:index
```

## Update local OpenGeoMetadata repositories

Runs `git pull origin master` 
on all cloned repositories in 
`./tmp/opengeometadata`

```ruby
bundle exec rake geocombine:pull
```

{: .warning }
If this doesn't work, ensure your current user has access to the `.git/` directory of the repository. 