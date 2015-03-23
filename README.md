# JavaScript Style Guide

**NB. This style guide should be considered a [living document][0]**.

Many style guides fail by being too rigid in their expectations. A large part of programming is the _art_ of the craft, and that includes knowing when to bend (or even _break_) the rules. This style guide instead opts for a flexible approach when it comes to syntax with this in mind.

## Visual and syntactical style

- 2 spaces for indentation (soft tab) **always**. Reads:
    - <http://blog.codinghorror.com/death-to-the-space-infidels/>
    - <http://www.emacswiki.org/emacs/TabsAreEvil>
    - <http://lemire.me/blog/archives/2005/11/24/tabs-are-evil/>

- POSIX-complient line endings (Sorry, Windows, but `\r\n` is _utter rubbish_). Read more about [dealing with line endings][3]. 

- Any line of code must be free of trailing whitespace, including empty (or "blank") lines (easy to integrate into any decent IDE as an "on-save" hook or action). 
- Use a linting tool (such as _JS Hint_) to force a flexible, yet firm, style for code consistency as a "pre-commit" hook. 
- If using a long chain of function methods (such as those often seen in fluent interfaces), break each call onto a new line, indented for clarity:
    ```js
    // Bad
    myFunction.trigger('foo').setAttribute('bar').doAnotherThing();
    
    // Good
    myFunction
      .trigger('foo')
      .setAttribute('bar')
      .doAnotherThing();
    ```
    
- Do not combine a single `var` statement for multiple variables, let JavaScript runtime engines deal with such optimizations:
    ```js
    // Bad
    var foo = 'foo',
        bar = 'bar',
        noop = function () {};
        
    // Good
    var foo = 'foo';
    var bar = 'bar';
    var noop = function () {};
    ```
    
- Be explicity, not implicit. This is applicable in many ways and can be hard to illustrate, but in short, it is better to explicitly call and define things than to rely on _magic_. **Magic bad**.
- Naming things is hard, strive for excellence because code **is** documentation. The next developer to dive into your code (including future-you) will thank you for the time you spent on naming things with clarity and intention.
- [Use verbs, not nouns][5] when naming functions and methods.
- Name variables to convey meaning and natural language. For example, a state-flag variable:
    ```js
    // Bad
    var checkLoggedIn = false;
    if (checkLoggedIn === true) // ...
    
    // Good
    var userIsLoggedIn = false;
    if (userIsLoggedIn) // ...
    ```
    
- Use camel-case for variable naming (this is not PHP!):
    ```js
    var not_marmot; // Bad
    var niceMarmot; // Good
    ```

- For the love of G-d, **do not use comma-first**:
    ```js
    // Bad
    var o =
      { a: "ape"
      , b: "bat"
      , c: "cat"
      , d: "dog"
      , e: "elf"
      , f: "fly"
      , g: "gnu"
      , h: "hat"
      , i: "ibu"
      };
    
    // Good
    var o = {
      a: "ape",
      b: "bat",
      c: "cat",
      d: "dog",
      e: "elf",
      f: "fly",
      g: "gnu",
      h: "hat",
      i: "ibu"
    },
    ```

- Always use a trailing semi-colon. This one can be controversial as they are, at times, _optional_. However I see this as a syntactical extension of being explicity over implicit. It additionally aids in preventing those edge-case problems.
- Braces can be optional, if used correctly, for visually beautiful code, especially given particular circumstances:
    ```js
    // Good
    if (true) {
      return true;
    }
    
    // Nope
    if (true) { return true; }
    
    // Also good
    if (true) return true;
    
    // Also good for times when you wish to avoid long lines
    if (true)
      return someConfiguration[environment].foo();
    
    // This really shines, in my opinion in the following example:
    
    // Okay
    if (true) {
      return thing;
    } else {
      return otherThing;
    }
    
    // Pretty
    if (true) return thing;
    else return otherThing;
    ```

- Try to avoid a function with a body longer than 15 lines. This is usually a codesmell that the function is doing too much and can be broken into multiple functions.
- Functions should almost always have a `return` value.
- Put all `require`s at the top of the page for clarity of dependencies:
    ```js
    // Bad
    if (true) return require('module-name')();
    
    // Good
    var moduleName = require('module-name');
    if (true) return moduleName();
    ```
    
- Declare `var` statements close to their local usage. In other words, do not feel the need to adhere to the Crockford way of declaring a temporary variable up at the very top of a section as dogma.
- Do not declare `var` in loops whenever possible.
- Love [semver][1]. <3  Because version numbers should convey meaning.
- Don't try to treat JavaScript like a classical OO language, it is not. Treat it with respect, learn to understand prototypical programming. **Love the function, not the class**.
- Avoid anonymous functions for better call stack traces and optimization. This is as easy as naming functions:
    ```js
    var obj = {
      methodA: function methodA() {}, // Good
      methodB: function() {}, // Not so good
      // ...
    };
    ```

- Be sane with nesting functions. Avoid _[Callback Hell][2]_.
- Embrace non-blocking code, understand and use asynchronous code whenever possible.
- One of Node's greatest strengths is one of it's most overlooked features: _streams_. Stream all the things!
- Avoid jQuery unless there is a good reason to actually use it (there rarely is for modern applications). 
- Keep markup in _markup_ or _template_ files (`.html`, `.jade`, etc.) and JavaScript in JavaScript files.
- Favour [npm][6] modules and use them everywhere with [browserify][4]:
    - Resolves all namespacing concerns (because globals are lame and namespacing is even lamer).
    - Huge number of libraries available in the wild.
    - Extremely easy to compose.
    - Extremely easy to test in isolation.
    - Amazing ecosystem and dependency management.

- Aim for 100% test coverage. Understand and appreciate the difference between unit tests and integration tests—both are important! Tools like [covert][7] can help report your current coverage.
- Unit tests should only test the subject in question. In other words, learn to use mocks, stubs, and spies, when called for.
- Aim for headless testing whenever possible, ie. [testling][8], jsdom, zombie.js, etc.
- Do not prematurely optimize code for optimization's sake (Big O nogtation has value when bottlenecks are discovered, but readability has more value overall).
- Prefer readibility over optimization, ie. using iterators (`collection.forEach();`) is always better than using `for` loops.
- Code for clarity
- Prefer composition over inheritance:
    - Approach should always be like Lego, not carpentry: using small, modular pieces that connect is fun. _In other words, avoid building monoliths_.

- Use _strict_ (`===`) equality **always**.
- Always use _strict_ mode (`'use strict';`).
- Never extend native primitives (also referring to as monkeypatching).
- Use `akiva/module-generator` or the like, for consistent starting points, without the assumptions of true scaffolding
- Do not use lame whitespace characters for alignment, as is often seen in Ruby and Python:
    ```js
    // Bad
    var something        = require('something');
    var anotherThing     = require('another-something');
    var yetMoreSomething = require('yet-more-something');
    ```

## Parting notes

JavaScript and front-end development is the fastest growing field of computer sciences; faster than we can reasonably keep up with. However, most of this is self-induced because developers feel the need to constantly build monoliths to solve easy problems rather than see things as a composition of small, digestible pieces.

## Other style guides and best practices in the widle

If you fancy reviewing some other documents expressing views other than my own, here are a few popular choices:

- <https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md>
- <https://github.com/stevekwan/best-practices/blob/master/javascript/gotchas.md>
- <https://github.com/airbnb/javascript>
- <https://github.com/felixge/node-style-guide>
- <https://github.com/Jam3/jam3-testing-tools>
- <https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md>
- <http://james.padolsey.com/javascript/javascript-bad-practices/>
- <https://www.python.org/dev/peps/pep-0008/>

[0]: https://en.wikipedia.org/wiki/Living_document
[1]: http://semver.org/
[2]: http://callbackhell.com/
[3]: https://help.github.com/articles/dealing-with-line-endings/
[4]: http://browserify.org/
[5]: http://steve-yegge.blogspot.ca/2006/03/execution-in-kingdom-of-nouns.html
[6]: http://npmjs.org/
[7]: https://github.com/substack/covert
[8]: https://ci.testling.com/
