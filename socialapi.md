---
layout: default
---

# The Social API

## Status of this document

Early working draft, based on [Social API Requirements](https://www.w3.org/wiki/Socialwg/Social_API/Requirements); inspired by and borrowing from [ActivityPump](http://w3c-social.github.io/activitypump/), the Indieweb ecosystem (including [Micropub](http://indiewebcamp.com/Micropub) and [Webmention](http://indiewebcamp.com/Webmention)) and [SoLiD](http://linkeddata.github.io/SoLiD/); all are subject to ongoing development. PRs and issues please!.

## Overview

People and the content they create are the core componants of the social web; they make up the social graph. This document describes a standard way in which people can:

* connect with other people and subscribe to their content;
* create, update and delete social content;
* interact with other people's content;
* be notified when other people interact with their content;

regardless of *what that content* is or *where it is stored*.

This provides the core building blocks for interoperable social systems.

This specification is divided into parts that can be implemented independantly as needed, or all together in one system, as well as extended to meet domain-specific requirements. Users can store their social data across any number of compliant servers, and use compliant clients hosted elsewhere to interact with their own content and the content of others. Put simply, this specification tells you:

* how to publish social content, and how to discover content someone has published.
* how to subscribe to a particular stream or feed of content.
* what to post, and where to, to create, update or delete content.
* what to post, and where to, to notify relevant parties of changes to content.
* what to post, and where to, to notify relevant parties of interactions with content.
* ...

## Publishing

Upon [discovery](#discovery) of the URL of a content object or stream of content:

* a `GET` retrieves the JSON representation of the object or objects in the stream;
  * in reverse chronological order where applicable;
  * which SHOULD be structured according to [ActivityStreams](#) (either Activities or Content Objects);
  * which MAY be embedded in a HTML representation of the object or objects;
  * **TODO:** limit/paging

Each object in a stream MUST have a globally unique identifier (HTTP URI) in the `@id` property, and MAY contain only this identifier, which can be dereferenced to retrieve all properties of an object.


<div class="issue">
  <span class="issue-title">ISSUE</title>
  <p>Microformats uses `url` not `@id`</p>
</div>


**TODO:** Example single object.

**TODO:** Example stream of objects.

## Subscribing

An agent (client or server) may wish to be notified of changes to a content object (eg. edits, new replies) or stream of content (eg. objects added or removed from the stream).

Here are some options for indicating this...

* **ActivityPump**: The subscriber posts a `Follow` Activity (JSON object) to the target's `inbox` endpoint, and adds the target to the subscriber's `Following` Collection. The target's server adds the subscriber to the target's `Followers` Collection, and subsequently `POST`s all new activities of the target to the subscriber's `inbox` endpoint. (*See [ActivityPump](http://w3c-social.github.io/activitypump/) 7.4.2, 8 and 9.2.4*)
* **SoLiD**: The subscriber sends the keyword `sub` followed by an empty space and then the URI of the resource, to the target's websockets URI. The target's server sends a websockets message containing the keyword `pub`, followed by an empty space and the URI of the resource that has changed, whenever there is a change. (*See [SoLiD - Live Updates](https://github.com/solid/solid-spec#live-updates)*)
* **PubSubHubbub**: The subscriber discovers the target's hub, and sends a form-encoded `POST` request containing values for `hub.mode` ("subscribe"), `hub.topic` and `hub.callback`. When the target posts new content, the target's server sends a form-encoded `POST` to the hub with values for `hub.mode` ("publish") and `hub.url` and the hub checks the URL for new content and `POST`s updates to the subscriber's callback URL. (*See [PuSH 0.4](http://pubsubhubbub.github.io/PubSubHubbub/pubsubhubbub-core-0.4.html) and [How To Publish And Consume PuSH](http://indiewebcamp.com/How_to_publish_and_consume_PubSubHubbub)*)
* **Salmentions**: The subscriber creates content that links to the target (eg. a reply) and sends a form-encoded `POST` containing values for `source` and `target` to the target's webmention [webmention](https://indiewebcamp.com/webmention) endpoint. The target verifies the link and includes a link back to the subscriber's source on the target content. The target sends form-encoded `POST` requests containing values for `source` and `target` to the webmention endpoint of every link in the content, including that of the subscriber, to indicate that there has been a change. (*See [webmention](https://indiewebcamp.com/webmention) and [salmentions](https://indiewebcamp.com/salmentions)*)

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
