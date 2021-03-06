---
title: SIP Contexts | SIP.js
description: Contexts are SIP.js objects that help your WebRTC app handle SIP requests and define what happens after a request is accepted.
---

# Context

## What is a Context?

Let's get this out of the way first:  *context is not a SIP term.*  We made it up. Sorry. That said, it is an incredibly useful concept for a few reasons.

In the realm of SIP, requests and responses are sent from one user agent to another. These individual messages make up a SIP *transaction*. Transactions are able to track forked requests, retransmissions, timeouts, provisional responses, etc. As soon as the request is rejected, the transaction terminates. Any new requests create completely new transactions. Sometimes these requests are related, though, such as when the first request was challenged by an authenticating proxy.  Contexts let us wrap these two requests into one logical object.

Contexts also provide another use:  *they stick around.* SIP transactions terminate as soon as the request receives a final response, even if the request is accepted. Since the contexts don't go away, we can use them to describe the result of the request.  For example, a `ServerContext` contains the body of the accepted message, which an application can display to the user. An INVITE's `SIP.ClientContext` becomes the SIP session created by the accepted INVITE request (as a `SIP.Session`). This is where the contexts get their name. They represent not the request, but the entire surrounding context of where the request came from and what it is actually requesting.

## Flavors of Context Objects

Take a moment to think about your favorite kind of ice cream. Maybe it's chocolate with fudge swirls or pistachio with real nuts, or maybe just vanilla with a few fresh berries. Notice that these ice creams have two parts: the base (chocolate, pistachio, or vanilla) and the mixin (fudge, nuts, or berries). Contexts are, in a much less delicious way, kind of like ice cream. They have two parts, and they match up to their two main uses.

1. First, the base. This part of the Context *deals with handling the request, itself.* If you're sending a MESSAGE request, for example, your Context will be a `ClientContext` and will send the request, handle any authorization challenges and retransmissions, and notify you via events when the MESSAGE is accepted or rejected. If you're receiving the request, your context will be a `ServerContext`, and it will do the opposite:  notifying you that you received the request and allowing you to respond to it.

2. Second, the mixins. This part of the Context *defines what will happen after the request is accepted.*  An INVITE request will use the `Session` to define session methods such as `hold()` and `refer()`, while MESSAGE requests use `Message` to parse the body of the MESSAGE.

While some people may like plain ice cream or snacking on chocolate chips, creating a `ServerContext` or `Session` alone isn't very useful. The magic happens when you *mix them together.* That's why in the source code you'll typically see objects called `InviteServerContext` or `MessageClientContext`. They each share only the behavior they need for the *context* in which the request was encountered.

## Read on

* [`SIP.ClientContext`](/api/0.5.0/context/client/) and [`SIP.ServerContext`](/api/0.5.0/context/server/) documentation
* [`SIP.Session`](/api/0.5.0/session) documentation
* [`SIP.Message`](/api/0.5.0/message) documentation

