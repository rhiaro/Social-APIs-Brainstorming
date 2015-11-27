# Indieweb Specs

A number of different specifications for different pieces of the social web puzzle are under ongoing development by the [indieweb](https://indiewebcamp.com) community and others. This document consolidates them for easier comparison with larger specs that cover more peices at once.

As such, this specification is divided into parts (each 2nd level heading begins a 'part') that is intended to be implemented independently as needed, or could be implemented all together in one system, as well as extended to meet domain-specific requirements. Users can store their social data across any number of compliant servers, and use compliant clients at other domains to interact with their own content and the content of others, regardless of *what that content is* or *where it is stored*.

## Overview

## Profiles

A user is represented by a URL. A `GET` on the URL of a user SHOULD return a `text/html` document containing a Microformats2 `h-card` with at least one the following properties (all are optional). The `GET` MAY return additional data (such as a feed).

* name - The full/formatted name of the person or organisation
* honorific-prefix - e.g. Mrs., Mr. or Dr.
* given-name - given (often first) name
* additional-name - other/middle name
* family-name - family (often last) name
* sort-string - string to sort by
* honorific-suffix - e.g. Ph.D, Esq.
* nickname - nickname/alias/handle
* email - email address
* logo - a logo representing the person or organisation
* photo
* url - home page
* uid - universally unique identifier, typically canonical URL
* category - category/tag
* adr - postal address, optionally embed an `h-adr`
* post-office-box
* extended-address - apartment/suite/room name/number if any
* street-address - street number + name
* locality - city/town/village
* region - state/county/province
* postal-code - postal code, e.g. US ZIP
* country-name - country name
* label
* geo or * geo, optionally embed an `h-geo`
* latitude - decimal latitude
* longitude - decimal longitude
* altitude - decimal altitude
* tel - telephone number
* note - additional notes
* bday - birth date
* key - cryptographic public key e.g. SSH or GPG
* org - affiliated organization, optionally embed an h-card
* job-title - job title, previously 'title' in hCard, disambiguated.
* role - description of role
* impp per RFC 4770, new in vCard4 (RFC6350)
* sex - biological sex, new in vCard4 (RFC6350)
* gender-identity - gender identity, new in vCard4 (RFC6350)
* anniversary

## Reading

Whilst publishers are expected to return `text/html` documents marked up with Microformats2 (details below) consumers are likely to pass this through a Microformats2 parser to get Microformats2 JSON, and work with that.

### Streams

A `GET` on a the URL of a stream of content SHOULD return a `text/html` document containing a Microformats2 `h-feed` with at least one of the following properties (all are optional). Children of the `h-feed` SHOULD be one or more Microformats2 `h-entry`s. The `GET` MAY return additional data, such as an `h-card`.

* name - name of the feed
* author - author of the feed, optionally embed an h-card
* url - URL of the feed
* photo - representative photo / icon for the feed

### Objects

A `GET` on the URL of an individual piece of content SHOULD return a `text/html` document containing a Microformats2 `h-entry` with at least one of the following properties (all are optional). The `GET` MAY return additional data, such as an `h-card`.

* name - entry name/title
* summary - short entry summary
* content - full content of the entry
* published - when the entry was published
* updated - when the entry was updated
* author - who wrote the entry, optionally embedded `h-card`(s)
* category - entry categories/tags
* url - entry permalink URL
* uid - universally unique identifier, typically canonical entry URL
* location - location the entry was posted from, optionally embed `h-card`, `h-adr`, or `h-geo`
* syndication - URL(s) of syndicated copies of this post. The property equivalent of rel-syndication (example)
* in-reply-to - the URL which the h-entry is considered reply to (i.e. doesn’t make sense without context, could show up in comment thread), optionally an embedded `h-cite` (reply-context)
* comment - optionally embedded (or nested?) `h-cite`(s), each of which is a comment on/reply to the parent `h-entry`.
* like-of - the URL which the `h-entry` is considered a like (favorite, star) of. Optionally an embedded `h-cite`
* repost-of - the URL which the `h-entry` is considered a repost of. Optionally an embedded `h-cite`.
* photo - one or more photos that is/are considered the primary content of the entry, unless there is a `p-location h-card`, which is still considered a checkin (i.e. with a photo). Otherwise the presence of a `photo` means the name of the entry should be interpreted as a caption on the photo, and the summary/content should be interpreted as a description of the photo.
* rsvp (enum, use <data> element or value-class-pattern: "invited", "yes", "no", "maybe", "interested")

## Subscribing

[A minimum viable subset](https://indiewebcamp.com/How_to_publish_and_consume_PubSubHubbub) of PubSubHubbub 0.4.

## Mentioning

[Webmention](http://webmention.net)

## Creating content

[Micropub](http://micropub.net)