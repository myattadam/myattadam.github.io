# The Best Way to Embed SVG on HTML (2020)

You've probably came across various ways to embed SVG in HTML, but with the latest browser and technology updates, do we still need an `` tag or can we use `` instead? What are their pros and cons and how do they compare?

- 2018-12-27: Updated for 2019, img tag is still the best way, after compression with [Nano](https://vecta.io/nano).
- 2018-08-29: Updated inlining with ability to index text and select them.

Use ``<img>``  tag and embed fonts using [Nano](https://vecta.io/nano).

If you are embedding an SVG onto your website in this day and age, chances are, you have a lot more factors to consider, in addition to browser compatibility. Before we do a comparison, let's have a look at what factors we are considering, when making a decision:

## Design considerations

### Alt and title attribute availability

When we embed an image onto HTML, it's always good to have `alt` and `title` attributes, for better accessibility. An `alt` attribute allows a user to view the image description, even when it fails to render on a device, in addition to providing context for search engines. A `title` attribute allows hovering over the image to obtain more information. Ideally, whatever way you choose to embed an SVG, you should have `alt` and `title` attributes.

### Browser caching

We all know page speed will be a ranking factor on search engines, especially on mobile sites. It pays to enable browser caching for all static assets including SVG images. Ideally the embedding method will allow easy browser caching and [cache busting](https://www.alainschlesser.com/bust-cache-content-hash/).

### GZip Compression



Unlike PNG and JPEG formats (practically, already compressed), SVG images are very well suited for compression. According to the [World Wide Web Consortium](https://www.w3.org/TR/SVG11/minimize.html), SVG offers anything between **75% - 85%** compression ratio, reducing a **16KB** file to only **4KB**, resulting in considerable bandwidth and speed savings.

### Interactivity

Because of [security reasons](https://blog.guya.net/2014/02/17/svg-for-fun-and-phishing/), some SVG embedding methods will block access to external resources including CSS, fonts and javascript. Especially when we have multiple images, ideally our embedding method should be able to refer to a single CSS, font or javascript file (to save resources) and be able to manipulate our embedded SVG.

### Search engine indexing

Google has [publicly stated](https://searchengineland.com/google-search-now-indexes-crawls-svg-files-49695) that it will index and crawl SVG files. Therefore, it makes sense for better SEO (Search Engine Optimization), that the embedding method we adopt would allow search engines to list our images on image search.

### Streamlined work flow

The embedding method we choose should result in an easy and streamlined workflow, that do not require extra work with maintaining complicated CSS and embedding methods.

## 1. Using an `<img>` tag

The best and the simplest way to embed SVG into HTML is to use the `` tag. It has a syntax that is similar to how we embed other image formats like PNG, JPEG and GIF:

```
<img src="image.svg" />
```

| Browser support          | The `<img>` tag is now supported across all major browsers that support SVG (IE9+). |
| ------------------------ | ------------------------------------------------------------ |
| Alt and title attributes | Both available.                                              |
| Browser caching          | Available.                                                   |
| GZip compression         | Available.                                                   |
| Interactivity            | None. If interactivity is required, use `` tag.              |
| Search engine indexing   | Available.                                                   |
| Workflow                 | Streamlined, as with other image formats.                    |

### Losing fonts

If you use an `<img>` tag with web fonts, the fonts will fail to render and resort to using only system fonts.

![Losing fonts when using SVG with img tag](losing-fonts.b84b3c1ff1.svg)

This is mainly because images with `<img>` tags are not allowed to refer to external resources including CSS, fonts and scripts, for [security reasons](https://blog.guya.net/2014/02/17/svg-for-fun-and-phishing/).

You can manually embed fonts into your SVG to resolve this, but most times, will result in large SVG size, and therefore negating the advantages SVG has, over other image formats.

The exception being, to use [vecta.io](https://vecta.io/) or [vecta.io/nano](https://vecta.io/nano) to produce an SVG that is very low in size, self contained without losing any fonts, and reduced complexity in your workflow. They not only minify the SVG but also minify the fonts as well, resulting in a very small SVG (22% smaller than competition), comparable to PNG @1X resolution, saving you a ton of bandwidth and makes your site load faster.

![No loss of fonts if embedded](embed-fonts.abc7a3102c.svg)

Embedding fonts is just a matter of drag and drop, and results in impressive quality without sacrificing file sizes.

- [Making SVG easier to use (Why we build Nano)](https://vecta.io/blog/making-svg-easier-to-use/)
- [Comparing SVG and PNG file sizes](https://vecta.io/blog/comparing-svg-and-png-file-sizes/)

## 2. Using an `<object>` tag

If you require interactivity, this is your best option:

```
<object type="image/svg+xml" data="image.svg"></object>
```

| Browser support          | The `<object>` tag is supported across all major browsers that support SVG (IE9+). |
| ------------------------ | ------------------------------------------------------------ |
| Alt and title attributes | None.                                                        |
| Browser caching          | Available.                                                   |
| GZip Compression         | Available.                                                   |
| Interactivity            | Available.                                                   |
| Search engine indexing   | None. Workaround available, but with compromises.            |
| Workflow                 | Medium. Requires extra work in referring to external CSS and font files. |

Technically, `<object>` tags can be used on many elements, including SVG files, and therefore not recognized as image elements, thus not available on image search. The workaround is to use an `<object>` tag as fallback:

```
<object type="image/svg+xml" data="image.svg">
    <!-- Your fall back here -->
    <img src="image.svg" />
</object>
```

### Double loading

The only problem is double loading, that is the browser will load the image on the `<object>` tag, and another image on the `<object>` tag, even though only one of them is required, while the other is hidden. If you cache your images, and mark these files as cacheable on your server, at least, the browser will load these images from the cache, but still, there is no stopping the double loading.

## 3. Using inline SVG in HTML5

Essentially you are embedding all your SVG codes inside your HTML:

```
<body> 
   <svg xmlns="http://www.w3.org/2000/svg">
       <text x="10" y="50" font-size="30">My SVG</text>
   </svg> 
</body>
```

| Browser support          | Supported across all major browsers that support SVG (IE9+). |
| ------------------------ | ------------------------------------------------------------ |
| Alt and title attributes | None.                                                        |
| Browser caching          | None.                                                        |
| GZip compression         | None.                                                        |
| Interactivity            | Available.                                                   |
| Search engine indexing   | None.                                                        |
| Workflow                 | Convoluted.                                                  |

Because inline SVG is embedded into HTML, there is no necessity for another network request to obtain the SVG file, and therefore inline SVG will load the fastest.

Be aware that you will still get [FOUC (Flash of unstyled content)](https://en.wikipedia.org/wiki/Flash_of_unstyled_content) because chances are, your inline SVG will still refer to an external font. To solve FOUC, you will require embedded fonts as listed in [Using an  tag](https://vecta.io/blog/best-way-to-embed-svg#1-Using-an-lt-img-gt-tag).

Since the SVG is essentially the DOM, you can easily use external CSS, fonts and scripts. Multiple SVG can be inlined that refers to a single CSS or font files, therefore saving bandwidth and resources.

In addition, you get the ability to select, highlight and copy text in your SVG, plus the ability for search engines to index these text (Our opinion is that one should rely on HTML for SEO, and leave SVG separated, so we can index text using HTML and index images using SVG).

Workflow is way more complicated since SVG is mixed with HTML, and requires a lot more work maintaining dependencies.

## 4. Using an `<embed>` tag

Listed here for completeness, but do NOT use `<embed>` tag. It is not part of HTML specifications but widely supported on all browsers mainly used to implement Flash plugins.

```
<embed type="image/svg+xml" src="image.svg" />
```

## 5. Using an `<iframe>` tag

Do NOT use an `<iframe>` where you can use an `<object>` tag instead. Iframes are difficult to maintain, does not get indexed by search engines and bad for SEO (Search Engine Optimization).

```
<iframe src="image.svg"></iframe>
```

## 6. Using a CSS background image

This is equivalent to using the `<img>` tag, and subjected to the same advantages and disadvantages.

```
#id {
  background-image: url(image.svg);
}
```

## Comparison

|                         | `<img>`          | `<object>`               | Inline SVG |
| :---------------------- | :---------- | :--------------- | :--------- |
| Browser support         | Good        | Good             | Good       |
| Alt and title attribute | Yes         | None             | Title only |
| Browser caching         | Yes         | Yes              | None       |
| GZip compression        | Yes         | Yes              | None       |
| Interactivity           | None        | Yes              | Very good  |
| Search engine indexing  | Yes         | Through fallback | None       |
| Workflow                | Streamlined | Medium           | Convoluted |
| Loading speed           | Fast        | Slower           | Very fast  |

## Conclusion

As can be seen from comparison, it is clear we are recommending `<img>` tag for most use cases. The only exception is if you need interactivity, where you require dynamic changes to your SVG based on user interactions.

We do not recommend inline SVG for most cases, with the only exception being preloading pages. Preloading pages are contents shown when your application has yet to completely load, and since inline SVG shows almost immediately, it can be used to show images or graphics while waiting for your application to load.