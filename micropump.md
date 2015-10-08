# The Social API

**Editor's note:** Early draft, based on [Social API Requirements](https://www.w3.org/wiki/Socialwg/Social_API/Requirements); inspired by and borrowing from [ActivityPump](http://w3c-social.github.io/activitypump/), the Indieweb ecosystem (including [Micropub](http://indiewebcamp.com/Micropub) and [Webmention](http://indiewebcamp.com/Webmention)) and [SoLiD](http://linkeddata.github.io/SoLiD/); all are subject to ongoing development. PRs and issues please!.

## Overview

People and the content they create are the core componants of the social web; they make up the social graph. This document describes a standard way in which people can:

* connect with other people and subscribe to their content;
* create, update and delete social content;
* interact with other people's content;
* be notified when other people interact with their content;

regardless of *what that content* is or *where it is stored*.

This provides the core building blocks for interoperable social systems which allow people to express themselves, ideas to be shared, organisations to collaborate, and..

This specification is divided into parts that can be implemented independantly as needed, or all together in one system, as well as extended to meet domain-specific requirements. Users can store their social data across any number of compliant servers, and use compliant clients hosted elsewhere to interact with their own content and the content of others. Put simply, this specification tells you:

* given an identifier (URI) for a person (persona/organisation/group/other), how to discover content they have published.
* how to subscribe to a particular stream or feed of content.
* what to post, and where to, to create, update or delete content.
* what to post, and where to, to notify relevant parties of changes to content.
* what to post, and where to, to notify relevant parties of interactions with content.
* ...

## Reading

Upon [discovery](#discovery) of the URL of a content object or stream of content:

* a `GET` retrieves the JSON representation of the object or objects in the stream;
  * in reverse chronological order where applicable;
  * which MAY be embedded in a HTML representation of the object or objects;
  * **TODO:** limit/paging

## Subscribing

* follow activity
* PuSH
*
## Discovery

* different types of feeds (by post type, user curated, by topic)

## 'What to post'

### Syntax

| ActivityPump | Micropub | SoLiD |
| ------------ | -------- | ----- |
| JSON-LD | form-encoded or JSON | RDF (Turtle or JSON-LD) |

### Vocabulary

| ActivityPump | Micropub | SoLiD |
| ------------ | -------- | ----- |
| ActivityStreams 2.0 | Microformats2 | Any suitable RDF ontology (eg. FOAF, SIOC) |


## Authorization

* Bearer tokens for authentication
* Leave obtaining the bearer token out of the spec, since there are already several RFCs for ways to obtain bearer tokens.
* ...access control...

### Identity

* Profiles

## Creating content

`POST` a JSON object (see [what to post](#what-to-post)) to the appropriate endpoint.

<!--

|              | ActivityPump | Micropub |
| ------------ | --------------------------------------------- | -------- |
| **Endpoint** | discoverable outbox                           | `rel="micropub"` |
| **Create**   | `{`                                           | Form-encoding: |
|              | ` "@type": "Create",`                         | `h=entry&` |
|              | ` "published": "2015-05-15T13:06:00+02:00",`  | `content=hello+moon&` |
|              | ` "actor": "http://rhiaro.co.uk/about#me",`   | `category[]=indieweb&` |
|              | ` "object": {`                                | `category[]=micropub&` |
|              | `    "content": "hello world",`               | `author=http://rhiaro.co.uk/about#me&` |
|              | `    "category": ["indieweb","micropub"]`     | `published=2015-05-15T13:06:00+02:00` |
|              | `  }`                                         | JSON: |
|              | `}`                                           | `{` |
|              |                                               | `  "type": [h-entry],` |
|              |                                               | `  "properties": {` |
|              |                                               | `    "content": ["hello world"],` |
|              |                                               | `    "category": ["indieweb","micropub"]` |
|              |                                               | `  }` |
|              |                                               | `}` |
| **Update**   |                                               | |
| **Delete**   |                                               | |

-->

### Updating

### Deleting

## Notifications

### Propagating content

* AP section 8, posting to inbox
* webmentions

### Updating the social graph

side effects, adding to collections etc
