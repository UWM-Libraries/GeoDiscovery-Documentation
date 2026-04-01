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

## Clone all OGM repos to the local harvest root

```ruby
bundle exec rake geocombine:clone
```

However, this gives us no control over what repositories we pull down.

## Clone a specific OGM repo

```ruby
bundle exec rake geocombine:clone[edu.stanford.purl]
```

This use case is more common.

You can check to see what repositories are cloned in the configured harvest root,
typically `tmp/opengeometadata` in local development.

Example:

```bash
ls tmp/opengeometadata/
edu.berkeley   edu.columbia  edu.cornell   edu.illinois  edu.indiana
edu.msu        edu.nyu       edu.osu       edu.princeton.arks  edu.purdue
edu.rutgers    edu.stanford.purl  edu.uchicago  edu.uiowa  edu.umd
edu.umich      edu.umn       edu.unl       edu.utexas     edu.uwm
edu.wisc
```

{: .note }
> Some repositories already publish Aardvark metadata directly.
>
> Others are still legacy GBL 1.0 sources and are converted locally by `uwm:opendataharvest:gbl1_to_aardvark` during the weekly pipeline.

## Update local OpenGeoMetadata repositories

```bash
bundle exec rake geocombine:pull[repo]
```

This will update (via `git pull`) the specified repo.

{: .warning }
Running it without a specified repo may pull more repositories than the curated set used by GeoDiscovery.


## Index cloned repos into Solr

```ruby
bundle exec rake geocombine:index
```

GeoDiscovery also provides a higher-level task:

```ruby
bundle exec rake uwm:geocombine_pull_and_index
```

That combined task pulls the configured repositories, harvests DCAT metadata, converts legacy records, normalizes harvested Aardvark metadata, indexes into Solr, and prunes stale Solr records.

{: .warning }
> Depending on your environment, you may need to set...
> ```bash
> RAILS_ENV=production
> SCHEMA_VERSION=Aardvark
> SOLR_URL=http://127.0.0.1:8983/solr/blacklight-core
> OGM_PATH=tmp/opengeometadata
> ```
>
> `OGM_PATH` can also be pointed at a narrower subtree when you want to target a specific institution or metadata subset.
> 

{: .note}
> On your local environment, the `SCHEMA_VERSION` is set to `Aardvark` via the `.env.test` and `.env.development` files using the `dot-env` gem.
> 
> On your local environment, the `SOLR_URL` is set to `http://127.0.0.1:8983/solr/blacklight-core` via the `.env.test` and `.env.development` files using the `dot-env` gem.
>
> In production, these could be set by either using a `.env.production` file or by specifying in the rake command, for example:
>
> ```bash
> SCHEMA_VERSION=Aardvark SOLR_URL=http://127.0.0.1:8983/solr/blacklight-core bundle exec rake geocombine:index
> ```

{: .warning }
If these functions don't work, ensure your current user has read/write access to the `.git/` directory of the targeted repo. 
Permission issues can arise.

