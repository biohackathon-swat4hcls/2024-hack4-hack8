---
title: 'SWAT4HCLS Biohackathon 2024 report: Beyond the Void & AI Prompt engineering for SPARQL'
title_short: 'SWAT4HCLS 2024 Hackathon'
tags:
  - SPARQL
  - ChatGPT
  - AI
  - WikiPathways
  - FAIR Data Point
authors:
  - name: Andra Waagmeester
    orcid: 0000-0000-0000-0000
    affiliation: 1
  - name: Mark Doerr
    orcid: 0000-0003-3270-6895
    affiliation: 2
  - name: Tooba Abbassi-Daloii
    orcid: 0000-0002-4904-3269
    affiliation: 3
  - name: Egon Willighagen
    orcid: 0000-0001-7542-0286
    affiliation: 3
  - name: Ammar Ammar
    orcid: 0000-0002-8399-8990
    affiliation: 3
  - name: Alexander Kellmann
    orcid: 0000-0001-6108-5552
    affiliation: 4
  - name: Ana Claudia Sima
    orcid: 0000-0003-3213-4495
    affiliation: 5
  - name: Yasunori Yamamoto
    orcid: 0000-0002-6943-6887
    affiliation: 6
  - name: Tarcisio Mendes de Farias
    orcid: 0000-0002-3175-5372
    affiliation: 7
  - name: Julio C. Rangel
    orcid: 0000-0001-8132-6341
    affiliation: 8
  - name: Jerven Bolleman
    affiliation: 5
    orcid: 0000-0002-7449-1266
  - name: Jakub Galgonek
    orcid: 0000-0002-7038-544X
    affiliation: 9
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
  - name: Database Center for Life Science
    index: 6
  - name: FIXME
    index: 7
  - name: Data Knowledge Organization Unit, RIKEN 
    index: 8
  - name: Institute of Organic Chemistry and Biochemistry of the Czech Academy of Sciences
    index: 9
date: 29 February 2024
cito-bibliography: paper.bib

event: SWAT4HCLS24
biohackathon_name: "BioHackathon SWAT4HCLS 2024"
biohackathon_url:   "https://www.swat4ls.org/workshops/leiden2024/hackathon/"
biohackathon_location: "Leiden, The Netherlands, 2024"
group: Project 4-8
git_url: https://github.com/biohackathon-swat4hcls/2024-hack4-hack8
authors_short: Andra Waagmeester \emph{et al.}
---

# Introduction

As part of the BioHackathon SWAT4HCLS 2024, we here report on the topic AI prompt engineering for SPARQL. During the introductions we noticed that there is a possible overlap between this project and the other project `Beyond the VoID`. 

## WikiPathways 

WikiPathways is a database of biological pathways and an ELIXIR-NL service [@Martens2021; @Slenter2018]. The project was involved
in two hackathon projects as explained in this section.

### The VoID statistics use case

Involved: Jerven (code fixes), Egon (testing), Tooba (testing), Marvin (WP SPARQL endpoint)

In this session we explored the VoID header file of WikiPathways [@Martens2021] for enrichment with statistics generated with the
Jerven Bolleman's VoID statistics tool (XXXXX). The tool did not work out of the box and several code updates were written.
Furthermore, we noted issues with the WikiPathways SPARQL endpoint. For example, we observed that the graph IRI for the
`servicedescription` was wrong and needed to be updated. We also ran into the default Virtuoso limit of returning 10 thousand
rows, affecting the calculation of the statistics. We also realized that statistics are more informative if they would
be calculate for the WPRDF and GPMLRDF separately. For this, they will need to be loaded into separate graphs in the Virtuoso
triple sotre, for example with the graph IRIs `http://rdf.wikipathways.org/wprdf/` and `http://rdf.wikipathways.org/gpmlrdf/`.

### WikiPathways FDP use case

Involved: Egon (development), Eric (discussion)

The WikiPathways FAIR Data Point (FDP) was originally developed in 2017 [@citesAsEvidence:Slenter2018]. During (and around) the hackathon
we updated the GitHub-hosted FDP. Last year this solution was setup as replacement of the original FDP which was based on dedicated
software. However, the replacement needed further testing with the EJP-RD setup. Based on a script developed in 2022 by Rajaram Kaliyaperumal
[@usesMethodIn:Kaliyaperumal2022], the GitHub repository pings the EJP-RD index of FAIR Data Points ([index.vp.ejprarediseases.org/](https://index.vp.ejprarediseases.org/)).
The repository has a script to automate the update the FDP for new WikiPathways releases and it now includes information
on the first two distributions. These are the archives for WPRDF and GPMLRDF as they can be found on the
[data.wikipathways.org](https://data.wikipathways.org) download website.

However, the solution had two shortcomings. First, the mime type returns as `Content-type` was not properly set, and second, we wanted
to further explore enriching the FDP for use in EJP-RD. We updated the setup of the repository to have GitHub host the `TTL` files
with the proper mime type. This required renaming the files from `index.html` to `index.tll`. The content of the files was adjusted
accordingly to ensure the linked data nature was not broken. That is, the IRIs used by the various resources now end with `index.ttl`.
We note that this leaks some implementation detail.

For the second, we explored the use of the VoID statistics work in Project, as reported elsewhere in this report. We envision
integrating the results of such an statistics analysis into the VoID header file for the RDF data and possible integrate some
statistics into the FAIR data point too. Related to this, we observed an interest in adding a Service Description and the need
to communicate more details about the content of the datasets, such as which diseases are covered by WikiPathways. This
information can be extraced from the RDF, but it is yet unclear how these annotations need to be added to the FDP Turtle.

## VoID support in Comunica

The following shows the performance results of a prototypical implementation of VoID-based cardinality estimation
Cardinality estimation using VoID is done based on the heuristics of "Hagedorn, Stefan, et al. "Resource Planning for SPARQL Query Execution on Data Sharing Platforms." COLD 1264 (2014)".

For endpoints not exposing VoID metadata, Comunica falls back to sending COUNT queries.

#### Query 15

Sources:

- https://lindas.admin.ch/query (without void)
- https://sparql.rhea-db.org/sparql (with void)

Query:
```sparql
PREFIX schema: <http://schema.org/>
SELECT * WHERE {

?substance a schema:DefinedTerm ;
    schema:identifier ?identifier ;
    schema:inDefinedTermSet <https://ld.admin.ch/cube/dimension/el01> .

?compound <http://rdf.rhea-db.org/formula> ?identifier ;
    <http://rdf.rhea-db.org/name> ?name ;
    <http://rdf.rhea-db.org/accession> ?accession .
}
```

With void:

|Result|Delay (ms)|HTTP requests|
| -------- | -------- | -------- |
|1|162.69260597229004,14|
|2|162.85465812683105|14|
|3|212.3646821975708|17|
|4|378.3809337615967|20|
|5|378.5601854324341|20|
|6|420.2927780151367|23|
|7|465.06996726989746|26|
|8|465.21367835998535|26|
|9|507.01233673095703|29|
|10|599.0587310791016|32|
|11|637.3339443206787|35|
|TOTAL|639.911449432373|35|

Without void:

|Result|Delay (ms)|HTTP requests|
|---|---|---|
|1|159.31932067871094|20|
|2|159.54065895080566|20|
|3|600.7964477539062|23|
|4|647.9744453430176|26|
|5|648.5275049209595|26|
|6|807.2107524871826|29|
|7|850.3266448974609|32|
|8|851.1076250076294|32|
|9|1003.3682670593262|35|
|10|1041.3201503753662|38|
|11|1076.443021774292|41|
|TOTAL|1079.3609952926636|41|

#### Query uniprot40

Network or endpoint too unstable (requests are dropped)

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rh: <http://rdf.rhea-db.org/>
PREFIX up: <http://purl.uniprot.org/core/>
PREFIX CHEBI: <http://purl.obolibrary.org/obo/CHEBI_>
PREFIX taxon: <http://purl.uniprot.org/taxonomy/>

SELECT DISTINCT
  ?protein
  ?chemblEntry
WHERE {
  SERVICE <https://sparql.rhea-db.org/sparql> {
    ?rhea rdfs:subClassOf rh:Reaction ;
      rh:side/rh:contains/rh:compound/rh:chebi/rdfs:subClassOf+ CHEBI:26739 .
  }
  ?ca up:catalyzedReaction ?rhea .
  ?protein up:annotation/up:catalyticActivity ?ca ;
    up:organism taxon:9606 ;
    rdfs:seeAlso ?chemblEntry .
  ?chemblEntry up:database <http://purl.uniprot.org/database/ChEMBL> .
}
```

#### Query uniprot38

Fails due to an endpoint producing a syntax error in SPARQL results at some point during execution.

```sparql
PREFIX uniprotkb: <http://purl.uniprot.org/uniprot/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX pq: <http://www.wikidata.org/prop/qualifier/>
PREFIX p: <http://www.wikidata.org/prop/>
PREFIX ps: <http://www.wikidata.org/prop/statement/>

SELECT
	?protein
	?begin
	?end
	?chromosome
	?assembly
WHERE {
    {
        BIND(uniprotkb:P05067 AS ?proteinIRI)
        BIND (SUBSTR(STR(?proteinIRI), STRLEN(STR(uniprotkb:))+1) AS ?protein)
    }
    #SERVICE <https://query.wikidata.org/sparql> {
        ?wp wdt:P352 ?protein ;
            wdt:P702 ?wg .
        ?wg p:P644   ?wgss .
        ?wgss ps:P644        ?begin ;
          pq:P1057/wdt:P1813 ?chromosome ;
          pq:P659/rdfs:label ?assembly .
        ?wg p:P645 ?wgse .
        ?wgse ps:P645        ?end ;
          pq:P1057/wdt:P1813 ?chromosome ;
          pq:P659/rdfs:label ?assembly .
        FILTER(lang(?assembly) = "en")
  #}
}
```

## VoID auto-generator
Involved: Yasunori, Tarcisio

We started the evaluation of two VoID metadata generators, namely, void-generator and Triple Data Profiler. First, we evaluate if the generated metadata are complementary or the same. We noticed that they are indeed complementary to each other. Secondly, we aimed at combining their results to produce a more complete VoID-based description of a given RDF data source. We found out that merging both tools' VoID results is not straightforward. We identified that the generated output by the tools are structured in different ways even though both rely on VoID.

One difference, now fixed, is that the Triple Data Profiler uses `void:entities` to count the number of classes in a `void:classPartition` while void-generator used `void:triples`. Void-generator was addapted to match the Triple Data Profiler.

## ChEMBL use case for the VoID auto-generator 

Involved: Ammar 

ChEMBL [@cites:10.1093/nar/gkr777], a prominent bioactivity database, plays a crucial role in drug discovery and chemical biology research. It houses vast amounts of data relating to the interactions between small molecules and their biological targets. Leveraging Semantic Web technologies, ChEMBL provides a SPARQL endpoint, enabling users to query its data using the RDF data model [@usesDataFrom:10.1093/bioinformatics/btt765].

On the other hand, VoID (Vocabulary of Interlinked Datasets) [@cites:Zhao/11/DLD] serves as a metadata standard for describing RDF datasets. It facilitates the discovery and integration of linked data by providing essential information about datasets, such as their structure, statistics, and relationships with other datasets. By generating a VoID file for the ChEMBL SPARQL endpoint, users gain insights into the dataset's characteristics, enabling better utilization and integration with other linked datasets.

The ChEMBL Core Ontology (CCO) encapsulates the essential concepts and relationships within the ChEMBL dataset in an RDF-compatible format. It defines classes and properties to represent entities such as compounds, targets, assays, and activities, fostering interoperability and standardization in bioactivity data representation. By querying the ChEMBL SPARQL endpoint for predicates within the CCO, researchers can uncover significant patterns and relationships present in the dataset, aiding in various analyses and investigations, such as target identification, compound profiling, and drug repurposing efforts.

During the [SWAT4HCLS](https://swat4hcls.org) BioHackathon 2024, a task was carried out aiming at creating a VoID file for the ChEMBL SPARQL endpoint. That was facilitated through the utilization of the [void-generator](https://github.com/JervenBolleman/void-generator) tool. The outcomes of this process have been shared within this [GitHub Repository](https://github.com/ammar257ammar/SWAT4HCLS-BioHackathon-void). Following this, the generated [void.ttl](https://github.com/ammar257ammar/SWAT4HCLS-BioHackathon-void/blob/main/void.ttl) file along with the [cco.ttl](https://ftp.ebi.ac.uk/pub/databases/chembl/ChEMBL-RDF/latest/cco.ttl.gz) ontology file were imported into a triple store (GraphDB), and thorough exploration was performed via SPARQL. To delve deeper into the usage of the ChEMBL Core Ontology (CCO) properties, a specific SPARQL query (shown below) was formulated to extract the top 20 predicates based on their frequency of occurrence within the dataset, alongside the enumeration of distinct subjects and objects associated with them.

```sparql
PREFIX void: <http://rdfs.org/ns/void#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?propertyDescription ?triples ?distinctSubjects ?distinctObject
WHERE { 

  ?s void:property ?property.
  ?property rdfs:label ?propertyLabel.
    
  ?s void:triples ?triples .
  ?s void:distinctObjects ?distinctObject.
  ?s void:distinctSubjects ?distinctSubjects.
    
  BIND(CONCAT(
         REPLACE(
           STR(?property),
           "http://rdf.ebi.ac.uk/terms/chembl#", 
           "chembl:"), 
          " (", 
          ?propertyLabel, 
          ")"
  ) AS ?propertyDescription) 
} ORDER BY DESC(?triples)
LIMIT 20
```



Below is the resulting table of the top 20 predicates based on their frequency of occurrence within the ChEMBL RDF dataset.



| Property Description                                         | # of triples | # of Distinct Subjects | # of Distinct Object |
| ------------------------------------------------------------ | ------------ | ---------------------- | -------------------- |
| chembl:hasActivity (ChEMBL Activity Association)             | 61004052     | 3999701                | 20334684             |
| chembl:hasAssay (ChEMBL Assay Association)                   | 25166472     | 20438130               | 1610596              |
| chembl:hasDocument (ChEMBL Document Association)             | 25023633     | 24257792               | 88630                |
| chembl:chemblId (The ChEMBL ID for a given entity)           | 24527418     | 24527418               | 24527418             |
| chembl:hasMolecule (ChEMBL Molecule Association)             | 23384062     | 20482807               | 2312287              |
| chembl:standardType (ChEMBL Activity Entity Standard Type)   | 20334684     | 20334684               | 6379                 |
| chembl:type (ChEMBL Activity Entity Type)                    | 20334684     | 20334684               | 6630                 |
| chembl:standardValue (ChEMBL Activity Entity Standard Value) | 18818643     | 18818643               | 306321               |
| chembl:value (ChEMBL Activity Entity Value)                  | 18818643     | 18818643               | 1325941              |
| chembl:standardUnits (ChEMBL Activity Entity Standard Units) | 18077058     | 18077058               | 2567                 |
| chembl:hasUnitOnto (Unit Ontology Link)                      | 17622648     | 17622648               | 43                   |
| chembl:hasQUDT (QUDT Ontology Link)                          | 17468137     | 17468137               | 25                   |
| chembl:units (ChEMBL Activity Entity Units)                  | 15078494     | 15078494               | 2794                 |
| chembl:relation (ChEMBL Activity Entity Relation)            | 13040294     | 13040294               | 24                   |
| chembl:standardRelation (ChEMBL Activity Entity Standard Relation) | 13037254     | 13037254               | 8                    |
| chembl:activityComment (ChEMBL Activity Entity Comment)      | 10135818     | 10135818               | 209889               |
| chembl:moleculeXref (ChEMBL Molecule Cross Reference Property) | 6447112      | 2234469                | 6447073              |
| chembl:pChembl (ChEMBL Activity pChembl Value)               | 4037920      | 4037920                | 1087                 |
| chembl:taxonomy (ChEMBL Entity Taxonomy)                     | 2937114      | 1468557                | 8254                 |
| chembl:substanceType (ChEMBL Substance Entity Type)          | 2399743      | 2399743                | 9                    |




## VoID testing on my local GraphDB

[ENPKG](https://enpkg.commons-lab.org)+ more stuff

Many queries with are MALFORMED QUERY


GraphDB freezed after 66444 queries. Last VoID messages:

```
processing request to {}->http://localhost:7200: The target server failed to respond
[pool-1-thread-3] INFO org.apache.http.impl.execchain.RetryExec - Retrying request to {}->http://localhost:7200
[pool-1-thread-2] WARN org.eclipse.rdf4j.http.client.SPARQLProtocolSession - Server reports problem: null (enable debug logging for full details)
[pool-1-thread-3] WARN org.eclipse.rdf4j.http.client.SPARQLProtocolSession - Server reports problem: null (enable debug logging for full details)
[pool-1-thread-2] WARN org.eclipse.rdf4j.http.client.SPARQLProtocolSession - Server reports problem: null (enable debug logging for full details)
[pool-1-thread-1] WARN org.eclipse.rdf4j.http.client.SharedHttpClientSessionManager$RetryHandlerStale - Closing stale connection
[pool-1-thread-1] INFO org.apache.http.impl.execchain.RetryExec - I/O exception (org.apache.http.NoHttpResponseException) caught when processing request to {}->http://localhost:7200: The target server failed to respond
[pool-1-thread-1] INFO org.apache.http.impl.execchain.RetryExec - Retrying request to {}->http://localhost:7200
[pool-1-thread-2] WARN org.eclipse.rdf4j.http.client.SPARQLProtocolSession - Server reports problem: null (enable debug logging for full details)
[pool-1-thread-3] WARN org.eclipse.rdf4j.http.client.SPARQLProtocolSession - Server reports problem: null (enable debug logging for full details)
```

and in the GraphDB main log:

```
[ERROR] 2024-02-29 16:15:31,116 [http-nio-7200-exec-8 | o.a.c.c.C.[.[.[.[openrdf-http-server]] Servlet.service() for servlet [openrdf-http-server] in context with path [] threw exception [Handler processing failed; nested exception is java.lang.OutOfMemoryError: Java heap space] with root cause
java.lang.OutOfMemoryError: Java heap space
```

I am ready to try it again after the next round of bugs fix :-))


## VoID testing on locally installed subsets of IDSM

* Testing was performed on the [ISDB](https://zenodo.org/records/8287341) and [MoNA](https://mona.fiehnlab.ucdavis.edu/) datasets, in their RDF forms as contained in the [IDSM](https://idsm.elixir-czech.cz/).
* Several issues were fixed and several optimizations were made on the VoID generator side.
* We have identified queries that will need to be better optimized on the IDSM SPARQL engine side.

### Testing on the ISDB subset

| measurement            |     value |
|------------------------|-----------|
| ISDB triplets          |  36783950 |
| ISDB classes           |        18 |
| ISDB predicates        |        19 |
| VoID execution time    |      3min |
| executed queries       |      1786 |

### Testing on the MoNA subset

| measurement            |     value |
|------------------------|-----------|
| MoNA triplets          | 282519684 |
| MoNA classes           |        48 |
| MoNA predicates        |        34 |
| VoID execution time    |     23min |
| executed queries       |     10408 |

## Validating a given SPARQL query against the VoID description of the target KG

The idea is to eliminate errors e.g. due to hallucinations in the generated SPARQL queries, by cross-checking the URIs in the query against the available URIs in the VoID description.



# AI Prompt engineering for SPARQL

## Setting up local Virtuoso triple store

We installed Virtuoso and loaded the drungbank database into the triple store. We already had prepared a .nq version of Drugbank, which had been created from an XML file downloaded on 4th of January 2023 from Drugbank (for academic use only) [@citesAsRelated:Wishart2017].
That file had been translated into nquads using scripts from Bio2RDF: `php runparser.php parser=drugbank download=false files=all indir=<path1> outdir=<path1> registry_dir=<path3>` [@citesAsSourceDocument:bio2rdf-scripts].

With the following steps:

Running the docker

- configuring the virtuoso.ini:
   ```DirsAllowed              = ., ../vad, /usr/share/proj, /data```

- copy the drugbank.nq to local /data volume

### Starting the Virtuoso client:

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

We chose Aspirin as a sample drug and ask for the "DrugID"


#### A remark to the reproducibility of the LLM queries

Prompting LLM usually send also the context in each query.
The AI improves its answers based on the context.
To reproduce a certain result one would require to reproduce the whole history of prompts of a session. 
We descided to **first find prompts leading to reasonable results**
and then reset the session and generate an optimised series of prompts to come to at least somewhat reproducable results, starting from scratch.


### Retrieving some basic information
#### The easy way: 

ChatGPT-prompt: What is the DrugbankID of Aspirin?

ChatGPT 3.5 reply:

```
The DrugBank ID for Aspirin is DB00945.
```


ChatGPT 4.0 reply:

```
The DrugBank ID of Aspirin is DB00945 (citation: https://www.drugs.com/ingredient/aspirin.html).
```

In this case the result was correct in both cases, no halucination and in GPT4 even a source for the valid result.

#### Requesting the URI of Aspirin via SPARQL:     

ChatGPT-prompt:

```
We are using a SPARQL endpoint of drugbank database. 

Generate a SPARQL query to retrieve the DrugID of aspirin

Use "aspirin" as the label to find right URI
```


Expectation:
We expect a releatively basic SPARQL query in the form of 

SELECT ?drug
WHERE {
  ?drug rdfs:label "Aspirin".
}


Responses
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

The prefix `rdfs` is correct, the prefix `http://drugbank.ca#` is invented, but fits to the website of drugbank. ChatGPT in version 4 tells us, that we need to specify the prefix, while ChatGPT version 3.5 did not warn about that.



#### Adding more details to the prompt

ChatGPT-prompt:

```
We are using a local SPARQL endpoint of drugbank database. 
The prefix of the graph is should be: 
dbank: http://bio2rdf.org/drugbank_resource
It is stored in a graph called <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5>
To explore the structure of the database, a good start would be to look for a known drug like aspirin.
Generate a SPARQL query to retrieve the URI of Aspirin.
Use "aspirin" as the label to find right URI.
Do not assume that it has the rdf:label predicate but an english language tag. 
```

Expectation:

```
PREFIX dbank: <http://bio2rdf.org/drugbank_resource:>
SELECT DISTINCT ?drugURI
WHERE {
  GRAPH <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5> {
    ?drugURI ?predicate "Aspirin"@en.
  }
}
```

ChatGPT 3.5 reply:

```
PREFIX dbank: <http://bio2rdf.org/drugbank_resource>

SELECT ?drug_uri
WHERE {
  GRAPH <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5> {
    ?drug_uri ?p ?o .
    FILTER (str(?o) = "aspirin"@en)
  }
}
```

The result is almost correct: only the capitalisation and the language tag was wrong.

How it should be:
    
```
PREFIX dbank: <http://bio2rdf.org/drugbank_resource>

SELECT ?drug_uri
WHERE {
  GRAPH <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5> {
    ?drug_uri ?p ?o .
    FILTER (str(?o) = "Aspirin")
  }
}
```

Running the corrected query, resulted in two hits :)


ChatGPT 4.0 reply:

Given the requirements and the prefix for the DrugBank database, a SPARQL query to find the URI of Aspirin, where "aspirin" is the label with an English language tag, would look like this:

PREFIX dbank: <http://bio2rdf.org/drugbank_resource>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?aspirinURI
WHERE {
  GRAPH <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5> {
    ?aspirinURI rdfs:label "aspirin"@en.
  }
}
    
In this query:

The PREFIX lines define short names for the URIs that will be used in the query. dbank is set as the prefix for DrugBank resources, and rdfs is the prefix for the RDF Schema vocabulary, which includes the label property.
The SELECT line specifies that we want to retrieve the URI of Aspirin, which we're calling ?aspirinURI in the query.
The WHERE clause specifies the pattern to match in the data. We're looking for a resource (?aspirinURI) within the specified graph (<http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5>) that has an English language label (rdfs:label "aspirin"@en).
This query assumes that the label for Aspirin is exactly "aspirin" and tagged with @en for English. Depending on the specifics of the data, you might need to adjust the query if, for example, the label includes additional information or uses a slightly different form (e.g., capitalization).

The query gave no results due to the spelling "aspirin"@en instead of "Aspirin"@en. That has been described as one of the potential weaknesses.
    

#### What we actually found out:

```
PREFIX dbank: <http://bio2rdf.org/drugbank_resource:>
SELECT DISTINCT ?drugURI ?predicate ?s
WHERE {
  GRAPH <http://bio2rdf.org/drugbank_resource:bio2rdf.dataset.drugbank.R5> {
    ?drugURI ?predicate "Aspirin"@en.
    ?s ?p ?drugURI.
  }
}
```

The DrugID of Aspirin is actually not the same URI that is connected to the label "Aspirin"@en directly, but indirectly via "http://bio2rdf.org/drugbank_vocabulary:synonym" so we actually need 2 hops to find the DrugID from the URI.
This is unexpected and not solvable without presenting ChatGPT the graph structure using prior knowledge or additional information.

## Experiment with open source model
We conducted another experiment to discover the Aspiring URI, this time leveraging the OpenLLama open-source data model.
Prompt:
```
INSTRUCTION
Please convert the following context into an SPARQL query.

CONTEXT:
Retrieve the URI of Aspirin

SPARQL:
```
```
Result:
SELECT DISTINCT ?pv WHERE { ?e pred:name "Aspirin" . ?e ?pv }
```

It appears that instruct-tuning is necessary to ensure the correct prefix is applied.

​    

# Future Work

## Refining the query with by generating the Drugbank graph Schema

We are investigating the Drugbank represented as Linked Data. Therefore, we are interested in building the ontology/schema to see the structure of the data. Therefore we used the following SPARQL query that we borrowed from [Bio2RDF_query_Dumontier](https://www.w3.org/TR/hcls-dataset/#s6_6) :

```
SELECT (COUNT(DISTINCT ?s) AS ?scount) ?stype ?p ?otype
  (COUNT(DISTINCT ?o) AS ?ocount)  
{ 
 ?s ?p ?o . 
 ?s a ?stype .
 ?o a ?otype .
} GROUP BY ?p ?stype ?otype
```

This query shows the structure of different types, but lacks of predicates and literals.
    
## Further queries to test

Augment the catalog of queries on DisGenet, using more complex queries.

...

## Acknowledgements

Michel Dumontier, Alberto Labarga

## Further reading

- https://github.com/vasugr/Natural-Language-To-SPARQL
- https://github.com/sebferre/sparklis
- https://github.com/machinalis/quepy
- https://github.com/xiaoyuin/tntspa
- https://chat.openai.com
- https://github.com/micheldumontier/sparql-langchain/

## References


