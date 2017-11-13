---
title: 'It''s all in my <head>'
published: true
date: '13-12-2016 10:40'
taxonomy:
    category:
        - HTML
    tag:
        - HTML
        - SEO
        - Tags
visible: true
icon: header
---

Recently I did some research on what exactly the latest standard for the content of the of the head of an HTML document is; Below you can find a detailed breakdown on how the different sections can or should be built.

## Sections:

### Essential tags:

The main tags here are the ones every website should have, no matter if optimized for search engines or social media:

```html
<meta charset="utf-8"/>
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="robots" content="index,follow"/>
<title>Getting into JavaScript Building &amp; Bundling. Part 2: Bundling Tools | F-Rilling.com</title>
<meta name="description" content="This is the second part of my series about JavaScript Bundling &amp; Building. If you haven't read the first Part yet, go check it out, otherwise the conte"/>
<meta name="keywords" content="JavaScript, Modules, Bundling"/>
<meta name="author" content="Felix Rilling"/>
```

The order of the tags follows the scheme:

1. Information about the page for the browser
2. Page title
3. Information about the page content

Let's break them down:

- **meta:charset:** The ["charset"](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#attr-charset) specifies the charset to use for the website, almost always this should be set to `utf-8`

- **meta:http-equiv:** ["http-equiv"](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#attr-http-equiv) can be used to tell user-agents how to handle the page. `IE=edge` will make Internet Explorer use the latest rendering technology to display the site.

- **meta:viewport:** "viewport" tells the browser how to scale the page and treat different screen sizes. `width=device-width, initial-scale=1` tells the browser that the document should fit the screen width and starts with a scale of 1, meaning no zoom. You could disable zooming entirely, but that is a bad practice in terms of accessibility and should not be done.

- **meta:robots:** The ["robots"](http://www.robotstxt.org/meta.html) meta property tells search engine crawling bots how to interact with your page. `index,follow` tells the bot to both index the current page and follow all links and crawl those.

- **title:** title is...well the title of the page. Most of the time the structure of the title is either `#pagetitle#` or `#pagetitle# | #page#`. On a side note, make sure to escape the content of the title if you use user generated data for titles.

- **meta:description:** What the page is about. The description should be [somewhere between 150 and 160 characters](https://moz.com/learn/seo/meta-description).

- **meta:keywords:** Comma separated tags that describe the page.

- **meta:author:** The name of the person or organization that created the content of the page.

### Social media tags:

The two biggest social networks both have their own approach to custom head tags that allow specifying how your site should be displayed in their feeds: Facebook uses [OpenGraph](http://ogp.me/), while Twitter has their [Twitter Cards](https://dev.twitter.com/cards/overview).

#### OpenGraph

In addition to the head tags, OpenGraph also needs attributes on the HTML and head element, depending on the content type:

```html
<html prefix="og: http://ogp.me/ns#">
    <head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# article: http://ogp.me/ns/article#">
        <!-- Head content -->

        <!--Open Graph for Facebook-->
        <meta property="og:title" content="Getting into JavaScript Building &amp; Bundling. Part 2: Bundling Tools | F-Rilling.com"/>
        <meta property="og:url" content="http://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools"/>
        <meta property="og:description" content="This is the second part of my series about JavaScript Bundling &amp; Building. If you haven't read the first Part yet, go check it out, otherwise the content of this part might not make sense to you.Bundling Tool Overview let's start by taking a look at this table."/>
        <meta property="og:type" content="article"/>
        <meta property="og:image" content="http://f-rilling.com/user/themes/frilling2/img/icons/android-chrome-256x256.png"/>
        <meta property="og:site_name" content="F-Rilling.com"/>

        <meta property="og:article:tag" content="JavaScript ,Modules ,Bundling"/>
        <meta property="og:article:author" content="Felix Rilling"/>
        <meta property="og:article:published_time" content="20.07.2016"/>
    </head>
    <body></body>
</html>
```

The [OpenGraph website](http://ogp.me/) contains a list of all properties and page types that are available. Facebook has a pretty neat tool for debugging and testing [here](https://developers.facebook.com/tools/debug/sharing/).

#### Twitter Cards:

The most common card type are [summary cards](https://dev.twitter.com/cards/types/summary):

```html
<!--Card Data for Twitter-->
<meta name="twitter:card" content="summary">
<meta name="twitter:url" content="http://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools">
<meta name="twitter:title" content="Getting into JavaScript Building &amp; Bundling. Part 2: Bundling Tools | F-Rilling.com">
<meta name="twitter:description" content="This is the second part of my series about JavaScript Bundling &amp; Building. If you haven't read the first Part yet, go check it out, otherwise the content of this part might not make sense ">
<meta name="twitter:image" content="http://f-rilling.com/user/themes/frilling2/img/icons/android-chrome-256x256.png"/>
<meta name="twitter:creator" content="@FelixRilling">
```

Twitter cards can be tested with [Twitters Validator](https://cards-dev.twitter.com/validator).

### Icons:

I can REALLY recommend [realfavicongenerator.net](http://realfavicongenerator.net/) here, a pretty awesome tool to generate the images and markup for your websites icon. Here is the markup I use for my page:

```html
<link rel="apple-touch-icon" sizes="180x180" href="http://f-rilling.com/user/themes/frilling2/img/icons/apple-touch-icon.png">
<link rel="icon" type="image/png" href="http://f-rilling.com/user/themes/frilling2/img/icons/favicon-32x32.png" sizes="32x32">
<link rel="icon" type="image/png" href="http://f-rilling.com/user/themes/frilling2/img/icons/favicon-16x16.png" sizes="16x16">
<link rel="manifest" href="http://f-rilling.com/user/themes/frilling2/img/icons/manifest.json">
<link rel="mask-icon" href="http://f-rilling.com/user/themes/frilling2/img/icons/safari-pinned-tab.svg" color="#83d356">
<link rel="shortcut icon" href="http://f-rilling.com/user/themes/frilling2/img/icons/favicon.ico">
<meta name="msapplication-config" content="http://f-rilling.com/user/themes/frilling2/img/icons/browserconfig.xml">
<meta name="theme-color" content="#83d356">
```

### Canonical:

The link:canonical tag specifies which URL should be used if more than one exists. For example, a page that has both a "www" and a "non-www" URL can make use of this to avoid splitting traffic on the two URLs which makes analytics a lot cleaner. For my page, I want "<http://f-rilling.com>" as the default URL, so users who visit "<http://www.f-rilling.com>" get redirected.

```html
<link rel="canonical" href="http://f-rilling.com"/>
```

### Analytics:

When using [Google Analytics](https://analytics.google.com/analytics/web/) it is recommended to insert the minfied tracking script in the head tag:

```html
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','#Tracking-ID#');</script>
```

### CSS:

When including CSS you should be aware of the most basic optimizations to do, depending on the type:

1. Serve it from CDNs if possible

2. Host it yourself

    1. Bundle it

    2. Minify it

### JavaScript:

There is almost never a reason to load JavaScript (besides Analytics) in the head! Moving script tags to the bottom of your body can drastically improve the perceived page loading time. Other than that, the above optimizations all apply to JavaScript too.

## Summary:

Of course not every page needs a complete header like this, If you just want a running site, the essential section should be enough. But if you want your site to be as accessible and social media friendly as possible its always a good idea to optimised your head.

Here is a summary of all the sections above combined:

```html
<!DOCTYPE html>
<html lang="en" prefix="og: http://ogp.me/ns#" itemscope itemtype="http://schema.org/WebPage">
    <head prefix="og:http://ogp.me/ns# fb:http://ogp.me/ns/fb# article:http://ogp.me/ns/article#">
        <meta charset="utf-8"/>
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <meta name="robots" content="index,follow"/>
        <title>Getting into JavaScript Building &amp; Bundling. Part 2: Bundling Tools | F-Rilling.com</title>
        <meta name="description" content="This is the second part of my series about JavaScript Bundling &amp; Building. If you haven't read the first Part yet, go check it out, otherwise the conte"/>
        <meta name="keywords" content="JavaScript, Modules, Bundling"/>
        <meta name="author" content="Felix Rilling"/>

        <!--Open Graph for Facebook-->
        <meta property="og:title" content="Getting into JavaScript Building &amp; Bundling. Part 2: Bundling Tools | F-Rilling.com"/>
        <meta property="og:url" content="http://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools"/>
        <meta property="og:description" content="This is the second part of my series about JavaScript Bundling &amp; Building. If you haven't read the first Part yet, go check it out, otherwise the content of this part might not make sense to you.Bundling Tool Overviewlet's start by taking a look at this table:Feature RequireJS Browser."/>
        <meta property="og:type" content="article"/>
        <meta property="og:image" content="http://f-rilling.com/user/themes/frilling2/img/icons/android-chrome-256x256.png"/>
        <meta property="og:site_name" content="F-Rilling.com"/>
        <meta property="og:article:tag" content="JavaScript ,Modules ,Bundling"/>
        <meta property="og:article:author" content="Felix Rilling"/>
        <meta property="og:article:published_time" content="20.07.2016"/>

        <!--Card Data for Twitter-->
        <meta name="twitter:card" content="summary">
        <meta name="twitter:url" content="http://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools">
        <meta name="twitter:title" content="Getting into JavaScript Building &amp; Bundling. Part 2: Bundling Tools | F-Rilling.com">
        <meta name="twitter:description" content="This is the second part of my series about JavaScript Bundling &amp; Building. If you haven't read the first Part yet, go check it out, otherwise the content of this part might not make sense to ">
        <meta name="twitter:image" content="http://f-rilling.com/user/themes/frilling2/img/icons/android-chrome-256x256.png"/>
        <meta name="twitter:creator" content="@FelixRilling">

        <!--Favicons-->
        <link rel="apple-touch-icon" sizes="180x180" href="http://f-rilling.com/user/themes/frilling2/img/icons/apple-touch-icon.png">
        <link rel="icon" type="image/png" href="http://f-rilling.com/user/themes/frilling2/img/icons/favicon-32x32.png" sizes="32x32">
        <link rel="icon" type="image/png" href="http://f-rilling.com/user/themes/frilling2/img/icons/favicon-16x16.png" sizes="16x16">
        <link rel="manifest" href="http://f-rilling.com/user/themes/frilling2/img/icons/manifest.json">
        <link rel="mask-icon" href="http://f-rilling.com/user/themes/frilling2/img/icons/safari-pinned-tab.svg" color="#83d356">
        <link rel="shortcut icon" href="http://f-rilling.com/user/themes/frilling2/img/icons/favicon.ico">
        <meta name="msapplication-config" content="http://f-rilling.com/user/themes/frilling2/img/icons/browserconfig.xml">
        <meta name="theme-color" content="#83d356">

        <link rel="canonical" href="http://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools"/>

        <!-- Google Tag Manager -->
        <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
        new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
        j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
        'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
        })(window,document,'script','dataLayer','#Tracking-ID#');</script>
        <!-- End Google Tag Manager -->

        <link href="https://fonts.googleapis.com/css?family=Arvo|Lato|Source+Code+Pro" type="text/css" rel="stylesheet"/>
        <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.6.3/css/font-awesome.css" type="text/css" rel="stylesheet"/>
        <link href="/assets/cc796860bc9223d29b62644525f07bee.css" type="text/css" rel="stylesheet"/>

    </head>
    <body></body>
</html>
```
