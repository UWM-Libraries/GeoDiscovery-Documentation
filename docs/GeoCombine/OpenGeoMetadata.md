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
This does not seem to work on Stephen's local install.

If that doesn't work, you can navigate into the individual repos in `./tmp/opengeometadata` and do a fetch and pull directly.