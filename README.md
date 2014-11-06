<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Bombproof Web Design](#bombproof-web-design)
  - [Detect and Respond: No JavaScript](#detect-and-respond-no-javascript)
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
  - [CSS Preprocessors and Prefixes](#css-preprocessors-and-prefixes)
  - [Using Normalize.css](#using-normalizecss)
  - [Allowing Base Font Size Control](#allowing-base-font-size-control)
    - [Scaling](#scaling)
  - [Dealing With Displays](#dealing-with-displays)
  - [Input Mechanisms](#input-mechanisms)
  - [Creating Accessibility](#creating-accessibility)

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
        Sorry, you must enable JavaScript to use this site.
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

## CSS Preprocessors and Prefixes

Needed for relatively new css properties that aren't standard yet in all browsers. For example

  ```css
  .wrapper {
    transform: rotate(7deg);
    -webkit-transform: rotate(7deg);
  }
  ```

It's a lot to keep track of, use tools to automate the process.

[Autoprefixer](https://github.com/postcss/autoprefixer) open source library, looks up css properties on [CanIUse](http://caniuse.com/),
and incorporates vendor prefixes if necessary.

Another possibility is to use a CSS preprocessor such as [Stylus](http://learnboost.github.io/stylus/),
[SASS](http://sass-lang.com/), or [LESS](http://lesscss.org/).

Another tool for working with vendor prefixes and CSS preprocessors is [Prepros App](https://prepros.io/),
it's a paid app, but also has a trial version. Simply drag in a folder containing for example, stylus, and it automatically compiles to CSS.
There is also an option to add vendor prefixes.

[nib](http://tj.github.io/nib/) is a mixin library for Stylus. This library also incorporates vendor prefixes.

## Using Normalize.css

[Normalize.css](http://necolas.github.io/normalize.css/) is used to handle cross browser inconsistencies.

The idea is to load normalize.css as first css file on your site,
to create a foundation that normalizes all of the default behaviours of html elements across browsers.

It's available in Stylus, LESS, or SASS versions. For example, if your site uses LESS, import normalize into your main stylesheet as follows

  ```LESS
  @import "normalize.less";
  ```

## Allowing Base Font Size Control

Never set pixel based font size. For example, **_DO NOT DO THIS_**

  ```css
  html {
    font-size: 16px;
  }
  ```

Because user can change font size, for example in Chrome:
* Settings
* Show Advanced Settings
* Web Content
* Font size

If you don't have base font size set, then user's settings change will be applied.
But if you have set base font size, then user's settings are not applied.
This is bad, user might have very good reason for increasing font size, such as if they are vision impaired.

Another reason to leave font size alone is because on different devices, browser vendors have determined the optimal base font size.

Also on very large devices, such as TV, user tends to be sitting far away from the screen,
and commonly UI designs optimized for TV screens have everything blown up very large.

Another consideration is how padding, margin and border radius can vary with base font size. If set in pixels, these values are carved in stone.

### Scaling

Can use CSS pre-processors to ensure that when user changes base font size, the whole site will scale with it.

Easiest way to scale margin and padding is to use `em` values instead of `px`.

Preprocessor can help in px/em conversion, for example, in stylus:

  ```stylus
  $base_font_size = 16
  $base_font_size_ems = nit($base_font_size / 16, em)
  $px = unit(1 / $base_font_size, rem)
  $px_em = unit(1 / $base_font_size, em)

  .wrapper {
    border 3 * $px solid grey
  }

1em represents however big the font is in a particular region.
For example, suppose base font size is set to 10px, then 1.5em = 15px, 3m = 30px, etc.

If you must adjust font size somewhat (for example, working with a smaller typeface), can use em's, for example

  ```css
  html {
    font-size: 1.1em;
  }
  ```

## Dealing With Displays

Key aspects to consider about displays:

1. Resolution
1. Pixel density (how many pixels per square inch)
1. Viewport size (amount of space in the screen that your site has to take up)
1. Physical size of the display

There are millions of different resolutions a device could have.
Any given resolution could have a different physical size depending on pixel density and physical size of the device.

Example: A 1920 x 1080 resolution could be on a 5" smartphone or on a 65" smart tv.

Best way to deal with all the different screen sizes and resolutions is not to deal with them at all (counterintuitive).
Site layout should be completely flexible and adaptable, i.e. device agnostic.

Two main aspects for creating display agnostic layouts

1. Every aspect of layout should have fully flexible width (never used fixed width)
1. Use media queries to adjust layout on increasingly smaller screens

Use css attributes `width` and `max-width`, for site overall, for example

  ```css
  .wrapper {
    max-width: 75rem;
    width: 100%;
    margin: 1.5em auto;
    zoom: 1;
  }
  ```

Within that, use percentages for widths, for example

  ```css
  .content {
    width: 64%;
    float: left;
    margin-right: 2%;
  }
  .sidebar {
    width: 34%;
    float: left;
  }
  ```

Use media queries to adjust layout at varying widths. For example, a two column layout doesn't make sense on a small display.

Determining at which width layout should change will vary with each site you build, and requires some trial and error.
Try shrinking the browser width down until you get to a point where it seems hard to read.
For example with the two column layout, ~840px width. That's a good place for a media query.

Every media query is called a breakpoint. This stylus example shows media query with calculation to use rem units rather than px

  ```stylus
  $base_font_size = 16
  $px = unit(1 / $base_font_size, rem)

  $query_01 = "(max-width: " + 840 * $px + ")"

  @media $query_01
    .panel
      padding 1.5em
  ```

To stack the two columns into one at a smaller breakpoint, for example 650 width, set width to 100% and float to none

  ```stylus
  $query_02 = "(max-width: " + 650 * $px + ")"

  @media $query_02
    .content
      width 100%
      float none
      margin-right 0
      margin-bottom 1.5em

    .sidebar
      width 100%
      float none
  ```

## Input Mechanisms

Don't assume user has a mouse to hover over menu items and interact with page. Input devices can include:

* Mouse
* Touch
* Keyboard / Game Controller / TV Remote
* Voice

Enabling pure keyboard interaction is good for accessibility.
Also, if site is fully navigable by keyboard, it will also be navigable by game controller and tv remotes.

Guidelines:

* Don't use hover dependent interactions (because mouse is only input mechanism that supports this)
* Ensure navigation can be done with TAB, arrow, Esc and ENTER keys

CSS dropdowns are [example](http://csswizardry.com/demos/css-dropdown/) of bad interaction because it only works with mouse, not touch friendly.

Example of menu that can be interacted with via touch and keyboard is nav provided by Bootstrap, [example](http://bootswatch.com/).
Dropdown menus are activated by a click instead of hover. Also menus that have dropdown have a small down arrow beside them
to give a hint that there are items.

As user is navigating the site using a keyboard, should provide visual feedback as to which part of the page is being interacted with.
For example, if user tabs to a link, provide an outline around it.

## Creating Accessibility

Things to think about:

* Font sizing, see [Allowing Base Font Size Control](#allowing-base-font-size-control)
* Allowing keyboard based navigation (helpful for people with visual and mobility impairments)
* Providing text accompaniment to auditory content (eg: videos should have closed captions)
* WAI-ARIA landmark roles & heading structure for screen readers
* Color contrast (helpful for people who have color blindness or reduced vision)

Try navigating your site using a screen reader.
On Mac, Cmd+F5 to turn on [Voice Over](https://www.apple.com/voiceover/info/guide/_1121.html)
