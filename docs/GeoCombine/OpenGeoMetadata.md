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

{: .note}
> On your local environment, the `SCHEMA_VERSION` is set to `Aardvark` via the `.env.test` and `.env.development` files using the `dot-env` gem.
> 
> In production, this could be set by either using a `.env.production` file or by specifying in the rake command:
> 
> ```ruby
> SCHEMA_VERSION=Aardvark bundle exec rake geocombine:index
> ```
> 

{: .note}
> We may need to specify a [Solr](../dependencies.md/#apache-solr) instance with the `SOLR_URL` variable. For example:
> 
> ```ruby
> SOLR_URL=http://127.0.0.1:8983/solr/test bundle exec rake geocombine:index
>```
> 

## Update local OpenGeoMetadata repositories

Runs `git pull origin master` 
on all cloned repositories in 
`./tmp/opengeometadata`

```ruby
bundle exec rake geocombine:pull
```

{: .warning }
If this doesn't work, ensure your current user has access to the `.git/` directory of the repository. 