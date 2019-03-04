[![Build Status](https://travis-ci.org/o10n-x/wordpress-css-optimization.svg?branch=master)](https://travis-ci.org/o10n-x/wordpress-css-optimization) ![Version](https://img.shields.io/github/release/o10n-x/wordpress-css-optimization.svg)

# WordPress CSS Optimization

Advanced CSS optimization toolkit. Critical CSS, minification, concatenation, async loading, advanced editor, CSS Lint, Clean CSS (professional), beautifier and more.

* [Documentation](https://github.com/o10n-x/wordpress-css-optimization/tree/master/docs)
* [Description](https://github.com/o10n-x/wordpress-css-optimization#description)
* [Version history (Changelog)](https://github.com/o10n-x/wordpress-css-optimization/releases)
* [About WordPress WPO Collection](https://github.com/o10n-x/wordpress-css-optimization#wordpress-wpo-collection)

## Installation

![Github Updater](https://github.com/afragen/github-updater/raw/develop/assets/GitHub_Updater_logo_small.png)

This plugin can be installed and updated using [Github Updater](https://github.com/afragen/github-updater) ([installation instructions](https://github.com/afragen/github-updater/wiki/Installation))

<details/>
  <summary>Installation instructions</summary>

### Step 1: Install Github Updater and first optimization plugin

Installing and updating the plugins is possible using Github Updater. It is easy to install one of the plugins. You simply need to download the Github Updater plugin ([zip file](https://github.com/afragen/github-updater/archive/develop.zip)), install it from the WordPress plugin admin panel and copy the Github URL of the plugin into the Github Updater installer.

![image](https://user-images.githubusercontent.com/8843669/39889846-46158cc2-5499-11e8-824d-720020f758db.png)

### Step 2: Install other optimization plugins with a single click

A recent update of all plugins contains a easy single click install button.

![image](https://user-images.githubusercontent.com/8843669/39661507-cc1eac5e-5052-11e8-8fba-33c0cc959b07.png)
</details>

## Beta Notice

This plugin is a **first beta** which was [banned from WordPress.org](https://docs.style.tools/cms-plugins/wordpress/ban) for a yet unknown motive. The plugin never received extensive user feedback which was originally planned and a lot of planned improvements and features were never added. Besides that, the investment by the (now) CEO of PageSpeed.pro Ltd. that was agreed mid 2017 was paid in the beginning of 2018, resulting in a severe delay and a greatly reduced version of the originally planned Advanced Optimization plugin (the bigger plugin was stripped of features and cut into individual plugins, which is actually a good change when looking back today).

Thanks to the valuable feedback from Github users, the plugins did receive feedback. The quality of the plugins is very good however in sense of usability/documentation there are simply unresolved issues.

The plugins are able to deliver results for SEO. Our www.e-scooter.co demo website (started in 2018) ranks in the top 10 in Google in many countries and received visitors from 205 countries and is now visited from 174 countries per week on average. In The Netherlands, www.e-scooter.nl ranks #1 for many scooters. The website is currently at 1.5M visits per year. 

**The WPO plugins are operating sublimely.**

![gogoro-prijs](https://user-images.githubusercontent.com/8843669/48940358-90f40e80-ef17-11e8-8bbb-62c3187d8953.png)

![image](https://user-images.githubusercontent.com/8843669/49431914-c964d980-f7ae-11e8-9a60-f5fb3cfd733d.png)

It will take a bit more effort, and maybe some more feedback from users, but then the WPO plugins could simply be 'perfect' for getting Google Lighthouse 100-scores and even better performance results (speed) than would be required for a 100-score (think of the unique innovations such as timed javascript execution and CSS rendering, advanced Web Worker based preload logic, preload on mouse-down and [Service Worker Push](https://github.com/o10n-x/wordpress-http2-optimization/wiki/HTTP-2-Server-Push-vs-Service-Worker-Push) with better performance than HTTP/2 Server Push).

## Description

This plugin is a toolkit for professional CSS optimization.

The plugin provides in a complete solution for CSS code optimization, CSS delivery optimization (async CSS loading) and Critical CSS management.

### Multiple minifiers and custom minifier

The plugin provides the option to minify CSS code using multiple CSS minifiers including [CSSMin](https://github.com/natxet/CssMin) (PHP), Yahoo's [YUI Compressor PHP Port](https://github.com/tubalmartin/YUI-CSS-compressor-PHP-port), a fast regular expression based CSS minifier and the option to use a custom minifier using a WordPress filter that enables to use any solution, including a Amazon Lambda or Google Cloud function with Node.js based CSS optimization software. 

![CSS Code Optimization](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/css-code-optimization.png)

#### Custom minifier

<details/>
  <summary>Show custom minifier example</summary>

```php
/* Custom CSS minifier */
add_filter('o10n_css_custom_minify', function ($CSS, $base_href) {

    // apply CSS optimization
    exec('/node /path/to/optimize-css.js /tmp/css-source.css');
    $minified = file_get_contents('/tmp/output.css');

    // alternative
    $minified = CSSCompressor::minify($CSS);

    return $minified;

});
```
</details>

### Grouped Concatenation

The plugin provides advanced concatenation with the ability to configure concat groups.

![Group Concat](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/concat-groups.png)

### Async loading and timed download and/or render

The plugin provides many unique innovations including conditional Critical CSS, timed CSS loading and/or rendering based on `requestAnimationFrame` with frame target, `requestIdleCallback`, element scrolled into view or a Media Query.

![Async CSS Optimization](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/async-optimization.png)

The plugin enables to render and unrender stylesheets based on a Media Query or element scrolled in and out of viewport enabling to optimize the CSS for individual devices (e.g. save +100kb of CSS on mobile devices). The plugin makes it possible to enable and disable stylesheets based on the viewport orientation change or element scrolled in or out of view event, making it possible (and easy to manage) to dynamically redesign a website based on events.

### Service Worker Push

The plugin provides a unique innovation called **Service Worker Push**. It is an alternative for HTTP/2 Server Push + Cache-Digest with better performance and better efficiency.

Cache-Digest is not yet an official supported feature. HTTP/2 Server Push without Cache-Digest causes a lot of overhead and has almost no performance advantage.

- Performance study: https://calendar.perfplanet.com/2016/cache-digests-http2-server-push/
- Google engineer: https://jakearchibald.com/2017/h2-push-tougher-than-i-thought/

Cache-Digest calculation for thousands of assets causes overhead. Service Worker Push has direct access to the browser cache storage and is therefor able to support millions of cached assets without performance loss.

Service Worker Push uses PHP method `\O10n\attach_preload()` to attach assets such as images, stylesheets and scripts to a page which are then exposed to a Service Worker using a HTTP header. The Service Worker is then able to preload assets intelligently on the basis of a page URL.

```php
/* Attach assets to page for smart preloading in the Service Worker */
add_action('init', function() {

    if (function_exists('O10n\attach_preload')) {

        // attach single asset to page
        \O10n\attach_preload('/path/to/image.jpg');

        // attach multiple assets to page
        \O10n\attach_preload(array('/path/to/image.jpg', 'https://cdn.google.com/script.js', '/path/to/stylesheet.css'));

    }
});
```

The Service Worker Optimization plugin ([PWA Optimization](https://github.com/o10n-x/wordpress-pwa-optimization)) provides an option to preload pages for offline availability and an option to start preloading a page on mouse down. Service Worker Push enables to automatically load all essential assets of a page on the basis of page URLs which makes it easy to make a full website available offline, or to provide an instant navigation experience for websites with many unique assets per page.

With the mouse-down preload option + Service Worker Push for CSS, the stylesheets of a page would start loading in the background as soon as a user touches a link.

### Localstorage

The plugin provides the option to cache stylesheets using localStorage with a HTTP `HEAD` based background update. LocalStorage has proven to be exceptionally fast for loading CSS, much faster than browser cache.

![localStorage CSS](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/localstorage-css.png)

### Critical CSS management

Critical CSS can be conditionally applied using a JSON config system that uses WordPress conditional methods such as `is_single` and `is_product` (WooCommerce) with the option to set attributes and match both individual conditions or a group of conditions as a whole.

![Critical CSS management](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/critical-css-conditions.png)

### External Script Proxy

The plugin provides a unique innovation to capture and proxy external script injected stylesheets to pass the [Leverage browser caching](https://developers.google.com/speed/docs/insights/LeverageBrowserCaching) rule from Google PageSpeed Insights.

The proxy can also be used to remove or modify script injected stylesheets.

![Stylesheet Proxy](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/proxy.png)

### CSS Code Search & Replace

The plugin provides an option to search and replace strings in CSS code before minification. The configuration supports both strings and regular expression.

![CSS code search & replace](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/css-code-replace.png)

With debug modus enabled, the browser console will show detailed information about the CSS loading and rendering process including a [Performance API](https://developer.mozilla.org/nl/docs/Web/API/Performance) result for an insight in the CSS loading performance of any given configuration.

![CSS debug console](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/css-debug-console.png)

### Advanced CSS editor

The plugin contains an advanced CSS editor with CSS Lint, Clean-CSS code optimization and CSS Beautifier. The editor can be personalized with more than 30 themes.

![Advanced CSS Editor](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/css-editor.png)

Additional features can be requested on the [Github forum](https://github.com/o10n-x/wordpress-css-optimization/issues).

## WordPress WPO Collection

This plugin is part of a Website Performance Optimization collection that include [Javascript](https://github.com/o10n-x/wordpress-javascript-optimization), [HTML](https://github.com/o10n-x/wordpress-html-optimization), [Web Font](https://github.com/o10n-x/wordpress-font-optimization), [HTTP/2](https://github.com/o10n-x/wordpress-http2-optimization), [Progressive Web App (Service Worker)](https://github.com/o10n-x/wordpress-pwa-optimization) and [Security Header](https://github.com/o10n-x/wordpress-security-header-optimization) optimization. 

The WPO optimization plugins provide in all essential tools that enable to achieve perfect [Google Lighthouse Test](https://developers.google.com/web/tools/lighthouse/) scores and to validate a website as [Google PWA](https://developers.google.com/web/progressive-web-apps/), an important ranking factor for Google's [Speed Update](https://searchengineland.com/google-speed-update-page-speed-will-become-ranking-factor-mobile-search-289904) (July 2018).

![Google Lighthouse Perfect Performance Scores](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/google-lighthouse-pwa-validation.jpg)

The WPO optimization plugins are designed to work together with single plugin performance. The plugins provide the latest optimization technologies and many unique innovations.

### JSON configuration

100% of the WPO plugin settings are controlled by JSON. This means that you could use the plugins without ever using the WordPress admin forms.

The JSON is verified using JSON schema's. More info about [JSON schemas](https://github.com/o10n-x/wordpress-o10n-core/tree/master/schemas).

### Local editing of optimization settings

A recently added [Stealth Optimization Config Proxy](https://github.com/o10n-x/wordpress-http2-optimization/releases/tag/0.0.55) concept makes it possible to edit the plugin settings using physical `.json` files from a local editor (with auto upload) making it efficient for fine tuning optimization settings. An update would cost a second compared to using + saving a WordPress admin panel.

https://github.com/o10n-x/wordpress-http2-optimization/releases/tag/0.0.55

## Google PageSpeed vs Google Lighthouse Scores

While a Google PageSpeed 100 score is still of value, websites with a high Google PageSpeed score may score very bad in Google's new [Lighthouse performance test](https://developers.google.com/web/tools/lighthouse/). 

The following scores are for the same site. It shows that a perfect Google PageSpeed score does not correlate to a high Google Lighthouse performance score.

![Perfect Google PageSpeed 100 Score](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/google-pagespeed-100.png) ![Google Lighthouse Critical Performance Score](https://github.com/o10n-x/wordpress-css-optimization/blob/master/docs/images/lighthouse-performance-15.png)

### Google PageSpeed score is outdated

For the open web to have a chance of survival in a mobile era it needs to compete with and win from native mobile apps. Google is dependent on the open web for it's advertising revenue. Google therefor seeks a way to secure the open web and the main objective is to rapidly enhance the quality of the open web to meet the standards of native mobile apps.

For SEO it is therefor simple: websites will need to meet the standards set by the [Google Lighthouse Test](https://developers.google.com/web/tools/lighthouse/) (or Google's future new tests). A website with perfect scores will be preferred in search over low performance websites. The officially announced [Google Speed Update](https://searchengineland.com/google-speed-update-page-speed-will-become-ranking-factor-mobile-search-289904) (July 2018) shows that Google is going as far as it can to drive people to enhance the quality to ultra high levels, to meet the quality of, and hopefully beat native mobile apps.

A perfect Google Lighthouse Score includes validation of a website as a [Progressive Web App (PWA)](https://developers.google.com/web/progressive-web-apps/).

Google offers another new website performance test that is much tougher than the Google PageSpeed score. It is based on a AI neural network and it can be accessed on https://testmysite.thinkwithgoogle.com
