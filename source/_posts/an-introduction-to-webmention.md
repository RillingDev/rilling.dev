---
title: "An Introduction to Webmention"
date: 2022-07-31 00:00:00
updated: 2023-04-30 00:00:00
tags:
    - Webmention
    - Java
---

Some time ago I stumbled over [a blog post by Xe Iaso](https://xeiaso.net/blog/webmention-support-2020-12-02) which mentions (pun intended) '[Webmention](https://www.w3.org/TR/webmention/)'. Webmention is a technology that allows websites to be notified when another website links to it.
Webmention can work like a simple and privacy-friendly analytics tool to find out where visitors might come from (e.g. if there is a notable increase in visitors after another blog links to yours).
It also enables you to showcase "other blogs that linked to this post"-like elements on your blog posts, to suggest related posts to your readers.

<!-- more -->

## How It Works

The Webmention technology consists of two parts:

-   A client that notifies a website that they were mentioned from another website.
-   A web server that listens for Webmentions from clients.

The website mentioning another is called the _source_. The website being mentioned is the _target_.

The following is an example of [steps taken](https://www.w3.org/TR/webmention/#webmention-protocol) when sending a Webmention. The source website is (`https://rilling.dev/blog/an-introduction-to-webmention/`), and the target website is `https://xeiaso.net/blog/webmention-support-2020-12-02`.

### 1. Sending Webmentions

#### 1.1. Create a Website That Mentions Another One

The source website contains a [`<a>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) with a [`href` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-href) that links to the other website.

```html
<a href="https://xeiaso.net/blog/webmention-support-2020-12-02">
	a blog post by Xe Iaso
</a>
```

Other possible kinds of links include media links, such as the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img), [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio), or [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) tags with the corresponding [`src` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-src). Non-HTML sources can also mention other websites, like [JSON](https://www.json.org/json-en.html) or plain text documents. In these cases, any value that looks like a URL is treated as a mention.

#### 1.2. Find the Location of the Target Website's Webmention Endpoint

The target website advertises its Webmention endpoint. This is either done via an HTML [`<link>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) with the [`rel` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#rel) set to `webmention`:

```html
<link href="https://mi.within.website/api/webmention/accept" rel="webmention" />
```

or by including the [`Link` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) in the response:

```http
Link: <https://mi.within.website/api/webmention/accept>; rel="webmention"
```

In this example, the Webmention endpoint of the target website is discovered to be `https://mi.within.website/api/webmention/accept`.

If the response of the target website does not include either of the above, Webmentions cannot be sent for this website because no Webmention endpoint exists.

#### 1.3. Send the Webmention

An HTTP POST request is sent to the endpoint. It contains two `x-www-form-urlencoded` parameters: the source and the target URL.

```http
POST https://mi.within.website/api/webmention/accept HTTP/1.1
Content-Type: application/x-www-form-urlencoded

source=https://rilling.dev/blog/an-introduction-to-webmention/&
target=https://xeiaso.net/blog/webmention-support-2020-12-02
```

If the endpoint accepted the Webmention, it responds with a 2xx status code.

### 2. Receiving Webmentions

#### 2.1. Listen for Webmention

Upon receiving the HTTP POST request described above in ["1.3. Send the Webmention"](#1-3-send-the-webmention), the endpoint extracts the submitted source and target website.

```txt
source=https://rilling.dev/blog/an-introduction-to-webmention/
target=https://xeiaso.net/blog/webmention-support-2020-12-02
```

#### 2.2. Verify the Webmention

To ensure that the Webmention is valid, the endpoint has to verify that the source website does mention the target website. This is done by fetching the source website's contents, and checking them based on the criteria described above in ["1.1. Create a Website That Mentions Another One"](#1-1-create-a-website-that-mentions-another-one).
If the source website contains a link to the target website, the verification passes. Otherwise, the submitted Webmention is rejected.

#### 2.3. Do Something With the Webmention

At this point, the endpoint has received a valid Webmention. What is done with the received Webmention is up the endpoint. For my website, Webmentions are simply written to a file and can be analyzed manually. Some [other blogs](https://xeiaso.net/blog/webmention-support-2020-12-02) display the received Webmentions at the end of the target page.

## Setting up Webmention Yourself

### Sending Webmentions

Pick a [Webmention client implementation](https://webmention.net/implementations/#sending). The one I use is [webmention4j](https://github.com/FelixRilling/webmention4j) (_Disclaimer: I am the author of it_) which is a Java implementation with both a Webmention client and a server.
You can use this client to send Webmentions to websites that your website links to if the target website supports Webmention.

### Receiving Webmentions

Start by picking how you want to listen for Webmentions. There are [several implementations](https://webmention.net/implementations/#receiving) for endpoints in all kinds of programming languages. The one I use is again my own, [webmention4j](https://github.com/FelixRilling/webmention4j). Once you've picked an implementation, you will have to set it up to be reachable from the internet with a fitting URL, such as `https://example.com/webmention`.

To advertise to clients that your website has a Webmention Endpoint, see the steps in ["1.2. Find the Location of the Target Website's Webmention Endpoint"](#1-2-find-the-location-of-the-target-website’s-webmention-endpoint) (either include an HTML `link` tag or the `Link` header in the HTTP responses of your website).

Webmention clients will now be able to detect your website's endpoint and can send Webmentions to it.
