---
title: "It's All in My <head>"
date: 2016-12-13
updated: 2023-09-10
extra:
  tags:
    - HTML
description: "Recently I did some research on the current best practices for the contents of the head tag of an HTML document. Below you can find a detailed breakdown of how it should be structured."
---

Recently I did some research on the current best practices for the contents of the [`head`](https://developer.mozilla.org/en-US/docs/Glossary/Head) tag of an HTML document. Below you can find a detailed breakdown of how it should be structured.

<!-- more -->

## Essential Tags

The tags here are the ones every website should have in its `<head>`, even if you do not plan to optimize it for search engines or social media:

```html
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>About | Rilling.dev</title>

<meta name="author" content="F. Rilling" />
<meta
	name="description"
	content="Hi, I'm a software developer from Germany. My main areas of interest are frontend and backend web application development."
/>
```

Let us break them down:

meta: charset
: The [character encoding of this document](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#charset). Note that unlike most tags listed in this article, this one _must_ appear as early as possible ([in the first 1024 bytes](https://html.spec.whatwg.org/multipage/semantics.html#charset)). You pretty much always want to use `utf-8` as the value.

meta: viewport
: ["viewport"](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name#meta_names_defined_in_other_specifications) tells the browser how to scale the page for a given viewport. `width=device-width, initial-scale=1` tells the browser that the document should fit the screen width and starts with a scale of one, meaning no zoom.

title
: The [page title](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title). A short, meaningful text, which will be displayed in browser tabs or search results. On a side note, make sure to HTML-escape the content of the title if you use user-generated data for titles.

meta: description
: What the page is about. The description should be [somewhere between 150 and 160 characters](https://moz.com/learn/seo/meta-description).

meta: author
: The name of the person or organization that created the content of the page.

Note that you might have come across the meta name `keywords`, but this type is [ignored by modern crawlers](https://webmasters.googleblog.com/2009/09/google-does-not-use-keywords-meta-tag.html) and is not needed nowadays.

## CSS & JavaScript

Make sure to analyze the order of your CSS and JavaScript when including them. For example, some stylesheets might be more important to the general page layout than others and should be loaded sooner.

A lot of JavaScript code is not required to be in the head, and placing it near the end of the body allows browsers to render the page before parsing all scripts. You can also use the [`async` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attributes) to achieve a similar effect.

### Note on Content Delivery Networks

While Content Delivery Networks (CDN) can speed up pages by transferring larger resources from a faster server, their effectiveness has been reduced due to recent changes in browsers to improve privacy, [most notably cache partitioning](https://www.zdnet.com/article/firefox-to-ship-network-partitioning-as-a-new-anti-tracking-defense/).

## Prefetching Resources

Using [prefetching `link` tags](https://developer.mozilla.org/en-US/docs/Glossary/Prefetch) allows you to tell the browser which resources might be needed soon. The browser can then load them early which will make them instantly available once they are needed. For this website, I tell the browser to prefetch some icons I only use on some pages:

```html
<link rel="prefetch" href="/sprites/font-awesome-brands.svg" />
<link rel="prefetch" href="/sprites/font-awesome-solid.svg" />
```

## Social Media Tags

Facebook and Twitter both have their own approach to custom metadata that allows specifying how your site should be displayed in their feeds: Facebook uses [OpenGraph](https://ogp.me/), while Twitter has Twitter Cards. The following will only explain OpenGraph, as Twitter is a crap site.

Note that, besides the head tags, OpenGraph also needs an attribute on the HTML element:

```html
<html lang="en" dir="ltr" prefix="og: https://ogp.me/ns#">
	<head>
		<!-- Essential tags omitted -->

		<!-- OpenGraph -->
		<meta property="og:type" content="website" />
		<meta property="og:title" content="About" />
		<meta property="og:url" content="https://rilling.dev/about/" />
		<meta property="og:site_name" content="Rilling.dev" />
		<meta
			property="og:description"
			content="I&#39;m a Software Developer from Germany."
		/>
		<meta property="og:locale" content="en_US" />
		<meta
			property="og:image"
			content="https://rilling.dev/apple-touch-icon.png"
		/>
		<meta
			property="article:published_time"
			content="2018-12-31T23:00:00.000Z"
		/>
		<meta
			property="article:modified_time"
			content="2021-06-29T16:01:43.018Z"
		/>
		<meta property="article:author" content="F. Rilling" />
		<meta
			property="article:tag"
			content="web development, web application development, programming, blog"
		/>
	</head>
	<body></body>
</html>
```

The [OpenGraph website](https://ogp.me/) contains a list of all properties and page types that are available. Facebook has [a tool for debugging and testing this metadata](https://developers.facebook.com/tools/debug/sharing/) (Requires a Facebook account).

### Color Scheme

You can tell browsers if your page supports dark and/or light color schemes using the following:

```html
<meta name="color-scheme" content="dark light" />
```

See [`color-scheme` in the MDN metadata documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name#meta_names_defined_in_other_specifications) for details.

Note that instead of this, you can also use [the corresponding CSS property](https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme).

## Icons

[Andrey Sitnik](https://github.com/ai) has a good article on this: [How to Favicon in 2021](https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs). I recommend [realfavicongenerator.net](https://realfavicongenerator.net/) for the generation of the image files themselves.

These are the icons I use for this site:

```html
<link rel="icon" href="/favicon.ico" sizes="any" />
<link rel="icon" href="/icon.svg" type="image/svg+xml" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" />
<meta name="theme-color" content="#222222" />
```

## SEO

The following tags are relevant when optimizing for search engines ([SEO](https://en.wikipedia.org/wiki/Search_engine_optimization)):

```html
<meta name="robots" content="index,follow" />

<link rel="canonical" href="https://rilling.dev/about/" />
```

[`robots`](https://www.robotstxt.org/meta.html) tells search engine crawling bots how to interact with your page. `index,follow` tells the bot to both index the current page and follow all links and crawl those.

The [`<link rel="canonical"/>`](https://developer.mozilla.org/en-US/docs/Web/URI/Authority/Choosing_between_www_and_non-www_URLs#using_link_relcanonical) tag specifies which URL should be used if more than one exists for the current page. For example, a page that has both a "www", and a "non-www" URL can use this to tell crawlers which version to use. You only need this when you provide multiple URLs for the same content **while not using redirects for that**, so I do not use it. See [Yoast](https://yoast.com/rel-canonical/) for details.

## Analytics

**Make sure you are aware of the [privacy problems of analytic tools like Google Analytics](https://en.wikipedia.org/wiki/Google_Analytics#Privacy).**

Analytics tools like Google Analytics usually work by embedding a JavaScript snippet somewhere inside the head. Consult the documentation of the analytics tool you use for details.

## Additional Resources

- The HTML5 Boilerplate Project has [documentation on the basic HTML head of a document.](https://github.com/h5bp/html5-boilerplate/blob/main/docs/html.md)
- [The 'HEAD' Project](https://github.com/joshbuchea/HEAD) has an exhaustive list of available tags and attributes that could be used in the head.
- Harry Roberts has a [great talk on head items and performance.](https://speakerdeck.com/csswizardry/get-your-head-straight)
