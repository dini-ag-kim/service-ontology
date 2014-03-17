# Introduction

The **Service Ontology** is a micro-ontology that defines the general concept
of a service. The ontology is related to [GoodRelations Vocabulary], to
[Schema.org Vocabulary], to [DCMI Metadata Terms], and to [FOAF Vocabulary].

## Status of this document

This document and RDF serializations of the Service Ontology
([**`service.ttl`**](service.ttl) in RDF/Turtle and 
[**`service.owl`**](service.owl) in RDF/XML) are based on a source
file written in Pandoc Markdown syntax, managed with 
[makespec](http://jakobib.github.io/makespec/). Sources and updates are
available at <http://github.com/dini-ag-kim/service-ontology>.

This is version {VERSION}, last modified at {GIT_REVISION_DATE} with revision
{GIT_REVISION_HASH}.

The current version of this ontology is a draft for open discussion.
[Feedback](https://github.com/dini-ag-kim/service-ontology/issues) is welcome!

**Revision history**

{GIT_CHANGES}

## Namespaces and ontology

The URI namespace of the Service Ontology is
[http://purl.org/ontology/service#](http://purl.org/ontology/service#). The
namespace prefix `service` is recommended. The URI of this ontology as a whole
is <http://purl.org/ontology/service>.

The following namespace prefixes are used to refer to related ontologies:

    @prefix gr:      <http://purl.org/goodrelations/v1#> .
    @prefix schema:  <http://schema.org/> .
    @prefix owl:     <http://www.w3.org/2002/07/owl#> .
    @prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix dcterms: <http://purl.org/dc/terms/> .
    @prefix dctype:  <http://purl.org/dc/dcmitype/> .
    @prefix vann:    <http://purl.org/vocab/vann/> .
    @prefix voaf:    <http://purl.org/vocommons/voaf#> .
    @prefix foaf:    <http://xmlns.com/foaf/0.1/> .
    @prefix vs:      <http://www.w3.org/2003/06/sw-vocab-status/ns#> .
    @prefix xsd:     <http://www.w3.org/2001/XMLSchema#> .
    @prefix cc:      <http://creativecommons.org/ns#> .

The Service Ontology is defined in RDF/Turtle as following:

    @prefix service: <http://purl.org/ontology/service#> .
    @base            <http://purl.org/ontology/service> .

    <> a owl:Ontology, voaf:Vocabulary ;
        dcterms:title "Service Ontology"@en ;
        vann:preferredNamespacePrefix "service" ;
        vann:preferredNamespaceUri "http://purl.org/ontology/service#" ;
        dcterms:description "A micro-ontology that defines the general concept of a service."@en ; 
        dcterms:modified "{GIT_REVISION_DATE}"^^xsd:date ;
        owl:versionInfo "{VERSION}" ;
        cc:license <http://creativecommons.org/licenses/by/3.0/> ;
        dcterms:creator "Jakob Voß" 
    .

# Overview

The following diagram illustrates the classes and properties defined in the
Service Ontology:

``` {.ditaa}

     +-----------------+   provides     +---------------+   consumedBy   +-----------------+
     | ServiceProvider |--------------->|               +--------------->| ServiceConsumer |
     |                 |<---------------+               |<---------------|                 |
     +-----------------+   providedBy   |               |   consumes     +-----------------+
                                        |    Service    |
   +-------------------+   limits       |               |   delay
   | ServiceLimitation |--------------->|               +-------------> duration-or-time
   |                   |<---------------+               +-------------> xsd:nonNegativeInteger
   +-------------------+  limitedBy     |               |   queue
                                        +--+------------+
                                           |         ^
                                           |         |
                                        includes includedIn
                                           |         |
                                           v         |
                                        +------------+--+
                                        |               |
                                        | ServiceObject |
                                        |               |
                                        +---------------+
                                         
```

A [Service], as defined by the Service Ontology is typically provided by a
[ServiceProvider] and/or consumed by a [ServiceConsumer]. Services may further
be constraint by a [ServiceLimitation], by a [delay] and/or by the length of
waiting [queue].

# Classes

## Service

[Service]: #service

A **service** is some action that is done for someone.

Each instance of a Service is also an instance of [gr:ProductOrService] from
[GoodRelations Vocabulary] and [dctype:Service] from [DCMI Metadata Terms]. A
service can be connected to one or more [ServiceProvider] with property
[providedBy] and to one or more [ServiceConsumer] with property [consumedBy].

    service:Service a owl:Class ;
        rdfs:label "Service"@en ;
        rdfs:subClassOf gr:ProductOrService, dctype:Service ;
        rdfs:seeAlso schema:Service, schema:Product ;
        rdfs:isDefinedBy <> .

Most services are also instance of [schema:Product] and/or [schema:Service]
from [Schema.org Vocabulary].

## ServiceProvider

[ServiceProvider]: #serviceprovider

A **service provider** is an entity that is responsible for providing a
[Service].

    service:ServiceProvider a owl:Class ;
        rdfs:label "ServiceProvider"@en ;
        rdfs:seeAlso foaf:Agent, dctype:Agent, gr:BusinessEntity, 
            schema:Person, schema:Organization ;
        rdfs:isDefinedBy <> .

Typical providers, such as organizations and people, are also instances of
[foaf:Agent], [dctype:Agent], [gr:BusinessEntity], and [schema:Person] or
[schema:Organization] but the Service Ontology does not put any constraints on
the nature of providers.

## ServiceConsumer

[ServiceConsumer]: #serviceconsumer

A **service consumer** is an entity that requests or consumes a [Service].

    service:ServiceConsumer a owl:Class ;
        rdfs:label "ServiceConsumer"@en ;
        rdfs:seeAlso foaf:Agent, dctype:Agent, gr:BusinessEntity, 
            schema:Person, schema:Organization ;
        rdfs:isDefinedBy <> .

Typical consumers, such as organizations and people, are instances of
[foaf:Agent], [dctype:Agent], [gr:BusinessEntity], [schema:Person],
[schema:Organization], or [schema:Audience] but the Service Ontology does not
put any constraints on the nature of consumers.

## ServiceLimitation

[ServiceLimitation]: #servicelimitation

A **service limitation** is some obstacle that may limit the use of a
[Service]. For instance the purchase of guns and drugs is limited to
consumers with special permission. Another example is providing a different
product or activity than originally requested. Services and limitations are
connected to each other with properties [limits] and [limitedBy].

    service:ServiceLimitation a owl:Class ;
        rdfs:label "ServiceLimitation"@en ;
        rdfs:isDefinedBy <> .

## ServiceObject

[ServiceObject]: #serviceobject

A **service object** is what the service is provided for. As an example, in the sentence "Peter is lending a car", the 'car' is the object of the service 'loan'.

Use property [includes] to state that a thing is the object of a service. Use property [includedIn] to state that a service is provided for an object.

    service:ServiceObject a owl:Class ;
        rdfs:label "service object"@en ;
        rdfs:comment "The thing which is the object of a service"@en ;
        rdfs:isDefinedBy <> .

# Properties

## provides

[provides]: #provides

Relates a [ServiceProvider] instance that **provides** a [Service] instance to
this service.

    service:provides a owl:ObjectProperty ;
        rdfs:label "provides"@en ;
        rdfs:domain service:ServiceProvider ;
        rdfs:range service:Service ;
        owl:inverseOf service:providedBy ;
        rdfs:isDefinedBy <> .

The existence of a provides relationship between a [ServiceProvider] and a
[Service] often implies the existence of an offer ([gr:Offering] and/or
[schema:Offer]) but this implication is no mandatory part of the Service
Ontology:

```
#   { ?p service:provides ?s } <=> 
#   { ?p a gr:BusinessEntity ; 
#        gr:offers [
#          a gr:Offering , schema:Offer ;
#          gr:includesObject ?s
#       ]
#   }
```

## providedBy

[providedBy]: #providedBy

Relates a [Service] instance that is **provided by** a [ServiceProvider]
instance to this service provider. In most cases this property can be used
interchangeable with [schema:provider].

    service:providedBy a owl:ObjectProperty ;
        rdfs:label "providedBy"@en ;
        rdfs:domain service:Service ;
        rdfs:range service:ServiceProvider ;
        owl:inverseOf service:provides ;
        rdfs:seeAlso schema:provider ;
        rdfs:isDefinedBy <> .

## consumes

[consumes]: #consumes

Relates a [ServiceConsumer] instance that **consumes** a [Service]
instance to this service.

    service:consumes a owl:ObjectProperty ;
        rdfs:label "consumes"@en ;
        rdfs:domain service:ServiceConsumer ;
        rdfs:range service:Service ;
        owl:inverseOf service:consumedBy ;
        rdfs:isDefinedBy <> .

## consumedBy

[consumedBy]: #consumedBy

Relates a [Service] instance that is **consumed by** a [ServiceConsumer]
instance to this service consumer.

    service:consumedBy a owl:ObjectProperty ;
        rdfs:label "consumedBy"@en ;
        rdfs:domain service:Service ;
        rdfs:range service:ServiceConsumer ;
        owl:inverseOf service:consumes ;
        rdfs:seeAlso schema:serviceAudience ;
        rdfs:isDefinedBy <> .

Typical properties that can be used together with this property include
[schema:serviceAudience] and [gr:isConsumableFor].

## limits

[limits]: #limits

Relates a [ServiceLimitation] instance that **limits** a [Service]
instance to this service.

    service:limits a owl:ObjectProperty ;
        rdfs:label "limits"@en ;
        rdfs:domain service:ServiceLimitation ;
        rdfs:range service:Service ;
        owl:inverseOf service:limitedBy ;
        rdfs:isDefinedBy <> .

## limitedBy

[limitedBy]: #limitedBy

Relates a [Service] instance that is **limited by** a [ServiceLimitation]
instance to this service limitation.

    service:limitedBy a owl:ObjectProperty ;
        rdfs:label "limitedBy"@en ;
        rdfs:domain service:Service ;
        rdfs:range service:ServiceLimitation ;
        owl:inverseOf service:limits ;
        rdfs:isDefinedBy <> .

## delay

[delay]: #delay

This property can be used to specify an **estimated period of time or a date**
when a delayed [Service] is expected to take place. Applications SHOULD
use values in the range of [xsd:duration], [xsd:date], [xsd:dateTime], or the
special value "unknown" to indicate a significant delay of unknown duration.
The range may later be extended to a subset of Extended [Date/Time
Format](http://www.loc.gov/standards/datetime/) (EDTF).

    service:delay a owl:DatatypeProperty ;
        rdfs:label "delay"@en ;
        rdfs:domain service:Service ;
        rdfs:seeAlso xsd:duration , xsd:date, xsd:dateTime ;
        rdfs:isDefinedBy <> .

[xsd:dateTime]: http://www.w3.org/TR/xmlschema-2/#dateTime
[xsd:date]: http://www.w3.org/TR/xmlschema-2/#date
[xsd:duration]: http://www.w3.org/TR/xmlschema-2/#duration

Applications SHOULD NOT use this property to specify the normal time of
service. An example of a service with delay is a concert that starts at an
official time (normal time) but there is a delay until band begins to play
(delayed time).

## queue

[queue]: #queue

This property can be used to indicate the **size of a waiting queue** for some
[Service]. Its value MUST be a non-negative integer (0,1,2...).

    service:queue a owl:DatatypeProperty ;
        rdfs:label "queue"@en ;
        rdfs:domain service:Service ;
        rdfs:range xsd:nonNegativeInteger ;
        rdfs:isDefinedBy <> .

## includes

[includes]: #includes

This property relates a [Service] to a thing, which is the object of that service.

    service:includes a owl:ObjectProperty ;
        rdfs:label "includes"@en ;
        rdfs:domain service:Service ;
        owl:inverseOf service:includedIn ;
        rdfs:isDefinedBy <> .

## includedIn

[includedIn]: #includedin

This property relates a thing to a [Service], which is provided on that thing.

    service:includedIn a owl:ObjectProperty ;
        rdfs:label "included in"@en ;
        rdfs:range service:Service ;
        owl:inverseOf service:includes ;
        rdfs:isDefinedBy <> .

# References

* S. Bradner: *Key words for use in RFCs to Indicate Requirement Levels*.
  RFC 2119, March 1997
  <http://tools.ietf.org/html/rfc2119>.

* *Schema.org Vocabular*. 
  <http://schema.org/>.

* *GoodRelations Vocabulary*.
  <http://purl.org/goodrelations/>.

* *DCMI Metadata Terms*.
  <http://dublincore.org/documents/dcmi-terms/>.

* *FOAF Vocabulary*.
  <http://xmlns.com/foaf/spec/>.

[DCMI Metadata Terms]: http://dublincore.org/documents/dcmi-terms/
[dctype:Service]: http://dublincore.org/documents/dcmi-terms/#dcmitype-Service
[dctype:Agent]: http://dublincore.org/documents/dcmi-terms/#dcmitype-Agent

[FOAF Vocabulary]: http://xmlns.com/foaf/spec/
[foaf:Agent]: http://xmlns.com/foaf/spec/#term_Agent

[GoodRelations Vocabulary]: http://www.heppnetz.de/ontologies/goodrelations/
[gr:BusinessEntity]: http://purl.org/goodrelations/v1#BusinessEntity
[gr:Offering]: http://purl.org/goodrelations/v1#Offering
[gr:ProductOrService]: http://purl.org/goodrelations/v1#ProductOrService
[gr:offers]: http://purl.org/goodrelations/v1#offers
[gr:seeks]: http://purl.org/goodrelations/v1#seeks
[gr:isConsumableFor]: http://purl.org/goodrelations/v1#isConsumableFor

[Schema.org Vocabulary]: http://schema.org
[schema:Product]: http://schema.org/Product
[schema:Service]: http://schema.org/Service
[schema:Offer]: http://schema.org/Offer
[schema:Person]: http://schema.org/Person
[schema:Organization]: http://schema.org/Organization
[schema:provider]: http://schema.org/Provider
[schema:serviceAudience]: http://schema.org/serviceAudience
[schema:Audience]: http://schema.org/Audience

