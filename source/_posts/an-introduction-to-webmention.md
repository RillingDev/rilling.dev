---
title: "An Introduction to Webmention"
date: 2022-07-31 00:00:00
updated: 2023-05-07 00:00:00
tags:
    - Java
description: "This article explores how Webmention works, and how to set it up."
---

[Webmention](https://www.w3.org/TR/webmention/) is a technology that allows a website to be notified when another website links to it. It can be used as a simple and privacy-friendly analytics tool to find out where visitors might come from (for example if there is a notable increase in visitors after another blog links to yours). It also enables you to showcase "other blogs that linked to this post"-like elements on your blog posts, to suggest related posts to your readers.

This article explores how Webmention works, and how to set it up.

<!-- more -->

## How It Works

The Webmention technology consists of two parts:

- A client that notifies a website that they were mentioned from another website.
- A web server that listens for Webmentions from clients.

The website mentioning another is called the _source_. The website being mentioned is the _target_.

The following is an example of the [steps taken](https://www.w3.org/TR/webmention/#webmention-protocol) when sending a Webmention, using both this blog post, and [a blog post by Xe Iaso](https://xeiaso.net/blog/webmention-support-2020-12-02/) (from which I learned about Webmention). Here the source website is `https://rilling.dev/blog/an-introduction-to-webmention/`, and the target website is `https://xeiaso.net/blog/webmention-support-2020-12-02/`.

### Sending Webmentions

A client can notify a target website that it was mentioned from another website (the source).

#### Create a Website That Mentions Another One

The source website contains a [`<a>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) with a [`href` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#href) that links to the target website.

```html
<a href="https://xeiaso.net/blog/webmention-support-2020-12-02/">
	a blog post by Xe Iaso
</a>
```

Other possible kinds of links include media links, such as the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img), [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio), or [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) tags with the corresponding [`src` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#src). Non-HTML sources can also mention other websites, like [JSON](https://www.json.org/json-en.html) or plain text documents. In these cases, any value that looks like a URL is treated as a mention.

#### Find the Webmention Endpoint of the Target Website

The Webmention endpoint of the target website is discovered. The target website advertises its endpoint by either an HTML [`<link>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) with the [`rel` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#rel) set to `webmention`:

```html
<link href="https://mi.within.website/api/webmention/accept" rel="webmention" />
```

or by including the [`Link` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) in the response:

```http
Link: <https://mi.within.website/api/webmention/accept>; rel="webmention"
```

In this example, the Webmention endpoint of the target website is discovered to be `https://mi.within.website/api/webmention/accept`.

If the target website doesn't include either of the above, Webmentions can't be sent for this website because no Webmention endpoint exists.

#### Send the Webmention

The Webmention is sent to the endpoint as an HTTP POST request. It contains two `x-www-form-urlencoded` parameters: the source and the target URL.

```http
POST https://mi.within.website/api/webmention/accept HTTP/1.1
Content-Type: application/x-www-form-urlencoded

source=https://rilling.dev/blog/an-introduction-to-webmention/&
target=https://xeiaso.net/blog/webmention-support-2020-12-02/
```

If the endpoint accepts the Webmention, it responds with a 2xx status code.

### Receiving Webmentions

The endpoint receives Webmentions from clients.

#### Listen for Webmention

Upon receiving the HTTP POST request described above in ["Send the Webmention"](#send-the-webmention), the endpoint extracts the submitted source and target website.

```txt
source=https://rilling.dev/blog/an-introduction-to-webmention/
target=https://xeiaso.net/blog/webmention-support-2020-12-02/
```

#### Verify the Webmention

To ensure that the Webmention is valid, the endpoint verifies that the source website does mention the target website. This is done by fetching the source website's contents, and checking them based on the criteria described above in ["Create a Website That Mentions Another One"](#create-a-website-that-mentions-another-one). If the source website contains a link to the target website, the verification passes. Otherwise, the submitted Webmention is rejected.

#### Do Something With the Webmention

At this point, the endpoint has received a valid Webmention. What is done with the received Webmention is up the endpoint. For my website, Webmentions are simply written to a file and can be analyzed manually. Some [other blogs](https://xeiaso.net/blog/webmention-support-2020-12-02/) display the received Webmentions at the end of the target page.

## Setting up Webmention Yourself

If you want to support Webmention on your website, here is how:

### Sending Webmentions

You can use a client to send Webmentions to websites that your website links to (if the target website supports Webmention).
Pick any [Webmention client implementation](https://webmention.net/implementations/#sending). The implementation I use is [webmention4j](https://github.com/RillingDev/webmention4j) (_Disclaimer: I am the author of it_) which is a Java implementation with both a Webmention client and a server.

### Receiving Webmentions

To receive Webmentions, your website has to have an endpoint for them.
There are [several server implementations](https://webmention.net/implementations/#receiving) for Webmention endpoints in all kinds of programming languages. Once you've picked an implementation, you will have to set it up to be reachable from the internet with a fitting URL, such as `https://example.com/webmention`.
To advertise to clients that your website has a Webmention Endpoint, see the steps in ["Find the Location of the Target Website's Webmention Endpoint"](#find-the-webmention-endpoint-of-the-target-website) (either include an HTML `link` tag or the `Link` header in the HTTP responses of your website).
Webmention clients can now detect your website's endpoint and send Webmentions to it.
The implementation I use is the previously mentioned [webmention4j](https://github.com/RillingDev/webmention4j) I created.
