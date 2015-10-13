---
layout: default
---

# The Social API

## Status of this document

Early working draft, based on [Social API Requirements](https://www.w3.org/wiki/Socialwg/Social_API/Requirements); inspired by and borrowing from [ActivityPump](http://w3c-social.github.io/activitypump/), the Indieweb ecosystem (including [Micropub](http://indiewebcamp.com/Micropub) and [Webmention](http://indiewebcamp.com/Webmention)) and [SoLiD](https://github.com/solid/solid-spec); all are subject to ongoing development. 

**Editor's note:** Optimistically, ultimately an implementation of any subsection of this spec should be compatible with the equivalent subsection of one of the aforementioned specs. Sometimes the overlap between spec subsections is unclear (to me), so I've written them all out for now.

## Overview

People and the content they create are the core componants of the social web; they make up the social graph. This document describes a standard way in which people can:

* connect with other people and subscribe to their content;
* create, update and delete social content;
* interact with other people's content;
* be notified when other people interact with their content;

regardless of *what that content* is or *where it is stored*.

This provides the core building blocks for interoperable social systems.

This specification is divided into parts that can be implemented independantly as needed, or all together in one system, as well as extended to meet domain-specific requirements. Users can store their social data across any number of compliant servers, and use compliant clients hosted elsewhere to interact with their own content and the content of others. Put simply, this specification tells you:

* how to [publish](#publishing) social content, and how to [discover](#discovery) content someone has published.
* how to [subscribe](#subscribing) to a particular stream or feed of content.
* what to post, and where to, to create, update or delete content.
* what to post, and where to, to notify relevant parties of changes to content.
* what to post, and where to, to notify relevant parties of interactions with content.
* ...

## Publishing and Consuming

Each stream MUST have a globally unique identifier (HTTP URI). Each object in a stream MUST have a globally unique identifier (HTTP URI) in the `@id` property, and MAY contain only this identifier, which can be dereferenced to retrieve all properties of an object.

Upon [discovery](#discovery) of the URL of a content object or stream of content a `GET` retrieves the JSON[-LD] representation of the object or objects in the stream, or an HTML representation from which the equivalent JSON representation can be parsed.

A JSON-LD representation which SHOULD be structured according to [ActivityStreams](#) (either Activities or Content Objects), but MAY use other vocabularies. Where ActivityStreams is used, a stream of objects SHOULD be of type ActivityStreams `Collection`.
 

**TODO:** limit/paging

<div class="issue">
  <div class="issue-title"><span>Issue</span></div>
  <div>Microformats uses <code>url</code> not <code>@id</code></div>
</div>


**TODO:** Example single object.

**TODO:** Example stream of objects.

## Discovery

One user may [publish](#publishing) one or more streams of content. Streams may be generated automatically or manually, and might be segregated by post type, topic, audience, or any arbitrary criteria decided by the curator of the stream. The result of a `GET` on the HTTP URI of a [profile](#profiles) MAY include links to other streams, which a consumer could follow to read or subscribe to. Eg.

`<link rel="stream" href="http://rhiaro.co.uk/tag/socialwg">`

```
HTTP/1.1 200 OK
....
Link: <http://rhiaro.co.uk/tag/socialwg>; rel="stream"
```

<div class="issue">
  <div class="issue-title"><span>Issue</span></div>
  <div>Strawman. Is 'stream' right or should be 'feed'? Could/should be JSON? How to include additional info, eg. title of stream/feed?</div>
</div>

* **h-feed**: `rel=feed` (*See [h-feed](http://indiewebcamp.com/h-feed#rel_feed)*)
* **ActivityPump**: contains some pre-defined ActivityStreams `Collections`, whose URIs are discoverable from the JSON returned by `GET`ing a user's profile, eg. via the `inbox` and `outbox` properties. (*See [ActivityPump - Discovery](http://w3c-social.github.io/activitypump/#endpoint-discovery)*)

## Subscribing

An agent (client or server) may wish to be notified of changes to a content object (eg. edits, new replies) or stream of content (eg. objects added or removed from the stream).

Here are some options for indicating this...

* **ActivityPump**: The subscriber posts a `Follow` Activity (JSON object) to the target's `inbox` endpoint, and adds the target to the subscriber's `Following` Collection. The target's server adds the subscriber to the target's `Followers` Collection, and subsequently `POST`s all new activities of the target to the subscriber's `inbox` endpoint. (*See [ActivityPump](http://w3c-social.github.io/activitypump/) 7.4.2, 8 and 9.2.4*)
* **SoLiD**: The subscriber sends the keyword `sub` followed by an empty space and then the URI of the resource, to the target's websockets URI. The target's server sends a websockets message containing the keyword `pub`, followed by an empty space and the URI of the resource that has changed, whenever there is a change. (*See [SoLiD - Live Updates](https://github.com/solid/solid-spec#live-updates)*)
* **PubSubHubbub**: The subscriber discovers the target's hub, and sends a form-encoded `POST` request containing values for `hub.mode` ("subscribe"), `hub.topic` and `hub.callback`. When the target posts new content, the target's server sends a form-encoded `POST` to the hub with values for `hub.mode` ("publish") and `hub.url` and the hub checks the URL for new content and `POST`s updates to the subscriber's callback URL. (*See [PuSH 0.4](http://pubsubhubbub.github.io/PubSubHubbub/pubsubhubbub-core-0.4.html) and [How To Publish And Consume PuSH](http://indiewebcamp.com/How_to_publish_and_consume_PubSubHubbub)*)
* **Salmentions**: The subscriber creates content that links to the target (eg. a reply) and sends a form-encoded `POST` containing values for `source` and `target` to the target's webmention [webmention](https://indiewebcamp.com/webmention) endpoint. The target verifies the link and includes a link back to the subscriber's source on the target content. The target sends form-encoded `POST` requests containing values for `source` and `target` to the webmention endpoint of every link in the content, including that of the subscriber, to indicate that there has been a change. (*See [webmention](https://indiewebcamp.com/webmention) and [salmentions](https://indiewebcamp.com/salmentions)*)

## 'What to post'

### Syntax

| ActivityPump | Micropub | SoLiD |
| ------------ | -------- | ----- |
| JSON-LD | form-encoded or JSON | RDF (Turtle or JSON-LD) |

### Vocabulary

| ActivityPump | Micropub | SoLiD |
| ------------ | -------- | ----- |
| ActivityStreams 2.0 | Microformats2 | Any suitable RDF ontology (eg. FOAF, SIOC) |


## Creating content

`POST` a JSON object (see [what to post](#what-to-post)) to the appropriate endpoint.

* **Micropub**: `POST` to `rel="micropub"` (*See [Micropub](https://indiewebcamp.com/micropub)*)
* **ActivityPump**: `POST` to `"outbox": "..."` (*See [ActivityPump 7.4.1](http://w3c-social.github.io/activitypump/#outbox)*)
* **SoLiD**: `POST` to an LDP container (*See [SoLiD - Creating new resources](https://github.com/solid/solid-spec#creating-new-resources)*)

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

## Profiles

The subject of a profile document can be a person, persona, organisation, bot, location, ...whatever. Each profile document MUST have a globally unique identifier (HTTP URI). Performing a `GET` on a profile document SHOULD return a JSON object containing attributes of the subject of the profile; MAY return objects the subject has created, such as an ActivityStreams `Collection`; and SHOULD return at least one link to a stream of content (see [discovery](#discovery)). The JSON representation of a profile document MAY be embedded in an HTML representation (eg. via Microformats (`h-card`) or RDFa).

### Authorization

* Bearer tokens for authentication
* Leave obtaining the bearer token out of the spec, since there are already several RFCs for ways to obtain bearer tokens.
* ...access control...
