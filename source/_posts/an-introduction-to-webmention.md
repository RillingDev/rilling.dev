---
title: "An Introduction to Webmention"
date: 2022-07-31 00:00:00
updated: 2022-07-31 00:00:00
tags:
    - Webmention
    - Java
---

Some time ago I stumbled over [a blog post by Xe Iaso](https://xeiaso.net/blog/webmention-support-2020-12-02) which mentions (pun intended) '[Webmention](https://www.w3.org/TR/webmention/)'. Webmention is a technology that allows websites to be notified when another website links to it.
When implemented, Webmention can work like a primitive but privacy-friendly analytics tool to find out where visitors might come from (e.g., if there is a notable increase in visitors after another blog mentions yours).
It can also enable you to showcase "other blogs that linked to this post"-like elements on your blog posts to suggest related posts to your readers.

<!-- more -->

## How It Works

Webmention consists of two parts: The sending part, where a client notifies another website that they were mentioned, and the receiving part, where a web server takes HTTP requests from clients that send Webmentions to it. The website that does mention another is called the _"source"_, while the website being mentioned is the _"target"_.

### 1. Sending Webmentions

The following is an example of [steps taken](https://www.w3.org/TR/webmention/#webmention-protocol) when sending a Webmention, with the source website being this one (`https://rilling.dev/blog/an-introduction-to-webmention/`), and the target website being Xe's blog post `https://xeiaso.net/blog/webmention-support-2020-12-02`.

#### 1.1. Create a Website That Mentions Another One

This is done by having an [`<a>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) with a [`href` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-href) that links to the other website. In the case of this blog post, the link to Xe's blog post would be a mention of Xe's website:

```html
<a href="https://xeiaso.net/blog/webmention-support-2020-12-02">
	a blog post by Xe Iaso
</a>
```

Other possible kinds of links include media links, such as the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img), [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio) or [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) tags with the corresponding [`src` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-src). Non-HTML sources can also mention other websites, like [JSON](https://www.json.org/json-en.html) or plain text documents. In these cases, any value that looks like a URL is treated as a mention.

#### 1.2. Find the Location of the Target Website's Webmention Endpoint

To be able to receive Webmentions, the target website has to advertise its Webmention endpoint. This is either done via an HTML [`<link>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) with the [`rel` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#rel) set to `webmention`:

```html
<link href="https://mi.within.website/api/webmention/accept" rel="webmention" />
```

or by having the [`Link` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) in the response:

```http
Link: <https://mi.within.website/api/webmention/accept>; rel="webmention"
```

In this example, the Webmention endpoint was discovered to be `https://mi.within.website/api/webmention/accept`.

Note: If the response of the target website does not include either, Webmentions cannot be sent to this website because no Webmention endpoint exists.

#### 1.3. Send the Webmention

Now that we have the endpoint, we send a POST request with two `x-www-form-urlencoded` parameters: the source and the target URL.

```http
POST https://mi.within.website/api/webmention/accept HTTP/1.1
Content-Type: application/x-www-form-urlencoded

source=https://rilling.dev/blog/an-introduction-to-webmention/&
target=https://xeiaso.net/blog/webmention-support-2020-12-02
```

If everything worked, the server will respond with a 2xx status code.
What is done with the received Webmention is up the endpoint server.

### 2. Receiving Webmentions

#### 2.1. Listen for Webmention

Upon receiving a post request as described above in ["1.3. Send the Webmention"](#1-3-send-the-webmention), the endpoint extracts the submitted source and target website.

```txt
source=https://rilling.dev/blog/an-introduction-to-webmention/
target=https://xeiaso.net/blog/webmention-support-2020-12-02
```

#### 2.2. Verify the Webmention

To ensure a valid Webmention was received, the endpoint has to validate that the source website does mention the target website. This is done by fetching the source page's contents, and checking them based on the criteria described above in ["1.1. Create a Website That Mentions Another One"](#1-1-create-a-website-that-mentions-another-one).
If the source page contains a link to the target page, the verification passes. Otherwise, the submitted Webmention is rejected.

#### 2.3. Do Something with the Webmention

At this point, the endpoint has received a known-to-be-valid Webmention and can do with it as it pleases. For my website, Webmentions are simply written to a file and can be analyzed manually. Some [other blogs](https://xeiaso.net/blog/webmention-support-2020-12-02) display Webmentions at the end of the target page.

## Setting up Webmention Yourself

### Sending Webmentions

Pick a [Webmention client implementation](https://webmention.net/implementations/). The one I use is [webmention4j](https://github.com/FelixRilling/webmention4j) (_Disclaimer: I am the author of it_) which is a Java implementation with both a Webmention client and a server.
You can then use that Webmention client to send Webmentions for any website that yours links to which has a Webmention endpoint.

### Receiving Webmentions

To start, pick how you want to listen for Webmentions. There are [several implementations](https://webmention.net/implementations/) in all kinds of languages. The one I use is again my own, [webmention4j](https://github.com/FelixRilling/webmention4j).
Once you've picked an implementation, you will have to set it up to be reachable from the internet with a fitting URL, such as `https://example.com/webmention`.

To advertise to clients that your website has a Webmention Endpoint, see the steps in ["1.2. Find the Location of the Target Website's Webmention Endpoint"](#1-2-find-the-location-of-the-target-website’s-webmention-endpoint) - either include an HTML `link` tag or the `Link` header in the HTTP responses of your website:

```html
<link href="https://example.com/webmention" rel="webmention" />
```

or

```http
Link: <https://example.com/webmentio>; rel="webmention"
```

Webmention clients will now be able to detect your website's endpoint and can send Webmentions to it.
