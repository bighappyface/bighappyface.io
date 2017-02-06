## AMPing things up

I have been fascinated by Google's AMP project and the results being achieved with focused optimization of web page delivery, especially for mobile. I believe the constraints imposed by AMP are great and to be embraced. There is no better way to demonstrate that than to adopt AMP for my site, and this post covers the conversion process of this site to valid AMP HTML.

### AMP

https://www.ampproject.org/

### Implementation

Adapting this site was not difficult. A great deal of the site was ready to go by simply [implementing the boilerplate](https://www.ampproject.org/docs/get_started/create/basic_markup).

The homepage had eleven errors:

```shell
The attribute 'href' in tag 'link rel=stylesheet for fonts' is set to the invalid value '/css/main.css'. (see https://www.ampproject.org/docs/reference/spec.html#custom-fonts)
The attribute 'href' in tag 'link rel=stylesheet for fonts' is set to the invalid value '//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css'. (see https://www.ampproject.org/docs/reference/spec.html#custom-fonts)
The attribute 'href' in tag 'link rel=stylesheet for fonts' is set to the invalid value '//fonts.googleapis.com/css?family=Lora:400,700,400italic,700italic'. (see https://www.ampproject.org/docs/reference/spec.html#custom-fonts)
The attribute 'href' in tag 'link rel=stylesheet for fonts' is set to the invalid value '//fonts.googleapis.com/css?family=Open+Sans:300italic,400italic,600italic,700italic,800italic,400,300,600,700,800'. (see https://www.ampproject.org/docs/reference/spec.html#custom-fonts)
The tag 'iframe' is disallowed.
The tag 'script' is disallowed except in specific forms.
The attribute 'style' may not appear in tag 'header'.
The attribute 'style' may not appear in tag 'div'.
The tag 'script' is disallowed except in specific forms.
The tag 'script' is disallowed except in specific forms.
The tag 'script' is disallowed except in specific forms.
```

The console validation errors are awesome. Using `#development=1` couldn't be easier, and the error messages even have links to solutions for those validation errors.

### The corrections

1. HTTPS and absolute URLs for external resources allowed by AMP. This solved three validation errors for the fonts.

2. A new GTM container configured for AMP pages. This solved the `iframe` error and I would have needed to do it anyway for AMP-compliant GA tracking. This change dropped two errors.

3. Scripts in the body must be `async`. I was fortunate enough to not need the various scripts I was loading so I removed them to silence three more errors.

4. Inline styles. I used them for dynamic background images but I pivoted to the inline `<style amp-custom>` element per-post to resolve two more errors.

5. The stylesheets. I used a custom script developed by my friend Brady Vitrano to pull all computed styles from a page, then I trimmed down all of the stuff from Bootstrap that I was certain I am not using. That got me below the 50KB limit for `<style amp-custom>` and with removing all `!important` directives that resolved the final validation error.

The aforementioned JS for harvesting computed CSS: https://gist.github.com/bighappyface/22b0a04293354502645ec624a6a28a59

**Tip:** If you harvest the computed styles, do this BEFORE you add the AMP elements so you do not pull in their styles as well.

### Results

WebPageTest before: https://www.webpagetest.org/result/170206_FH_TY/

WebPageTest after: https://www.webpagetest.org/result/170206_V1_31X/

While I also pushed in some resource hints and optimized the awesomeface image to webp, the speed index and page download speed improvements are modest but solid. About 20% improvement in fully-loaded time and about 17% improvement in speed index.

### Final thoughts

Overall, I am pleased. I dropped some requests, a ton of CSS, all custom JS, and learned about AMP.
