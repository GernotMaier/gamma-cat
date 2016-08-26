# gamma-cat

An open catalog of gamma-ray sources.

* Repo: https://github.com/gammapy/gamma-cat
* Webpage: https://gammapy.github.io/gamma-cat

This is work in progress.
Feedback and contributions welcome!

For now, we just collect TeV gamma-ray source information.
Later we might try to ingest and interconnect other catalogs
(e.g. Fermi-LAT GeV catalogs).

## Why? There's already TeVCat!

Yes, there is http://tevcat.uchicago.edu/ .

But TeVCat isn't open. You can view the info on their webpage,
and copy & paste individual numbers, but you can't download a catalog
and use it for your research.

To quote http://tevcat.uchicago.edu/terms.html (accessed August 26, 2016):

> Users may not perform systematic downloads of TeVCat data for any purpose, 
whether commercial or not, without the written permission of the
TeVCat team.  This includes scripted parsing of the website data,
webpage 'scraping', and the use of robots. If you need access to bulk 
TeVCat data products, please contact the TeVCat team at tevcat@gmail.com

A second major problem with TeVCat is that there's no version history.
Updates and corrections happen, but only the maintainers know when
that happens and (presumably) what the older values were.

The goal here is to have a fully open TeV catalog that you can download
and use as you like (well, we still require attribution, see next section).

Open and reproducible research for gamma-ray astronomy!

The concrete motivation for Christoph Deil to start this catalog in
August 2016 was to have a TeV catalog for http://gamma-sky.net ,
as well as for checks of all sources in the H.E.S.S. Galactic plane
survey catalog against previous publications.

## Terms of use

All data collected here was originally generated by others.
If you intend to use this data in a publication,
we ask that you please cite the linked sources and/or
contact the sources of the data directly.

Collecting, cleaning and maintaining this catalog has taken a lot
of time. We require that you give attribution if you're using it.

For now, while we don't have a website / paper yet that can be
cited, please use this attribution:

> Data taken from https://github.com/gammapy/gamma-cat,
> an open catalog of gamma-ray sources.

Otherwise, you are free to use this data as you like.

## Data

### Overview

This catalog contains data collected from the literature
(usually one or several papers per source).

In this repository, there's four major folders:

* `input` is where we collect data about sources given in papers.
  This is very heterogeneous and is the place for data entry only.
* `output` is produced with the `make_output.py` script in an
  automatic way from `input`. It's as-homogeneous and complete data
  as possible and what you should access for information lookup.
* `docs` is a webpage that displays part of the information in the
  repo (see link above). Specifically, there's a search field to find
  a given source and then from the source detail view there's links
  to the corresponding files in the `input` folder so that
  submitting corrections and additions is easy.
  This is intentionally very minimal, for a better experience to
  view the catalog online, go to http://gamma-sky.net/cat/tev 
  (not yet, coming soon).
* `seed` contains seed catalogs of TeV source info other people have
  collected previously. It was used to originally populate `input`
  using the `import_seed.py` script (see details below).

### Seed

As a starting point, data was ingested from these sources
(see the README files there for further information):

* https://github.com/gammapy/gamma-cat/tree/master/input/hess-galactic
* https://github.com/gammapy/gamma-cat/tree/master/input/tevcat
* https://github.com/gammapy/gamma-cat/tree/master/input/tgevcat

### Procedure

Information given in papers is very heterogeneous. Sometimes position
is given in Galactic coordinates, sometimes in RA/DEC coordinates.
There's different morphology and spectral models, sometimes certain
parameters haven't been measured or at least aren't given in the publication.

The idea is that we only collect the information from the papers here
(so that checking against the paper is easy and the manually
edited files are as small as possible), and then the `make_output.py`
script generates an as-homogeneous as possible version of the catalog.

Important points to decide:

* How do we structure the `input` folder?
  * Per-source: `source_id/papers.yaml`, `source_id/paper_id1.yaml`, `source_id/paper_id2.yaml`
  * Per-paper: `paper_id/sources.yaml`, `paper_id/source_id1.yaml`, `paper_id/source_id2.yaml`
* How do we structure the `output` folder?
  * Definitely should have per-source data, with data from multiple
    papers aggregated.
  * Should we use the `astrocats` method of tracking origin of all parameters?

#### Paper identifiers

For paper identifiers, we use the ADS identifiers.

  These are unique, well-known and stable.
  This corresponds to the `bibcode` in https://github.com/andycasey/ads/,
  i.e. can be used to obtain further info from ADS

  Note that some bibcodes contain characters that don't work well
  (or at all) as directory or filenames, e.g. `2011A&A...531L..18H`
  with the `&` character. 
  What ADS does in that case for URLs is to quote them, i.e.
  the URL is http://adsabs.harvard.edu/abs/2011A%26A...531L..18H . 
  In `gamma-cat` we do the same, we URL quote the bibcode for filenames,
  folder names and URLs, and otherwise use the normal bibcode.
  
  ```python
  >>> papers = list(ads.SearchQuery(bibcode='2011A&A...531L..18H'))
  >>> print(papers[0].bibcode)
  2011A&A...531L..18H

  >>> import urllib.parse
  >>> urllib.parse.quote('2011A&A...531L..18H')
  2011A%26A...531L..18H
  >>> urllib.parse.unquote('2011A%26A...531L..18H')
  2011A&A...531L..18H
  ```

#### Source identifiers

* TBD: How do we assign source identifiers (integer, TEV J...)?
* TBD: How do we handle sources that split out into multiple sources
  with deeper observations?

#### Source classes


* TBD: Define source classes and their semantics


## How it works

This is very much work in progress.
Feedback and contributions welcome!

* At the moment we're just using our own Python scripts,
  and YAML files (because I find them easier to read and edit than JSON),
  as well as ECSV files.
  Maybe we'll switch to use the https://astrocats.space/ machinery later.
* We use Python scripts. Only Python 3.5+ and the latest versions of
  Astropy, Gammapy, ... are supported (very few people will run the scripts,
  no point in supporting old versions here, even if it would be easy to do).

## References

* https://astrocats.space/
* https://gammapy.readthedocs.io/en/latest/catalog/index.html
* http://gamma-sky.net/
* https://github.com/gammapy/gamma-sky/issues/33
* https://github.com/gammapy/gamma-sky/issues/22

