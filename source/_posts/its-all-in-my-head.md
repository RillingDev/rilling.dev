---
title: "It's all in my <head>"
icon: heading
date: 2016/12/13
updated: 2019/06/13
tags:
    - HTML
    - SEO
    - Tags
---

Recently I did some research on what exactly the current consensus for the contents of the [head](https://developer.mozilla.org/en-US/docs/Glossary/Head) of an HTML document is; Below you can find a detailed breakdown on how the different sections can or should be built.

<!-- more -->

## Sections:

### Essential tags:

The main tags here are the ones every website should have, no matter if you plan to optimize for search engines or social media:

```html
<meta charset="utf-8" />

<title>NPM&#39;s Chaos and Possible Solutions | Rilling.dev</title>

<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="robots" content="index,follow" />

<meta
    name="description"
    content="While the JavaScript ecosystem is often criticized for being too complex and/or having way too many dependencies and tools (and rightfully so most of the time), we should keep in mind that most of the"
/>
<meta name="keywords" content="JavaScript,Workflow,Development" />
<meta name="author" content="Felix Rilling" />
```

The order of the tags follows the scheme:

1. General information about the page (in this case just the charset).
2. Page title.
3. Technical information about the page, both for browsers and crawlers.
4. Information about the page content, both for browsers and crawlers.

Let's break them down:

-   **meta:charset:** The ["charset"](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#attr-charset) specifies the charset to use for the website, this almost always should be set to `utf-8`.

-   **meta:viewport:** "viewport" tells the browser how to scale the page and treat different screen sizes. `width=device-width, initial-scale=1` tells the browser that the document should fit the screen width and starts with a scale of 1, meaning no zoom. You could disable zooming entirely using this attribute, but that is usually considered bad practice in terms of accessibility and should rarely be done.

-   **meta:robots:** The ["robots"](http://www.robotstxt.org/meta.html) meta property tells search engine crawling bots how to interact with your page. `index,follow` tells the bot to both index the current page and follow all links and crawl those.

-   **title:** title is..., well, the title of the page. On a side note, make sure to escape the content of the title if you use user generated data for titles.

-   **meta:description:** What the page is about. The description should be [somewhere between 150 and 160 characters](https://moz.com/learn/seo/meta-description).

-   **meta:keywords:** Comma separated tags that describe the page and its contents.

-   **meta:author:** The name of the person or organization that created the content of the page.

### Social media tags:

The two biggest social networks both have their own approach to custom head tags that allow specifying how your site should be displayed in their feeds: Facebook uses [OpenGraph](http://ogp.me/), while Twitter has their [Twitter Cards](https://dev.twitter.com/cards/overview).

#### OpenGraph

In addition to the head tags, OpenGraph also needs attributes on the HTML and head element, depending on the content type:

```html
<html prefix="og: http://ogp.me/ns#" lang="en">
    <head>
        <!-- Other head content -->

        <!--OpenGraph for Facebook-->
        <meta property="og:type" content="article" />
        <meta
            property="og:title"
            content="NPM&#39;s Chaos and Possible Solutions"
        />
        <meta
            property="og:url"
            content="https:&#x2F;&#x2F;rilling.dev&#x2F;npms-chaos-and-possible-solutions&#x2F;index.html"
        />
        <meta property="og:site_name" content="Rilling.dev" />
        <meta
            property="og:description"
            content="By now there are probably more articles about the flaws of NPM and the NPM registry than articles about how Java will die soon, but I feel like there haven’t been many of them which suggest solutions"
        />
        <meta property="og:locale" content="en" />
        <meta
            property="og:image"
            content="https:&#x2F;&#x2F;rilling.dev&#x2F;apple-touch-icon.png"
        />
        <meta property="og:updated_time" content="2019-07-11T22:00:00.000Z" />
    </head>
    <body></body>
</html>
```

The [OpenGraph website](http://ogp.me/) contains a list of all properties and page types that are available. Facebook has a pretty neat tool for debugging and testing [here](https://developers.facebook.com/tools/debug/sharing/).

#### Twitter Cards:

The most common card type are [summary cards](https://dev.twitter.com/cards/types/summary):

```html
<!--Card Data for Twitter-->
<meta name="twitter:card" content="summary" />
<meta
    name="twitter:image"
    content="https:&#x2F;&#x2F;rilling.dev&#x2F;apple-touch-icon.png"
/>
<meta name="twitter:creator" content="@FelixRilling" />
```

Twitter cards can be tested with [Twitters Validator](https://cards-dev.twitter.com/validator).

### Icons:

I can _really_ recommend [realfavicongenerator.net](http://realfavicongenerator.net/) here, a pretty awesome tool to generate the images and markup for your websites icon. Here is the markup I use for my page:

```html
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
<link rel="manifest" href="/site.webmanifest" />
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#83d356" />
<link rel="shortcut icon" href="/favicon.ico" />
<meta name="msapplication-TileColor" content="#83d356" />
<meta name="theme-color" content="#83d356" />
```

### Canonical:

The **link:canonical** tag specifies which URL should be used if more than one exists. For example, a page that has both a "www" and a "non-www" URL can make use of this to avoid splitting traffic on the two URLs which makes analytics clearer. You only need this when you provide multiple URLs for the same content **while not using redirects for that**, so I personally do not use it. See <https://yoast.com/rel-canonical/> for details. 

```html
<link rel="canonical" href="https://rilling.dev" />
```

### Analytics:

**Make sure you are aware of the [privacy problems of analytics like Google Analytics](https://en.wikipedia.org/wiki/Google_Analytics#Privacy).**

When using [Google Analytics](https://analytics.google.com/analytics/web/) it is recommended to insert the minified tracking script in the head tag:

```html
<script>
    (function(w, d, s, l, i) {
        w[l] = w[l] || [];
        w[l].push({ "gtm.start": new Date().getTime(), event: "gtm.js" });
        var f = d.getElementsByTagName(s)[0],
            j = d.createElement(s),
            dl = l != "dataLayer" ? "&l=" + l : "";
        j.async = true;
        j.src = "https://www.googletagmanager.com/gtm.js?id=" + i + dl;
        f.parentNode.insertBefore(j, f);
    })(window, document, "script", "dataLayer", "#Tracking-ID#");
</script>
```

### CSS/JavaScript:

When including CSS and JavaScript you should be aware of the basic optimizations to do:

- Serve it from CDNs if possible
    - Can drastically improve speed due to caching or the CDN having faster webservers than your.
    - Removes control over contents from you, posing a certain privacy and security risk.
    - Is not practical for site specific code.

- Otherwise, when hosting it yourself
    - Make sure to bundle/minify it to save several milliseconds or even seconds opf page load time.
    - Make sure to analyse the required order
        - A lot of JavaScript code is not required to be in the head - placing it near the end of the body allows browsers to render the page before parsing all scripts.

## Summary:

Of course not every page needs a "full" header like this, If you just want a running site, the essential section should be enough. But if you want your site to be as fast, accessible and SEO friendly as possible its always a good idea to optimise your head.
