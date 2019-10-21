read browser rendering from notebook


Critical Rendering Path--

1)DOM
2)CSSOM
3)Render Tree
4)Layout
5)Paint

--->  executing our inline script blocks DOM construction, which also delays the initial render.
---> the browser delays script execution and DOM construction until it has finished downloading and constructing the CSSOM.
---> In the case of an external JavaScript file the browser must pause to wait for the script to be fetched from disk, cache, or a remote server, which can add tens to thousands of milliseconds of delay to the critical rendering path.

To void it use -- async keyword
<script src="app.js" async></script>
Adding the async keyword to the script tag tells the browser not to block DOM construction while it waits for the script to become available, which can significantly improve performance.


----- 
Critical Rendering  Measure

<!DOCTYPE html>
<html>
  <head>
    <title>Critical Path: Measure</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
    <script>
      function measureCRP() {
        var t = window.performance.timing,
          interactive = t.domInteractive - t.domLoading,
          dcl = t.domContentLoadedEventStart - t.domLoading,
          complete = t.domComplete - t.domLoading;
        var stats = document.createElement('p');
        stats.textContent = 'interactive: ' + interactive + 'ms, ' +
            'dcl: ' + dcl + 'ms, complete: ' + complete + 'ms';
        document.body.appendChild(stats);
      }
    </script>
  </head>
  <body onload="measureCRP()">
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
  </body>
  
 ------------------------- Preload--------------------
 
<link rel="preload"> informs the browser that a resource is needed as part of the current navigation, and that it should start getting fetched as soon as possible. Here’s how you use it:

<link rel="preload" as="script" href="super-important.js">
<link rel="preload" as="style" href="critical.css">
  
Use-case: Fonts
Fonts are a great example of late-discovered resources that must be fetched, often sitting at the bottom of one of several CSS files loaded by a page.

In order to reduce the amount of time the user has to wait for the text content of your site, as well as avoid jarring flashes between system fonts and your preferred ones, you can use <link rel="preload"> in your HTML to let the browser know immediately that a font is needed
  
  
   ------------------------- Preconnect--------------------
   
<link rel="preconnect"> informs the browser that your page intends to establish a connection to another origin, and that you’d like the process to start as soon as possible.
  
  
  Use-case: Streaming Media
  
   ------------------------- Prefetch--------------------
<link rel="prefetch"> is somewhat different from <link rel="preload"> and <link rel="preconnect">, in that it doesn’t try to make something critical happen faster; instead, it tries to make something non-critical happen earlier, if there’s a chance.

It does this by informing the browser of a resource that is expected to be needed as part of a future navigation or user interaction,





OPtimization

Decrease Front-end Size---
Use the production mode (webpack 4 only)

1. Enable minification
Minification is when you compress the code by removing extra spaces, shortening variable names and so on
2.Specify NODE_ENV=production
3. Use ES modules
When you use ES modules, webpack becomes able to do tree-shaking. Tree-shaking is when a bundler traverses the whole dependency tree, checks what dependencies are used, and removes unused ones. So, if you use the ES module syntax, webpack can eliminate the unused code:
--------Tree shaking---------
Tree shaking is a term commonly used in the JavaScript context for dead-code elimination
4. Optimize images


---Steps for optimzation------
Enable the production mode if you use webpack 4
Minimize your code with the bundle-level minifier and loader options
Remove the development-only code by replacing NODE_ENV with production
Use ES modules to enable tree shaking
Compress images
Apply dependency-specific optimizations
Enable module concatenation
Use externals if this makes sense for you


Summing up:

Cut unnecessary bytes. Compress everything, strip unused code, be wise when adding dependencies

Split code by routes. Load only what’s really necessary right now and lazy-load other stuff later

Cache code. Some parts of your app are updated less often than other ones. Separate these parts into files so that they are only re-downloaded when necessary

Keep track of the size. Use tools like webpack-dashboard and webpack-bundle-analyzer to stay aware how large is your app. Take a fresh look at your app’s performance at whole every few months





HTTP2

Main goals of developing HTTP/2 was:

1. Request multiplexing
HTTP/2 can send multiple requests for data in parallel over a single TCP connection. This is the most advanced feature of the HTTP/2 protocol because it allows you to download web files asynchronously from one server. Most modern browsers limit TCP connections to one server.

2.Header compression

3.HTTP/2 Server Push
This capability allows the server to send additional cacheable information to the client that isn’t requested but is anticipated in future requests. For example, if the client requests for the resource X and it is understood that the resource Y is referenced with the requested file, the server can choose to push Y along with X instead of waiting for an appropriate client request.

4. Binary Protocol
The latest HTTP version has evolved significantly in terms of capabilities and attributes such as transforming from a text protocol to a binary protocol. HTTP1.x used to process text commands to complete request-response cycles. HTTP/2 will use binary commands (in 1s and 0s) to execute the same tasks. This attribute eases complications with framing and simplifies implementation of commands that were confusingly intermixed due to commands containing text and optional spaces.




Virtual DOM VS REAL DOM--------

Your browser does a lot of work to update the DOM. Changing the DOM can trigger reflows and repaints; when one thing changes, the browser has to re-calculate the position of other elements in the flow of the page, and also has to do work re-drawing.

The browser has its own internal optimization to reduce the impact of DOM changes (e.g. doing repaints on the GPU, isolating some repaints on their own layers, etc), but broadly speaking, changing a few things can trigger expensive reflows and repaints.

It's common even when writing everything from scratch to build UI off the DOM, then insert it all at once (e.g. document.createElement a div and insert a whole tree under it for attaching to the main DOM), but React is engineered to watch changes and intelligently update small parts of the DOM to minimize the impact of reflows and repaints. It goes much further than what you might typically build manually.


What is DOM reflow?

A reflow computes the layout of the page. A reflow on an element recomputes the dimensions and position of the element, and it also triggers further reflows on that element’s children, ancestors and elements that appear after it in the DOM. Then it calls a final repaint. Reflowing is very expensive, but unfortunately it can be triggered easily.

Reflow occurs when you:

insert, remove or update an element in the DOM

modify content on the page, e.g. the text in an input box


move a DOM element

animate a DOM element

take measurements of an element such as offsetHeight or getComputedStyle

change a CSS style

change the className of an element

add or remove a stylesheet

resize the window

scroll


Virtual dom doesnot speed our initial render , its only fast the updating the dom.



When new elements are added to the UI, a virtual DOM, which is represented as a tree is created. Each element is a node on this tree. If the state of any of these elements changes, a new virtual DOM tree is created. This tree is then compared or “diffed” with the previous virtual DOM tree.

Once this is done, the virtual DOM calculates the best possible method to make these changes to the real DOM. This ensures that there are minimal operations on the real DOM. Hence, reducing the performance cost of updating the real DOM.



####Repaint
As the name suggests repaint is nothing but the repainting element on the screen as the skin of element change which affects the visibility of an element but do not affects layout.
Example.
1. Changing visibility of an element.
2. Changing outline of the element.
3. Changing background.
Would trigger a repaint.
According to Opera, the repaint is an expensive operation as it forces the browser to verify/check visibility of all other dom nodes.


####Reflow
Reflow means re-calculating the positions and geometries of elements in the document, for the purpose of re-rendering part or all of the document. Because reflow is a user-blocking operation in the browser, it is useful for developers to understand how to improve reflow time and also to understand the effects of various document properties (DOM depth, CSS rule efficiency, different types of style changes) on reflow time. Sometimes reflowing a single element in the document may require reflowing its parent elements and also any elements which follow it.


What makes React’s virtual DOM so fast?
React doesn’t really do anything new. It’s just a strategic move. What it does is It stores a replica of real DOM in memory. When you modify the DOM, it first applies these changes to the in-memory DOM. Then, using it’s diffing algorithm, figures out what has really changed.
Finally, it batches the changes and call applies them on real-dom in one go. Thus, minimizing the re-flow and re-paint.

