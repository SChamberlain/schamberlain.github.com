---
name: archived-pkgs
layout: post
title: "stories behind archived packages"
date: 2020-09-10
author: Scott Chamberlain
sourceslug: _drafts/2020-09-10-archived-pkgs.Rmd
tags:
- R
- taxonomy
- database
---



Code is often arranged in packages for any given language. Packages
are often cataloged in a package registry of some kind: NPM for
node, crates.io for Rust, etc. For R, that registry is either
[CRAN](https://cran.r-project.org/) or [Bioconductor](https://bioconductor.org/)
(for the most part).

CRAN has the concept of an archived package. That is, the namespace
for a package (`foo`) is still in the registry (and can not be used again),
but the package is archived - no longer gets updated and checks
I think are no longer performed.

We rarely hear the stories behind how software gets laid to rest. What are
the most common reasons for software to be abandoned?

## My CRAN archived packages

First, my archived CRAN packages:


```r
library(pkgsearch)
library(dplyr)
library(data.table)
library(tibble)
x = cran_events(releases = FALSE, archivals = TRUE, limit = 4000L)
res = lapply(x, function(w)
  tibble(pkg=w$name, maintainer=w$package$Maintainer))
df = rbindlist(res, use.names = TRUE, fill = TRUE)
df = as_tibble(df)
scott = filter(df, grepl("chamberlain", maintainer, ignore.case = TRUE)) %>%
  select(pkg)
scott
#> # A tibble: 16 x 1
#>    pkg        
#>    <chr>      
#>  1 rjsonapi   
#>  2 rdpla      
#>  3 seaaroundus
#>  4 crevents   
#>  5 etseed     
#>  6 rtimes     
#>  7 rsunlight  
#>  8 nneo       
#>  9 binomen    
#> 10 solr       
#> 11 enigma     
#> 12 alm        
#> 13 ropensnp   
#> 14 govdat     
#> 15 spoccutils 
#> 16 rgauges
```

Apparently I have 16 archived packages on CRAN.


## Stories

The following are brief stories of why each package was archived on CRAN.

- **spoccutils**: was a package of utility functions that didn't quite fit within the scope of another package [spocc][]. It was renamed to [mapr][]. I suppose I could have asked CRAN to change the name, hmmm
- **[rgauges][]**: was a client for the [Gaug.es](https://get.gaug.es/) website analytics API - we started the package to gather data on visitors to the rOpenSci website. Eventually we stopped using Gaug.es and then it didn't make sense to maintain the package, so it was archived.
- **[alm][]**: was a client for a generic article-level metrics web service framework called Lagotto. In the early days of rOpenSci we were quite engaged with the community of folks working on article-level metrics. If I remember correctly, Lagotto usage slowed down and wasn't used much so I gave up on maintaining `alm`
- **ropensnp**: was a client for the service [OpenSNP](https://opensnp.org/). There were other sources of SNP data that I thought would be nice to access all from one package; so a new package ([rsnps][]) was created and incorporated functions for OpenSNP and `ropensnp` was archived.
- **solr**: was a package client for the [Apache Solr](https://lucene.apache.org/solr/) database. At some point it got a major overhual and I decided to change the package name to [solrium][].
- **[binomen][]**: aim of the package was functions for creating taxonomic classes/objects and functions for manipulating taxonomic data, sort of like dplyr. Evolution of ideas in `binomen` gave way to a new package [taxa][], now maintained by [Zach Foster](https://github.com/zachary-foster).
- **[nneo][]**: was a client for APIs for [NEON](https://data.neonscience.org/data-api/) data. At some point I stumbled upon someone else from NEON making essentially the same package, so I archived mine.
- **[etseed][]**: was a package for interacting with the distributed key-value store [etcd](https://etcd.io/). I was on a kick at the time of making R packages for databases, and saw a missing package I thought. After getting familiar with etcd, I realized I would probably never use it myself, and further, it probably didn't make sense to interact with etcd from R anyway.
- **[crevents][]**: [Crossref](https://www.crossref.org/) mints DOIs for scholarly articles (among other works). A neat service they started was collecting and making searchable the "events" on DOIs - that is, the links pointing to DOIs, e.g., from Twitter, etc. The service at some point became very unreliable (was often down), so the package was archived.
- **[seaaroundus][]**: [Seaaroundus](http://www.seaaroundus.org/) maintains fisheries and fisheries-related data. I was helping maintain an R package for the API, but it was a difficult one to maintain, and most users simply sent emails requesting dumps of data anyway - so the package was archived.
- **[rdpla][]**: The [Digital Public Library of America](https://dp.la/) is a very cool organization that is similar to Crossref in a way, in that they centralize metadata about "things"; metadata on museum collections for DPLA and scholarly works for Crossref. I figured many researchers would enjoy being able to easily get metadata from DPLA for research on museum collections. In the end not many people used the package.
- **[rjsonapi][]**: [JSON:API](https://jsonapi.org/) is a cool idea - a sort of specification for building APIs in JSON. REST APIs are incredibly variable - this is an attempto standardize it a bit. I thought perhaps JSON:API would be adopted widely and that an R client would be useful for consuming JSON:API services - however, I've seen only very few APIs using JSON:API.

The following four packages were all R clients for sources of government open data - see the organization [rOpenGov](https://github.com/ropengov) for R packages on government data.

- **govdat**: was split into two packages (`rsunlight` and `rtimes`) and `govdat` was archived
- **[rsunlight][]**: was a client for many APIs of the organization [Sunlight Labs](https://sunlightfoundation.com/api/) - part of the reason for archiving this package was the disintegration of Sunlight Labs, which made the previously sensible organization of many APIs into one R package not sensible anymore. Also, government data was considered out of scope for our work at rOpenSci.
- **[rtimes][]**: was a client for a number of the government data APIs from the New York Times. One reason for abandoning this package was that NYT almost never responded to questions/feedback on their APIs. Another reason was the aforementioned focus of rOpenSci.
- **[enigma][]**: was a client for the Enigma API - the company I think was first focused on making open government data easier to access - as well as data on companies. I didn't really use the package at all though, and there wasn't much usage of the package, so archived it.


As a summary of the lists above, a list of the major reasons each package was archived:

- Not used
  - rjsonapi
  - rdpla
  - etseed
  - enigma
  - rgauges
  - rtimes
- Bad/retired service
  - rtimes
  - seaaroundus
  - crevents
  - alm
- Name change
  - solr
  - ropensnp
  - spoccutils
- Evolution to new package
  - binomen
  - govdat
- Duplicated work
  - nneo
- Out of scope
  - rsunlight



[mapr]: https://github.com/ropensci/mapr
[spocc]: https://github.com/ropensci/spocc
[rsnps]: https://github.com/ropensci/rsnps
[rsunlight]: https://github.com/rOpenGov/rsunlight
[rtimes]: https://github.com/rOpenGov/rtimes
[enigma]: https://github.com/rOpenGov/enigma
[solrium]: https://github.com/ropensci/solrium
[binomen]: https://github.com/ropensci/binomen
[taxa]: https://github.com/ropensci/taxa
[nneo]: https://github.com/ropensci-archive/nneo
[seaaroundus]: https://github.com/ropensci-archive/seaaroundus
[rgauges]: https://github.com/ropensci-archive/rgauges
[alm]: https://github.com/ropensci-archive/alm
[etseed]: https://github.com/ropensci-archive/etseed
[crevents]: https://github.com/ropensci-archive/crevents
[rdpla]: https://github.com/ropensci-archive/rdpla
[rjsonapi]: https://github.com/ropensci-archive/rjsonapi