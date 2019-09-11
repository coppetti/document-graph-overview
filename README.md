# Document Graph
## Data Model
![](https://github.com/coppetti/document-graph-overview/blob/master/D313907F-D23D-4816-96C3-910F18F304BC.png)

### Nodes
Document Graph data model represents the following Nodes (entities):

1. Term: FCA Glossary Terms
2. Document: represents different Regulations, such as GDPR, MFiD II, etc;
3. Node: Sections and paragraphs inside Documents;
4. Industry: currently mapping only Finance;
5. Handbook: FCA Handbooks;

#### Ontology Nodes:
The ontology maps data from Legislation.Gov document only.

1. MainType: Type of the document, such as `NorthernIrelandOrderInCouncil`, `UnitedKingdomStatutoryInstrument`, etc;
2. Publisher: Document publisher, such as: `Statute Law Database`, `Queen's Printer for Scotland`, etc.;
3. Subject: `PUBLIC SERVICE PENSIONS`, `SOCIAL SECURITY`, etc.
4. Status: identifies if the document is `revised` or not;
5. Category: if `secondary` or not;
6. Number: number of the Document.

### Relationships
The model also represents several relationships across these nodes, such as:
1. HAS_REFERENCE: nodes have references to: other nodes, handbooks and documents.  Also stores information like: paragraph number, start and end of the text, chapter, subparagraph, direction of the reference (IN our OUT), ...
2. HAS_MENTION_IN: this relationship links terms and nodes, based on node text content and term name.
3. HAS_SUCCESSOR and HAS_PREDECESSOR: order paragraphs/subparagraphs and articles

#### Ontology Relationships
Those relationships describe the Document Metadata

### Accessing the data

On browser at:
`35.176.13.253:7474/browser/`

### Running some Queries

+ References to a Document:

```
MATCH (d:Document)<-[:HAS_REFERENCE]-(x)
WHERE d.document_name = '(EU) 2016/679'
RETURN d,x LIMIT 25
```

+ Find All documents that concern to `payment service user` and `payment service provider`and the documents that have any reference (in/out) to them:

```
MATCH (t:Term)--(n:Node)-[r:HAS_REFERENCE]->(x)
WHERE toLower(t.term) CONTAINS 'payment service user' OR toLower(t.term) CONTAINS 'payment service provider'

return * LIMIT 100
```
