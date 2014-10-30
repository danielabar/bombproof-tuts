# Bombproof Web Design

My notes and project from Tuts Plus course [Designing in the Browser with Bootstrap](https://webdesign.tutsplus.com/courses/bombproof-web-design)

## Detect and Respond: No JavaScript

To quickly determine how your site will look with no JavaScript, use [Quick Javascript Switcher](https://chrome.google.com/webstore/detail/quick-javascript-switcher/geddoclleiomckbhadiaipdggiiccfje?hl=en) chrome extension.

To create a fallback, use `<noscript>`, for example:

  ```
  <noscript>
    Sorry, this site needs JavaScript enabled to display properly.
  </noscript>
  ```

Content can also be styled, for example:

  ```
  <noscript>
    <main class="container">
      <div class="alert alert-danger" role="alert">
        <h3>Sorry, you must enable JavaScript to use this site.</h3>
      </div>
    </main>
  </noscript>
  ```