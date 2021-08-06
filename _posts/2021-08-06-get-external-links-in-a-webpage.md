---
title:  "Get external links in a webpage"
date:   2021-08-06 09:28:00 +02
---

I was about to answer a question on Stackoverflow, but then the question got deleted.
So here I my answer.

The question was: How can I get a list of external links on a webpage?

My suggestion is to use the build-in list of links on `document` and then filter that on the origin of the current URL:

A simple HTML page:

```html
<ul>
  <li><a href="/about">About</a></li>
  <li><a href="https://www.google.com">Google</a></li>
</ul>

```

The JavaScript:

```javascript
var url = new URL(document.baseURI);
var origin = url.origin;
var external = [...document.links].filter(a => !a.href.startsWith(origin));

console.log(external);

```

Here I create a URL object based on the current baseURI of the document.
Then I get the origin so that I can compare that with all the links.
There can be different ways of writing URLs in the href attribute, but the href property will always return the *absolute* path that starts off with the origin.
So, by testing if a href starts with the origin you can see if it is an internal or external link.