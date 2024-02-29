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
    orchid: 0000-0001-7542-0286
    affiliation: 3
  - name: Ammar Ammar
    orcid: 0000-0002-8399-8990
    affiliation: 3
  - name: Alexander Kellmann
    orchid: 0000-0001-6108-5552
    affiliation: 4
  - name: Ana Claudia Sima
    orchid: 0000-0003-3213-4495
    affiliation: 5

affiliations:
  - name: Micelio BV
    index: 1
  - name: Inst. f. Biochemistry, University Greifswald
    index: 2
  - name: Dept of Bioinformatics - BiGCaT, NUTRIM, FHML, Maastricht University
    index: 3
  - name: Genomics Coordination Center, University Medical Center Groningen 
    index: 4
  - name: SIB Swiss Institute of Bioinformatics 
    index: 5

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
 
 Involved: Jerven (code fixes), Egon testing, Marvin (WP SPARQL endpoint), Tooba ...


* things found
    * graph IRI wrong for `servicedescription` (Egon/Marvin)
        * discussed, will be updated
    * Virtuoso limit to 10k impacts output (Egon/Marvin)
    * statistics on GPML/WPRDF separation needed (or only WPRDF)
        * e.g. http://rdf.wikipathways.org/wprdf/ for the WPRDF, and  http://rdf.wikipathways.org/gpmlrdf/ for the GPMLRDF
        * also ontologies in separate graphs? 
            * Why? we migt be intereted in the usage of the ontologies
    * license? CCO for WikiPathways, but how to communicate the license for each dataset?
    * 

## ChEMBL VoID use case

Involved: Ammar 

Generating VoID file for the ChEMBL SPARQL endpoint.



## Setting up local virtuoso triple store

We installed virtuoso and loaded the drungbank database into the triple store. We already had prepared a .nq version of Drugbank, which had been created from previously [@citesAsRelated:Wishart2017].

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

ChatGPT-prompt:

```
We are using a SPARQL endpoint of drugbank database. 

Generate a SPARQL query to retrieve the DrugID of aspirin

Use "aspirin" as the label to find right URI
```


Respones
ChatGPT 3.5:

```sparql
PREFIX db: <http://www4.wiwiss.fu-berlin.de/drugbank/resource/drugbank/>

SELECT ?drugID
WHERE {
  ?drug db:commonName "aspirin" .
  ?drug db:drugbankId ?drugID .
}
```

Prefix was "invented", therefore we refined the query:





SPARQL (chatgpt 4):

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX db: <http://drugbank.ca#>

SELECT ?drugID
WHERE {
  ?drug rdfs:label "aspirin"@en .
  ?drug db:drugID ?drugID .
}
```

The prefix rdfs is correct, the prefix `http://drugbank.ca#` is invented, but fits to the website of drugbank. ChatGPT in version 4 tells us, that we need to specify the prefix, while ChatGPT verseion 3.5 did not warn about that.





ChatGPT-prompt:

```
We are using a local SPARQL endpoint of drugbank database. 
The prefix of the graph is should be: 
dbank <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5>
Generate a SPARQL query to retrieve the DrugID of aspirin
Use "aspirin" as the label to find right URI
```




## Validating a given SPARQL query against the VoID description of the target KG

The idea is to eliminate errors e.g. due to hallucinations in the SPARQL queries generated by cross-checking the URIs in the query against the available URIs in the VoID description.


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


https://github.com/vasugr/Natural-Language-To-SPARQL
https://github.com/sebferre/sparklis
https://github.com/machinalis/quepy
https://github.com/xiaoyuin/tntspa
https://chat.openai.com


