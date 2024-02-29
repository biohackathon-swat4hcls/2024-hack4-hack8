---
title: 'SWAT4HCLS Biohackathon 2024 report: Beyond the Void & AI Prompt engineering for SPARQL'
title_short: 'SWAT4HCLS 2024 Hackathon'
tags:
  - SPARQL
  - ChatGPT
  - AI
  - WikiPathways
authors:
  - name: Andra Waagmeester
    orcid: 0000-0000-0000-0000
    affiliation: 1
  - name: Mark Doerr
    orcid: 0000-0003-3270-6895
    affiliation: 2
  - name: Tooba Abbassi-Daloii
    orchid: 0000-0002-4904-3269
    affiliation: 3
  - name: Egon Willighagen
    orchid: 0000-0002-
    affiliation: 3
  - name: Alexander Kellmann
    orchid: 0000-0001-6108-5552
    affiliation: 4

affiliations:
  - name: Micelio BV
    index: 1
  - name: Inst. f. Biochemistry, University Greifswald
    index: 2
  - name: Dept of Bioinformatics - BiGCaT, NUTRIM, FHML, Maastricht University
    index: 3
  - name: Genomics Coordination Center, University Medical Center Groningen 
    index: 4
date: 29 February 2024
cito-bibliography: paper.bib
event: SWAT4HCLS24
biohackathon_name: "BioHackathon SWAT4HCLS 2024"
biohackathon_url:   "https://www.swat4ls.org/workshops/leiden2024/hackathon/"
biohackathon_location: "Leiden, the Netherlands, 2024"
group: Project 4-8
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackrxiv/publication-template
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Andra Waagmeester \emph{et al.}
---


# Introduction

As part of the BioHackathon SWAT4LSHC 2024, we here report on the biohackathon SWAT4HCLS topic AI prompt engineering for SPARQL. During the introductions we noticed that there is a possible overlap between this project and the other project Beyond the VoID. 

## Overview of Related work / projects

## WikiPathways VoID use case

<!-- discuss with Marvin -->
 
 Involved: Jerven (code fixes), Egon testing, Marvin (WP SPARQL endpoint), ...


* things found
    * graph IRI wrong for `servicedescription` (Egon/Marvin)
        * discussed, will be updated
    * Virtuoso limit to 10k impacts output (Egon/Marvin)
    * statistics on GPML/WPRDF separation needed (or only WPRDF)
        * e.g. http://rdf.wikipathways.org/wprdf/ for the WPRDF, and  http://rdf.wikipathways.org/gpmlrdf/ for the GPMLRDF
        * also ontologies in separate graphs?
    * license? CCO, but do we want that on each dataset
* 




## Setting up local virtuoso triple store

We installed virtuoso and loaded the drungbank database into the triple store. We already had prepared a .nq version of Drugbank, which had been created from previously. [@citesAsRelated:Wishart2017]

With the following steps:

Running the docker

- configuring the virtuoso.ini:
   ```DirsAllowed              = ., ../vad, /usr/share/proj, /data``

- copy the drugbank.nq to local /data volume

### starting the virtuoso cli client:

We used the isql console to load some data via the bulk loader:
  ```isql 1111 dba dba```


The ld_dir command is used to select which data to load (in this case /data/drugbank.nq).
 ```ld_dir ('/data', '*.nq', 'http://localhost:8890/drugbank') ;```

To bulk load the data into the Virtuoso server:
  ```rdf_loader_run();```

To prevent a rollback when restarting the container:
  ```checkpoint;```

The drungbank graph is now locally available through the browser at localhost:8890


## Sample Queries

### Getting simple drug information
 
We chose Aspirin as a sample drug and ask for the "drugID"

English: What is the drugID of aspirin

SPARQL (chatgpt 4):



## Formatting

This document use Markdown and you can look at [this tutorial](https://www.markdowntutorial.com/).

## Subsection level 2

Please keep sections to a maximum of only two levels.

## Tables and figures

Tables can be added in the following way, though alternatives are possible:

Table: Note that table caption is automatically numbered and should be
given before the table itself.

| Header 1 | Header 2 |
| -------- | -------- |
| item 1 | item 2 |
| item 3 | item 4 |

A figure is added with:

![Caption for BioHackrXiv logo figure](./biohackrxiv.png)

# Other main section on your manuscript level 1

Lists can be added with:

1. Item 1
2. Item 2

# Citation Typing Ontology annotation

You can use [CiTO](http://purl.org/spar/cito/2018-02-12) annotations, as explained in [this BioHackathon Europe 2021 write up](https://raw.githubusercontent.com/biohackrxiv/bhxiv-metadata/main/doc/elixir_biohackathon2021/paper.md) and [this CiTO Pilot](https://www.biomedcentral.com/collections/cito).
Using this template, you can cite an article and indicate _why_ you cite that article, for instance DisGeNET-RDF [@citesAsAuthority:Queralt2016].

The syntax in Markdown is as follows: a single intention annotation looks like
`[@usesMethodIn:Krewinkel2017]`; two or more intentions are separated
with colons, like `[@extends:discusses:Nielsen2017Scholia]`. When you cite two
different articles, you use this syntax: `[@citesAsDataSource:Ammar2022ETL; @citesAsDataSource:Arend2022BioHackEU22]`.

Possible CiTO typing annotation include:

* citesAsDataSource: when you point the reader to a source of data which may explain a claim
* usesDataFrom: when you reuse somehow (and elaborate on) the data in the cited entity
* usesMethodIn
* citesAsAuthority
* citesAsEvidence
* citesAsPotentialSolution
* citesAsRecommendedReading
* citesAsRelated
* citesAsSourceDocument
* citesForInformation
* confirms
* documents
* providesDataFor
* obtainsSupportFrom
* discusses
* extends
* agreesWith
* disagreesWith
* updates
* citation: generic citation


# Results


# Discussion

...

## Acknowledgements

...

## References

<!-- the blow should be inline OR in the BiBTeX -->

@article{Wishart2017,
  author = {Wishart, D. S. and Feunang, Y. D. and Guo, A. C. and Lo, E. J. and Marcu, A. and Grant, J. R. and Sajed, T. and Johnson, D. and Li, C. and Sayeeda, Z. and Assempour, N. and Iynkkaran, I. and Liu, Y. and Maciejewski, A. and Gale, N. and Wilson, A. and Chin, L. and Cummings, R. and Le, D. and Pon, A. and Knox, C. and Wilson, M.},
  title = {DrugBank 5.0: a major update to the DrugBank database for 2018},
  journal = {Nucleic Acids Res},
  year = {2017},
  volume = {Nov 8},
  doi = {10.1093/nar/gkx1037}
}


https://github.com/vasugr/Natural-Language-To-SPARQL
https://github.com/sebferre/sparklis
https://github.com/machinalis/quepy
https://github.com/xiaoyuin/tntspa
https://chat.openai.com


