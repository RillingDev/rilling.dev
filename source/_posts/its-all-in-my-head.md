---
title: "It's All in My <head>"
date: 2016-12-13
updated: 2021-07-03
tags:
    - HTML
    - SEO
    - Tags
---

Recently I did some research on what the current recommendations for the contents of the [head](https://developer.mozilla.org/en-US/docs/Glossary/Head) tag of an HTML document are. Below you can find a detailed breakdown on how the different sections can or should be structured.

<!-- more -->

## Essential tags:

The main tags here are the ones every website should have, even if you do not plan to optimize it for search engines or social media:

```html
<meta charset="utf-8" />

<title>About | Felix Rilling</title>

<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="robots" content="index,follow" />

<meta name="author" content="Felix Rilling" />
<meta
    name="description"
    content="Hi, I&#39;m Felix. I&#39;m a Software Developer from Germany."
/>
```

Let's break them down:

-   **meta:charset:**
    The [character encoding of this document](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#attr-charset). Note that unlike most tags listed in this article, this one _must_ appear as early as possible ([in the first 1024 bytes](https://html.spec.whatwg.org/multipage/semantics.html#charset)). The value `utf-8` is the value you want to pretty much always use.

-   **title:**
    The [page title](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title). A short, meaningful text that will be displayed in browser tabs or search results. On a side note, make sure to HTML escape the content of the title if you use user generated data for titles.

-   **meta: viewport:**
    ["viewport"](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name#standard_metadata_names_defined_in_other_specifications) tells the browser how to scale the page for a given viewport. `width=device-width, initial-scale=1` tells the browser that the document should fit the screen width and starts with a scale of 1, meaning no zoom.

-   **meta: robots:**
    The ["robots"](https://www.robotstxt.org/meta.html) meta property tells search engine crawling bots how to interact with your page. `index,follow` tells the bot to both index the current page and follow all links and crawl those.

-   **meta: description:**
    What the page is about. The description should be [somewhere between 150 and 160 characters](https://moz.com/learn/seo/meta-description).

-   **meta: author:**
    The name of the person or organization that created the content of the page.

Note that you might have come across the meta type "keywords", but this type is [ignored by modern crawlers](https://webmasters.googleblog.com/2009/09/google-does-not-use-keywords-meta-tag.html) and is of no value nowadays.

## Social media tags:

These two huge social networks both have their own approach to custom head tags that allow specifying how your site should be displayed in their feeds: Facebook uses [OpenGraph](https://ogp.me/), while Twitter has their [Twitter Cards](https://dev.twitter.com/cards/overview).

### OpenGraph

Note that, in addition to the head tags, OpenGraph also needs an attribute on the HTML element:

```html
<html lang="en" dir="ltr" prefix="og: https://ogp.me/ns#">
    <head>
        <!-- Essential tags -->

        <!-- OpenGraph -->
        <meta property="og:type" content="website" />
        <meta property="og:title" content="About" />
        <meta property="og:url" content="https://rilling.dev/about/" />
        <meta property="og:site_name" content="Felix Rilling" />
        <meta
            property="og:description"
            content="Hi, I&#39;m Felix. I&#39;m a Software Developer from Germany."
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
        <meta property="article:author" content="Felix Rilling" />
        <meta
            property="article:tag"
            content="web development, web application development, programming, blog"
        />
    </head>
    <body></body>
</html>
```

The [OpenGraph website](https://ogp.me/) contains a list of all properties and page types that are available. Facebook has a tool for debugging and testing [here](https://developers.facebook.com/tools/debug/sharing/) (Requires a Facebook account).

### Twitter Cards:

Twitter has a [documentation page for Twitter cards on their developer platform page](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards):

```html
<meta name="twitter:card" content="summary" />
<meta name="twitter:image" content="https://rilling.dev/apple-touch-icon.png" />
```

Twitter cards can be tested with [Twitters validator](https://cards-dev.twitter.com/validator).

## Icons:

I can _really_ recommend [realfavicongenerator.net](https://realfavicongenerator.net/) here, a pretty awesome tool to generate the images and markup for your websites icon. Here is the markup I use for my page:

```html
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#c95497" />
<link rel="shortcut icon" href="/favicon.ico" />
<meta name="theme-color" content="#222222" />
```

To see what all of these are used for, see the documentation on [realfavicongenerator.net](https://realfavicongenerator.net/).

### Color Scheme:

You can tell browsers if your page supports dark and/or light color schemes (like this one) using the following:

```html
<meta name="color-scheme" content="dark light" />
```

See ['color-scheme' in the MDN metadata documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name#standard_metadata_names_defined_in_other_specifications) for details.

## Canonical:

The **link: canonical** tag specifies which URL should be used if more than one exists. For example, a page that has both a "www", and a "non-www" URL can make use of this to avoid splitting traffic on the two URLs which makes analytics clearer. You only need this when you provide multiple URLs for the same content **while not using redirects for that**, so I personally do not use it. See [Yoast](https://yoast.com/rel-canonical/) for details.

```html
<link rel="canonical" href="https://rilling.dev/about/" />
```

## Analytics:

**Make sure you are aware of the [privacy problems of analytics like Google Analytics](https://en.wikipedia.org/wiki/Google_Analytics#Privacy).**

Note that this website does not use Google Analytics.

When using [Google Analytics](https://analytics.google.com/analytics/web/) it is recommended to insert the [minified tracking script](https://developers.google.com/analytics/devguides/collection/analyticsjs/tracking-snippet-reference) in the head tag:

```html
<script>
    (function (i, s, o, g, r, a, m) {
        i["GoogleAnalyticsObject"] = r;
        (i[r] =
            i[r] ||
            function () {
                (i[r].q = i[r].q || []).push(arguments);
            }),
            (i[r].l = 1 * new Date());
        (a = s.createElement(o)), (m = s.getElementsByTagName(o)[0]);
        a.async = 1;
        a.src = g;
        m.parentNode.insertBefore(a, m);
    })(
        window,
        document,
        "script",
        "https://www.google-analytics.com/analytics.js",
        "ga"
    );

    ga("create", "UA-XXXXX-Y", "auto");
    ga("send", "pageview");
</script>
```

[Some more information on settings up Google Analytics can be found here](https://www.websiteplanet.com/blog/ultimate-beginners-guide-google-analytics/) (Thanks to Emma for pointing this out).

Other analytic tools usually operate similarly, but with different JavaScript snippets.

## CSS/JavaScript:

When including CSS and JavaScript you should be aware of the basic optimizations to do:

-   Make sure to bundle/minify them to save several milliseconds or even seconds of page load time.
-   Make sure to analyse the required order. Some Stylesheets might be more important to the general page layout than others, and should be loaded sooner.
-   A lot of JavaScript code is not required to be in the head - placing it near the end of the body allows browsers to render the page before parsing all scripts.

### Note on Content Delivery Networks

While Content Delivery Networks (CDN) can speed up pages by transferring larger resources from a faster server, their effectiveness has been reduced due to recent changes in browsers to improve privacy, [most notably cache partitioning](https://www.zdnet.com/article/firefox-to-ship-network-partitioning-as-a-new-anti-tracking-defense/).

## Prefetching Resources

Using [prefetching link tags](https://developer.mozilla.org/en-US/docs/Glossary/Prefetch) allows you to tell the browser which resources might be needed in the near future. The browser might then load them early which will make them instantly available once they are needed. In my case I tell the browser to prefetch some icons I only use on some pages:

```html
<link rel="prefetch" href="/sprites/font-awesome-brands.svg" />
<link rel="prefetch" href="/sprites/font-awesome-solid.svg" />
```

## Further Resources

-   [The HTML5 Boilerplate Project](https://github.com/h5bp/html5-boilerplate/blob/master/dist/doc/html.md) has a good documentation regarding the basic HTML structure of a document, including the head.
-   [The 'HEAD' Project](https://github.com/joshbuchea/HEAD) has an exhaustive list of available tags and attributes that could be used in the head.
