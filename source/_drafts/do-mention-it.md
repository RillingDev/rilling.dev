---
title: "Do Mention It"
date: 2022-04-09
updated: 2022-04-09
tags:
    - Webmention
    - Java
---

Some time ago I stumbled over [a blog post by Xe Iaso](https://xeiaso.net/blog/webmention-support-2020-12-02) which mentions (pun intended) '[Webmention](https://www.w3.org/TR/webmention/)'. Webmention is a technology that allows websites to send notifications when they mention another site, as well as receiving a notification when one's site is mentioned from somewhere else.

When implemented, Webmention can work like a very simple but privacy friendly analytics tool to find out where visitors might come from (e.g., if there is a notable increase in visitors after another blog mentions yours).
It can also enable you to showcase "other blogs that linked to this post"-like elements on your blog posts which could lead readers to related posts.

<!-- more -->

## How It Works

Webmention consists of two parts: The 'sending' part, where a client notifies another website that they were mentioned, and the the 'receiving' part, where a webserver takes HTTP requests from clients that send Webmentions to it.

The website that does mention another is referred to as the _"source"_, while the website being mentioned is the _"target"_.

### Sending Webmentions

The following is an example of [steps taken](https://www.w3.org/TR/webmention/#webmention-protocol) when sending a webmention, with the source site being this one (`https://rilling.dev/blog/do-mention-it/`), and the target site being Xe's blog post `https://xeiaso.net/blog/webmention-support-2020-12-02`.

#### 1. Create a website that mentions another one

This is done by having an [`<a>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) with a [`href` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-href) that links to the other site. In the case of this blog post, the link to Xe's blog post would be a mention of Xe's website:

```html
<a href="https://xeiaso.net/blog/webmention-support-2020-12-02">
	a blog post by Xe Iaso
</a>
```

Other possible kinds of links include media links, such as the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img), [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio) or [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) tags with the corresponding [`src` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-src). Non-HTML sources can also mention other sites, for example [JSON](https://www.json.org/json-en.html) or plain text documents. In these cases any value that looks like an URL is treated as a link.

#### 2. Find the location of the target site's Webmention endpoint

In order to be able to receive Webmentions, the target site has to advertise their Webmention endpoint. This is either done via a HTML [`<link>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) with the [`rel` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#rel) set to `webmention`:

```html
<link href="https://mi.within.website/api/webmention/accept" rel="webmention" />
```

or by having the [`Link` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) in their response:

```http
Link: <https://mi.within.website/api/webmention/accept>; rel="webmention"
```

Note: If the response of the target site does not include either, Webmentions cannot be sent to this site because no Webmention endpoint exists.

In this example, the Webmention endpoint was discovered to be `https://mi.within.website/api/webmention/accept`.
