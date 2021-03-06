# Chapter 2. Building the page at runtime

Topics Covered:

- Lifecycle of a web application
- How HTML is processed to produce a web page
- Order of JS code execution
- Interaction through events
- The event loop

## Do You Know? (Pre-Chapter Knowledge Check)

1. Does the browser always build the page exactly according to the given HTML?
    - Browsers will build the page according to the provided HTML, but that HTML can
      be altered by a variety of languages (JavaScript, CSS, etc.) or programs 
      (the browser itself) either _after_ or _before_ the given HTML page is built.

2. How many events can a web application handle at once?
    - A JavaScript web application can handle one event at a time.
3. Why must browsers use an event queue to process events?
    - Browsers implement an _event queue_ to keep track of the order in which 
      events have taken place.

----------
## 2.1. The Lifecycle Overview

Typical Client-Side Web Application Lifecycle:
  
  1. User types a URL into the browser's address bar or clicks a hyperlink.
  2. Browser creates a request on behalf of the user, and sends the request to 
      a server.
  3. Server processes request, generates a response (generally HTML/CSS/JS code), 
      and sends the response back to the browser.
  4. Browser receives response from the server, and generates a user interface 
      as described by that response.
  5. Browser listens for events.
  6. Application invokes event handlers corresponding to occuring events.
  7. Application ends when user navigates away from the web page.

----------
## 2.2. The Page-Building Phase

Steps:

  1. Parse HTML to build the Document Object Model:
      - Performed when browser processes HTML nodes
  2. Execute JavaScript code:
      - Performed when browser encounters `script` tag

The browser switches back and forth bewtween these steps as necessary.

### 2.2.1. Parsing the HTML & Building the DOM

  - The browser receives HTML code from the server and uses it as a base for creating 
      the application's UI by parsing through the one HTML element at a time and 
      building a structured representation of the HTML page (DOM), in which every
      HTML element is represented as a node. 
  - Each HTML node can have only **ONE** parent, **MULTIPLE** children, and **MULTIPLE**
      siblings (nodes that share the same parent node).

[HTML5 Guide by Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)  
[DOM API Guide by Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)


> **NOTE:** Though HTML and the DOM are closely linked (DOM is initially constructed from HTML),
>  the two aren't synonymous. HTML code can be thought of as the _blueprint_ which the browser
>  follows when contructing the initial layout of the application UI (DOM). Blueprints can be 
>  modified as necessary. :)

  If the browser encounters a `script` element while parsing through the HTML elements, it 
    pauses DOM creation and begins executing the JavaScript code.

### 2.2.2. Executing JavaScript Code

JavaScript code is executed by the browser's JavaScript engine:

  - Firefox (SpiderMonkey)
  - Chrome (V8)
  - Opera (V8)
  - Edge (Chakra)
  - Safari (Nitro)

Primary differentiatior being compilation methods and speed. Browsers also provide an
API through a globally accesible object which the JavaScript engine uses to interact 
with and modify the page.

`window` Object in JavaScript:

  - The `window` object is the window in which an app is contained. All other globally
      available objects, variables (including user-defined variables), and browser APIs are
      available _through_ the `window` object. JavaScript uses this object to alter the DOM
      by changing/adding/removing DOM elements.

[Web API Guide by Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/API)

JavaScript code can be broadly differentiated into two types: _global_ or _function code_. _Global
  code_ is executed automatically by a browser's JavaScript engine and is interpreted in a linear, 
  line-by-line manner. _Function code_ isn't executed until it's **called upon**; defining
  a function doesn't invoke it (in most cases), so to run the process defined in the function the
  program (or the JavaScript engine, or the browser) has to be told to call it by specifying
  the function name, followed with matching parenthesis that contain user-provided arguments
  or arguments that are required by the function.

All user-defined global variables that are defined during the program's runtime remain within
  the global `window` object unless the window is closed or the variable is over-written.

----------
## 2.3. Event Handling

Once the browser has completed parsing through the HTML document, it moves on to the second 
step of the standard web app lifecyle: _event handling_.

### 2.3.1. Event Handling Overview

The browser is based on a _single-threaded execution model_; only a single piece of code can be 
  executed at one time. This means that the browser processes events one at a time, and utilizes
  an _event queue_ to store all generated events in the ordered that the browser processed them.
  The event handling flow can be described by the following flowchart:

  - Browser checks the event queue head.
  - If no event exists, browser continues to check.
  - If an event exists, the browser executes the corresponding event handler (if one exists).
      While this event is being processed, any other events that get added to the queue 
      wait for this event's process to complete.

Events occur and are responded to in asynchronous (AKA unpredictable) fashion. As such, we
  handle the invocation of event-handling functions in the same manner. Code is written
  and set up well before an event ever occurs, and waits to be called upon and executed as
  needed. In order to 'listen' for events, our progam needs to alert the browser of
  which events it has handlers for.

### 2.3.2. Registering Event Handlers

There are two ways to register event handlers:

  1. Assign functions to special properties
  2. Use built-in `addEventListener()` method

Best practice is to use the `addEventListener()` method; allows for multiple handlers 
for any given event, where as assigning functions to special properties limits us
to one handler per event.

### 2.3.3. Handling Events

As events occur, the browser calls upon the associated event handler. Events are
  processed in the order they're received, one at a time; the current event must 
  complete execution before the next event can be processed.

----------
## 2.4. Summary

- After receiving HTML code from a server, the browser uses that code as a blueprint
    to build out the Document Object Model (DOM), an internal representation of the
    application's client-facing UI.

- JavaScript code is used to _dynamically modify_ the DOM and provides interactivity
    between the application and the user.

- Client-side web applications are executed in two phases:

    1. Page building phase: Browser processes HTML code to create the DOM, and global
        JS code is executed as `script` nodes are discovered. The JS code can modify
        the DOM and register _event handlers_ using the built-in `addEventListener()`
        method.
    2. Event handling: The browser processes events one at a time in the order they 
        are generated. While processing an event, any additional events are stored in
        an _event queue_ in the order received. If the event loop discovers an event at
        the top of the event queue, the browser invokes the corresponding event-handler
        if one exists.

----------
## 2.5. Exercies

1. What are the two phases in the lifecycle of a client-side application?
    - Page building phase: Browser processes HTML code and uses as a blueprint to generate
      the DOM, then processes any global JavaScript code located within `script` tags. JS
      can further modify the DOM and register event handlers to respond to specific actions
      or events.

    - Event handling: Browser enters a loop, listening for events and responding with the
      appropriate event handler (if one exists). If the browser is processing one event,
      any additional events are stored in the _event queue_ and are processed in the order
      they were received, but only after the current event has completed execution.

2. What is the main advantage of using the `addEventListener()` method to register
    an event handler versus assigning a handler to a specific element property?

    - Assigning an event handler to a specific element property limits the developer
        to one _event handler_ function per eventt, where as the `addEventListener()`
        method allows developers to register as many _event handler_ functions as needed.

3. How many events can be processed at once?

    - The browser can handle process one event at a time; any additional events are
        stored in an _event queue_ to be processed in the order they were received, but
        only after the current event process finishes execution.

4. In what order are events from the event queue processed?

    - Events from the event queue are processed in the order they are received.
