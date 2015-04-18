---
layout: post
title: PageSpeed Insights Scoring for Jekyll Github Pages
---

This blog is built on Jekyll/Hyde and is hosted on Github Pages. With the upcoming scoring change on Google Search 
results for mobile devices I was curious about the score for my blog.

As it turns out, the initial score wasn't too bad.  

|Mobile|Desktop|
|------|-------|
|Speed: 77/100|Overall: 90/100|
|User Experience: 97/100|     |

But I was after perfection.

The issues were centered on
*** Render-blocking JavaScript  

This was easy enough to fix.  In my case I had the default embed code for a Typekit font. As it happens they 
also provide an asynchronous code snippet to load their fonts. For more info refer to Typekit's 
support article on (Embed Code)[http://help.typekit.com/customer/portal/articles/649336].

I didn't bother with controlling FOUT (the Flash of Unstyled Text), but if you're inclined to do so, they have more 
tips (here)[http://help.typekit.com/customer/portal/articles/6852]

*** Optimize CSS delivery

The fix for this was directly from Google's 'how to fix' guidelines on (Optimizing CSS Delivery)[https://developers
.google.com/speed/docs/insights/OptimizeCSSDelivery].  I also contactonated my CSS files (hyde.css, poole
.css, style.css and syntax.css) in to one minified file `styles.min.css`. Then loaded it asynchronously just before
 the closing `<body>` tag.

{% highlight html %}
    <script>
      var cb = function() {
        var l = document.createElement('link'); l.rel = 'stylesheet';
        l.href = 'styles.min.css';
        var h = document.getElementsByTagName('head')[0]; h.parentNode.insertBefore(l, h);
      };
      var raf = requestAnimationFrame || mozRequestAnimationFrame ||
          webkitRequestAnimationFrame || msRequestAnimationFrame;
      if (raf) raf(cb);
      else window.addEventListener('load', cb);
    </script>
{% endhighlight %}

While this fixed the original issue (optimizing css delivery) it introduced another one.  PageSpeed Insights does its
 analysis *before* the asynchronously loaded css is loaded. So, it reported several 'user experience' issues related 
 to not having the CSS loaded.  The issues it reported were clearly fixed in the live site, but I was after the 
 perfect score. So I ended up with an ugly hack by inlining a few CSS snippets within the page template `\_includes\head
 .html` so that it's picked up by Pagespeed Insights.
 
*** Minifying CSS

I had four CSS files that I concatanated and then minified using the (YUI Compressor)
[https://yui.github.io/yuicompressor/]

*** Sizing Tap Targets Appropriately

This was related to the page links on the sidebar. The default template doesn't provide enough padding between the 
links. So adding some padding sorted it out.  

{% highlight css %}
.sidebar a {
    color: #fff;
    padding: 5px 0;
}
{% endhighlight %}

*** Leverage browser caching

This was the only issue I couldn't fix.  My site is hosted on Github Pages, and it's not possible to configure 
browser cacheing.

*** Final results


