# Application Architecture: Considerations, Best Practices, And So On

> NOTE: This is very much a work in progress. Proceed as though everything you read here is up for debate; because, well, it is! I just thought I'd go ahead and get started on this, while I was feeling good and motivated.

Lustre is a full-stack web application development platform designed in the grand tradition of "The Elm Architecture", known more generally as _Model-View-Update_ and henceforth referred to as **MVU**.

Conceptually, MVU is fairly straightforward:

- Your application defines a `Model`, which is a persistent store of your application's state.
- Your application defines a `View`, which renders the output of the `Model` to IO (in many cases, as HTML to a web browser) and can create _messages_ based on user input events.
- Your application defines an `Update` - a function that processes _messages_ from IO and updates the application's state in the `View`.

In practice, MVU can also _be_ fairly straightforward; but in many cases, MVU applications can suffer from some of the same problems as any other codebase, developed in any other language, in any other programming paradigm.

This document seeks to address the following topics:

- The roles and responsibilities of each part of the MVU architecture
- An approach for designing and implementing a MVU application that can scale in any direction
- Common architectural missteps in MVU development, and how they can be avoided or remediated

## MVU 101

While MVU shares some surface-level attributes with other acronymic architectural patterns (such as MVC, MVVM, etc), the nature of MVU is fundamentally different in some important ways; of utmost interest is the nature of the `View` and the `Update`.

In a MVU application, **the `View` and `Update` are implemented as functions that are executed by a runtime**. Said aother way: a programmer building a MVU application requires the programmer to define functions to represent the view, and any events that are emitted by either the view or the runtime.

```
Cool Lustre ASCII art diagram goes here
```

Imagine a simple website with a single button When that button is clicked, the `onClick` handler for that button does _not_ "run" or "call" a function; that event is **emitted** by the button element, and it is **handled** in the `Update` function.
