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

### Detect and Respond: Deploying Modernizr

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