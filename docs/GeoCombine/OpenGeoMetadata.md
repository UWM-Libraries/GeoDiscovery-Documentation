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

However, this gives us no control over what repositories we pull down.

## Clone a specific OGM repo to /tmp:

```ruby
bundle exec rake geocombine:clone[edu.stanford.purl]
```

This use case is more common.

You can check to see what repositories are cloned in
`../GeoDiscovey/tmp/opengeometadata`

Right now these are the repositories being regularly pulled down:

```bash
ls tmp/opengeometadata/
edu.illinois  edu.msu  edu.osu  edu.purdue   edu.uchicago  edu.umd    edu.umn  edu.uwm  edu.wisc.aardvark
edu.indiana   edu.nyu  edu.psu  edu.rutgers  edu.uiowa     edu.umich  edu.unl  edu.uwm.converted
```

{: .note }
> edu.wisc.aardvark is a repository on the UWM Libraries GitHub with a version of UW-Madison's Metadata
>
> edu.uwm.converted is a repository on the UWM Libraries GitHub with a version of other institutions' metadata
>
> Both of these repositories have Aardvark version metadata that was converted by UWM Libraries staff from GBL 1.0 version metadata.
>
> These repositories can be cloned with git directly, rather than a rake task.
> Once they are cloned, they can be updated just like the other repos with:
> 
> ```bash
> bundle exec rake geombine:pull[edu.wisc.aardvark]
> ```

## Update local OpenGeoMetadata repositories

```bash
bundle exec rake geocombine:pull[repo]
```

This will update (via `git pull`) the specified repo.

{: .warning }
Running it without a specified repo seems functionally equivalent to `rake geocombine:clone` which will
pull everything from OGM, even repos we might not want!


## Index cloned repos into Solr

```ruby
bundle exec rake geocombine:index
```

{: .warning }
> Depending on your environment, you may need to set...
> ```bash
> RAILS_ENV=production
> SCHEMA_VERSION=Aardvark
> SOLR_URL=http://127.0.0.1:8983/solr/blacklight-core
> OGM_PATH=tmp/opengeometadata/edu.uwm/metadata-aardvark/
> ```
>
> The last environment variable is also useful for targeting specific directories for indexing.
> But could have wider use managing the location of stored JSON metadata documents.
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


