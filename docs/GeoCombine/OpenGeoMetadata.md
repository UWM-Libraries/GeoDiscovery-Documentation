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

You can check to see what repositories are cloned in
`../GeoDiscovey/tmp/opengeometadata`

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
> On your local environment, the `SOLR_URL` is set to `http://127.0.0.1:8983/solr/blacklight-core` via the `.env.test` and `.env.development` files using the `dot-env` gem.
>
> In production, this could be set by either using a `.env.production` file or by specifying in the rake command: 
>
> ```ruby
> SOLR_URL=http://127.0.0.1:8983/solr/test bundle exec rake geocombine:index
>```
> 

## Update local OpenGeoMetadata repositories

```ruby
bundle exec rake geocombine:pull
```

Runs `git pull origin master` 
on all cloned repositories in 
`GeoDiscovery/tmp/opengeometadata`

{: .warning }
If this doesn't work, ensure your current user has read/write access to the `.git/` directory of the repository. 
Permission issues can arrise.