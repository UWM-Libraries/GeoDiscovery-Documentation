---
title: Converting Metadata
layout: default
nav_order: 2
parent: GeoCombine
---

# Converting Metadata with GeoCombine

At first inspection, it looks like this is a one-by-one kind of thing.

# Use the interactive rails console:

in your shell, navigate to the GeoDiscovery directory and run `rails console`

You can then run operations like creating an ISO metadata object:

```bash
iso_metadata =  GeoCombine::Iso19139.new('./tmp/opengeometadata/ISO_Metadata.xml')
```

{: .warning }
The current [XSLT in the UWM GeoCombine fork](https://github.com/UWM-Libraries/GeoCombine/blob/main/lib/xslt/iso2geoBLAardvark.xsl) uses XML 2.0 features that are not supported by Nokogiri, and by extension, GeoCombine.
