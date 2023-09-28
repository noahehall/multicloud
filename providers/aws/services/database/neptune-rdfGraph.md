# Neptune RDF Graph

- create and query data using W3C's SPARQL

## links

### open source

- [w3c: vcard](https://www.w3.org/TR/vcard-rdf/)
- [w3c: foaf](http://xmlns.com/foaf/0.1/)
- [dublin core](https://www.dublincore.org/)
- [w3c: skos](https://www.w3.org/2004/02/skos/)
- [dbmedia](https://en.wikipedia.org/wiki/DBpedia)
- [geonames](https://en.wikipedia.org/wiki/GeoNames)

## best practicies

## basics

- encodes resource descriptions in the form of subject-predicate-object triples
- creates a more fine-grained representation of your domain relative to gremlin

## data model

## tests

## SPARQL

```ts
// find the names of users that p-1 follows
PREFIX s: <http://www.example.com/social#>

SELECT ?firstName ?lastName WHERE {
    s:p-1 s:follows ?p .
    ?p s:firstName ?firstName .
    ?p s:lastName ?lastName
}

```
