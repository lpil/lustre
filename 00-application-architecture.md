# Application Architecture: Considerations, Best Practices, And So On

> NOTE: This is very much a work in progress. Proceed as though everything you read here is up for debate; because, well, it is! I just thought I'd go ahead and get started on this, while I was feeling good and motivated.

Lustre is a full-stack web application development platform designed in the grand tradition of "The Elm Architecture", known more generally as _Model-View-Update_ and henceforth referred to as **MVU**.

Conceptually, MVU is fairly straightforward:

- Your application defines a `Model`, which is a persistent store of your application's state.
- Your application defines a `view`, which renders the output of the `Model` to IO (in many cases, as HTML to a web browser) and can create _messages_ based on user input events.
- Your application defines an `Update` - a function that processes _messages_ from IO and updates the application's state in the `view`.

In practice, MVU can also _be_ fairly straightforward; but in many cases, MVU applications can suffer from some of the same problems as any other codebase, developed in any other language, in any other programming paradigm.

This document seeks to address the following topics:

- The roles and responsibilities of each part of the MVU architecture
- An approach for designing and implementing a MVU application that can scale in any direction
- Common architectural missteps in MVU development, and how they can be avoided or remediated

## MVU 101

While MVU shares some surface-level attributes with other acronymic architectural patterns (such as MVC, MVVM, etc), the nature of MVU is fundamentally different in some important ways; of utmost interest is the nature of the `view` and the `Update`.

In a MVU application, **the `view` and `Update` are implemented as functions that are executed by a runtime**. Said aother way: building a MVU application requires the programmer to define functions to represent the view, and any events that are emitted by either the view or the runtime.

```
Cool Lustre ASCII art diagram goes here
```

Imagine a simple website with a single button When that button is clicked, the `onClick` handler for that button does _not_ "run" or "call" a function; that event is **emitted** by the button element, and it is **handled** in the `Update` function.

(unfinished)

## MVU Considerations
(todo)

### Model
(todo)

### View

**Terminology:**

- `view`: Refers to the single `view` function that must be created and passed to the constructor of a Lustre application
- view-function: Any function that is called from `view` and returns an `Element(msg)`, or a higher-ordered function that eventually returns an `Element(msg)`
    - Example: `html.text` is a view-function
    - Counter-example: `list.map` is _not_ a view function
    - Example:
        ```
        const sentence = list.map(html.text, ["Hello", "how", "are", "you"])
        ```
        is a view-function

#### About the `view`

The `view` in MVU is implemented as a function at the top level of the application that the runtime parameterizes with the `Model`. In Lustre, the `view` function returns a value of type `Element(msg)`, which the runtime interprets as a set of instructions for:

- What content to render to the user, and
- The type of the messages that the user will be able to raise as events

A Lustre application only has one `view` function; however, it is typically composed of many smaller functions that return an `Element(msg)`. One common approach is to divide the application up into "pages", where each page corresponds to a top-level URL path, and for the `view` function to branch from the state of the URL, mapping each path to a function that represents the data that should be displayed for that "page"; but this is a convention borne of convenience, and is not strictly necessary.

#### The `view`, Data, and You

However, the fact that a Lustre application only has one `view` function brings us to an interesting observation; it causes it to be the case that the structure of the `view` is always going to be a single **tree** - which, at the root node, always has all of the application's data in-scope.

Data travels "downwards" from the root of the `view` and can be passed into any of the 

(data does NOT travel "upwards", although messages can be handled)

( there's only one view )

#### Considering the View as IO

#### Structuring Views