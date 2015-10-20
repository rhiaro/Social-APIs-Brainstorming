# Shape of the SocialAPI notes

This document contains the pieces of the social api described such that they are interoperable but not interdependant. That is, you could choose to implement any one or more of the subsections but are not obligated to implement all of them, or them in any particular order. There are combinations that make more sense, but this should allow for multiple small, granular apps that work together, rather than expecting to get meet all of your social needs from the same place. So for instance, I could go to one domain and use a reader, and another domain to actually post stuff, and actually store the content somewhere else, and a different application to actully subscribe to stuff or check who has replied to me. Or if I find a domain that does all of these things and I like the integrated UX better, I can use that for everything.

This includes federation, by the way.

It is obviously not implementable in its current form as it contains missing parts or multiple conflicting options for some sections. This is what I would hope the group can work to resolve. I'll work through implementing each part, and prompt smaller discussions as I go, and make specific proposals if I want to choose a particular approach to something.

I'm totally open to change of the structure; I have iterated on this a bit and I think this is the best division of concerns. It's also likely I'll have missed some stuff that other people thing is important. That said, the sections are:

Reading - social data is exposed on the web, however it got there. There exists social data; if you find some, here is a rough guide to what to expect and how to get it.

Creating, updating, deleting - pretty self explanatory.

Discovery - is the idea that you can find content from a starting point, which is probably a user profile. Specifically that a user can publish multiple feeds of content, sorted according to whatever criteria they like, and an application like a reader needs a way to find them.

Subscribing - is when you're asking to be notified of some updates, either to a single peice of content or a feed of some kind.

Mentions - is similar, but rather when you're pushed a notification that you didn't necessarily ask for. These are conflated in AP but separated in indieweb. There is overlap; I think it's valuable to think about them separately.

Profiles - I don't want to talk about identity, and obviously auth is out of scope. But we need basically some document to describe the author of some content, or the originator of an activity, or whatever. People can have multiple profiles for whatever purposes, and we needn't constrain what is represented by a profile if it's useful to have a document about something... So, my instinct is to focus only on what affects actual content delivery. This particularly applies to relationships between profiles; it's easy to get bogged down in how people can sort their friends, but what really matters is: when is person A going to see content from person B? So relationships have implications for subscription and access control, but anything more than that is out of scope.


*PROPOSAL:* accept this basic structure of a Social API document and continue development on w3c-social github. Have an implementable draft ready by the f2f.