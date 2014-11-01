<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Bombproof Web Design](#bombproof-web-design)
  - [Detect and Respond: No JavaScript](#detect-and-respond-no-javascript)
    - [Sorry, you must enable JavaScript to use this site.](#sorry-you-must-enable-javascript-to-use-this-site)
  - [Detect and Respond: Problem Browsers](#detect-and-respond-problem-browsers)
    - [Detect older versions of IE](#detect-older-versions-of-ie)
    - [Detect Opera Mini](#detect-opera-mini)
  - [Detect and Respond: Deploying Modernizr](#detect-and-respond-deploying-modernizr)
      - [Hello](#hello)
    - [Modernizr Bower Gotcha](#modernizr-bower-gotcha)
  - [Cross Compatible Type](#cross-compatible-type)
    - [Web Font Issues (including icon fonts)](#web-font-issues-including-icon-fonts)
    - [OS Font Issues](#os-font-issues)
    - [Icon Font Issues](#icon-font-issues)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Bombproof Web Design

My notes and project from Tuts Plus course [Bombproof Web Design](https://webdesign.tutsplus.com/courses/bombproof-web-design)

## Detect and Respond: No JavaScript

To quickly determine how your site will look with no JavaScript, use [Quick Javascript Switcher](https://chrome.google.com/webstore/detail/quick-javascript-switcher/geddoclleiomckbhadiaipdggiiccfje?hl=en) chrome extension.

To create a fallback, use `<noscript>`, for example:

  ```html
  <noscript>
    Sorry, this site needs JavaScript enabled to display properly.
  </noscript>
  ```

Content can also be styled, for example:

  ```html
  <noscript>
    <main class="container">
      <div class="alert alert-danger" role="alert">
        <h3>Sorry, you must enable JavaScript to use this site.</h3>
      </div>
    </main>
  </noscript>
  ```

## Detect and Respond: Problem Browsers

For example, if site uses web components, will not work on IE8 and below. Even with polyfill, won't work so can't support this browser.

Also, IE6 is so old not even supported by Microsoft, but is easy enough to display notice to these users to go and upgrade their browsers.

### Detect older versions of IE

Use conditional tags, change version number as appropriate:

  ```html
  <!--[if lt IE 10]>
    <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
  <![endif]-->
  ```

### Detect Opera Mini

Use JavaScript to parse user agent

  ```html
  <script>
    var isOperaMini = (navigator.userAgent.indexOf('Opera Mini'));
    if (isOperaMini) {
      $('#operamini').show();
    }
  </script>
  ```

## Detect and Respond: Deploying Modernizr

[Modernizr](http://modernizr.com/download/) performs feature detections.

Modernizr also load common polyfills, such as html5shiv (support main, header, footer, aside etc elements) and media queries.

Adds class attribute to `<html>` element with list of supported styling features, and `no-` versions of these when not supported.

For example, `boxshadow` is supported on Chrome

  ```html
  <html boxshadow>
  ```

But not supported on IE8

  ```html
  <html no-boxshadow>
  ```

So given an element you want to style with boxshadow such as

  ```html
  <div class="wrapper">
    <h4>Hello</h4>
    <p>This is a box shadow</p>
  </div>
  ```

CSS can target both supported and not supported versions as follows

  ```css
  .wrapper {
    -webkit-box-shadow: 3px 3px 3px 0px #c0c0c0;
    box-shadow: 3px 3px 3px 0px #c0c0c0;
  }

  .no-boxshadow .wrapper {
    border: 1px solid #c0c0c0;
    -ms-filter: "progid:DXImageTransform.Microsoft.Shadow(Strength=5, Direction=135, Color='#c0c0c0')";
    filter: progid:DXImageTransform.Microsoft.Shadow(Strength=5, Direction=135, Color='#c0c0c0');
  }
  ```

Modernizr can be used to load polyfill if a feature is not supported.
For example, to load Adobe Flash player when HTML5 video element is not supported:

  ```html
  <script type="text/javascript">
  Modernizr.load({
    test: Modernizr.video,
    nope: 'http://api.html5media.info/1.1.8/html5media.min.js'
  });
  </script>
  ```

### Modernizr Bower Gotcha

Bower installed version of Modernizr does not come with `load` test.
You must download a [custom install](http://modernizr.com/download/#-fontface-backgroundsize-borderimage-borderradius-boxshadow-flexbox-flexboxlegacy-hsla-multiplebgs-opacity-rgba-textshadow-cssanimations-csscolumns-generatedcontent-cssgradients-cssreflections-csstransforms-csstransforms3d-csstransitions-applicationcache-canvas-canvastext-draganddrop-hashchange-history-audio-video-indexeddb-input-inputtypes-localstorage-postmessage-sessionstorage-websockets-websqldatabase-webworkers-geolocation-inlinesvg-smil-svg-svgclippaths-touch-webgl-shiv-mq-cssclasses-teststyles-testprop-testallprops-hasevent-prefixes-domprefixes-load)
and ensure that `Modernizr.load` is checked

## Cross Compatible Type

### Web Font Issues (including icon fonts)

Web fonts can render differently or poorly on different browsers. Web fonts may not load or be supported properly.

For example, [ChangaOne](https://www.google.com/fonts/specimen/Changa+One) from Google Fonts renders nice and smoothly on Chrome on OSX,
but on Chrome for Ubuntu, and Windows, is rough and jagged. Same font on Firefox looks good both on OSX and Windows.

When choosing web fonts, first check how they look on as many different browsers and operating systems as possible.

### OS Font Issues

Some OS fonts can be missing on some systems. For example, Helvetica Neue is Mac only.

Generally fonts that are safe to use on Windows will also be available on Mac.

For the most consistent experience, use generic font families, to specify a font style rather than particular font.
This will work on any system, no matter what fonts it has available. Font families are:

* Serif
* Sans Serif
* Cursive
* Fantasy
* Monospace

Always specify a font stack in css, list of fonts to use in preferred order. For example:

  ```css
  .wrapper {
    font-family: "Helvetica Neueu", "Arial", sans-serif;
  }
  h1 {
    font-family: "Lemon", fantasy;
  }
  ```

### Icon Font Issues

For example, [Font Awesome](http://fortawesome.github.io/Font-Awesome/icons/)

These use `@font-face` but this is not supported in Opera Mini. Each icon will render as little empty rectangle. No polyfill for this.

Can use Modernizr to detect whether font-face is available, and if not, use css to load images instead.