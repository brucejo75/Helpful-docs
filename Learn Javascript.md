# Learning Javascript (ecmascript)
Javascript it is interchangeable with ecmascript, they are the same thing.  I will always refer to it as javascript, but in this document I will always refer to it as ecmascript (to drive home the point).

W3schools is kind of a classic training site: https://www.w3schools.com/js go there first

Seriously, just go do it, the rest of this may be better read after that...

Given that you know C, ecmascript will at first seem familiar.  But it gets weird. Worth taking a look at
this: https://www.geeksforgeeks.org/7-javascript-concepts-that-every-developer-must-know/ &
this: https://www.geeksforgeeks.org/objects-in-javascript/

And to be clear: ecmascript has no relation at all to Java.  None really.

For me the important points to ecmascript are:
1.	**Functions** are first class & callbacks are used constantly.  When you are first looking at code you will wonder why all these callback functions are being declared in place?  It really makes it disorienting. I found it useful for a while to think of ecmascript being more like lisp than C because of the way callback functions are defined everywhere. Now I hardly notice anymore.
2.	**Closure** is very powerful and can be used in many ways to solve some gnarly issues and simplify some interfaces.  It took a while for this lightbulb to go off for me, but when it did wow!  Here is a typical problem application: during initialization I have some state that is important for the inner loop.  But the inner loop does not have that information passed to it & ideally it should not have that information passed down. Using closure you can define pieces of inner loop code that have that initialization information embedded in them with out passing the state through the entire runtime chain.
3.	**Objects** are first class, super simple and very easy to extend.  It is done constantly.  Kind of the exact opposite of `Java` and very different than `C`.  ecmascript has incorporated class declarations into the language as well.  I have been using these for larger objects where I need methods. Everyday variable grouping is done with objects all the time. E.g. time.second, time.minute, time.hour vs timeSecond, timeMinute, timeHour. Very useful.
4.	**Truthy & Falsy**: unlike other languages you can Boolean test variables that are not Boolean.  For example, false, null, undefined, ‘’ – empty string, 0 all evaluate to false. Very handy, but you need to be aware of it. Very handy for initializing variables: “x = x || 10;” if x is already defined (but not 0) use it, if not set it to 10.
5.	**Modules**. For a long time there were no language modules defined for ecmascript. In this void the community started creating module standard using some language quirks. You will see references to CommonJS (CJS), Asynchronous Module Definition (AMD) or Universal Module Definition (UMD) modules.

    With es6 ecmascript finally gained their own ["esmodules"](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) (ESM).  Node started fully supporting ESM with v13.2.0.

    What this means is that there are a large number of packages (mostly older) that use: commonJS (CJS), Asynchronous Module Definition (AMD) or Universal Module Definition (UMD). So do not get caught in the trap of including code that uses one of these older module systems, pain in the butt.

You will read that list and say "ok, got it". After you start diving into coding you will likely not even consider these. But I suspect you will have similar lightbulb moments as you read/write more code.

## ecmascript versions
Here is a rundown of ecmascript versions:
https://www.w3schools.com/js/js_versions.asp, take a look at the [IE/Edge](https://www.w3schools.com/js/js_ie_edge.asp) & [history](https://www.w3schools.com/js/js_history.asp) sections to see how things move over time.

What happens is a feature is approved and then all the browser makers set about adding the feature (or they may have already speculatively added the feature in their non-standard api).  Then they release an element of the new spec in a certain version of their browser.

As a developer how can I know which parts of the language I can use?

### Method 1 Browser support
If all my users use a browser that supports the feature then I can probably use it...

This leads to tools like [caniuse.com](https://caniuse.com/).  So you can find which version of the browser supports a feature and how much of the web traffic appears to be on that browser.  For instance, I really want to use [optional chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining), I can see now that [icanuse](https://caniuse.com/?search=optional%20chaining) tells me that this feature is installed for 95% of users.  So I should start using it. 

### Method 2 Polyfills
[Pollyfills](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill) are the feature implemented via older versions of ecmascript. What? Essentially, instead of shippng the ecmascript down with the new feature (e.g. optional chaining `?.`), interpret the code and ship down old code that accomplishes the new feature.

There is an open source tool that most people use as part of their toolchain: [babel](https://babeljs.io/).  Mostly, you do not need to really be concious of this.

## Toolchains
How do I create the stuff that gets sent down to the browser? With the Meris system you will essentially get a full toolchain that you can install and "just use" without really needing to be aware of it at all.

I am describing toolchains just to be complete.

### Compilers
I thought ecmascript was interpretted? What compiler?

Here are some of the use cases for a compiler:
1. Enable the **latest language** features via babel. Compile the latest features to browser compatible ecmascript.
2. **Other languages!** [typescript](https://www.typescriptlang.org/) is a compiled to ecmascript. [coffescript](https://coffeescript.org/) was a popular alternative to ecmascript.  I do not like these because of their impacts on toolchains, it is yet another dependency that can cause grief, especially in libraries.
3. **Minifying**. A good minifier could take your ecmascript code as input and shrink it down to 10% of the size.  This makes a difference for loading over the network.
4. **Uglifying**. This will scramble the code to make it much less readable. Getting rid of all your variable names in favor of single letter variable names. A form of obfuscation also. Caveat: browser developer tools have gotten pretty good at formatting uglified code. Often uglifying is simply a side-effect of minifying.
5. **CSS**. CSS has improved lately but many [CSS preprocessors](https://developer.mozilla.org/en-US/docs/Glossary/CSS_preprocessor) are actively used to make CSS more programmable in its definition.

In the end, the output of the compiler is valid ecmascript.

### Bundlers
A bundler is like a linker. It pulls the ecmascript together in distinct packages to download. A typical option is to download the entire ecmascript bundle as one file. A bundler will also handle CSS & images as well. Bundlers can also ["tree shake"](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking).

I have been using [Vite](https://vitejs.dev/) which uses [Rollup](https://rollupjs.org/) for bundling, it is kind of the hot one now.  You will see references to other bundlers: [Webpack](https://webpack.js.org/) & [Parcel](https://parceljs.org/).

## Libraries
[npmjs.org](https://www.npmjs.org) is the The library of libraries. Npm is where you can find open source libraries. All libraries in the end ship valid ecmascript and as part of being in `npm` all the libraries expose their sourcecode on github.

- [jQuery](https://jquery.com/): A very popular one historically, but today most people recommend not using this in favor of vanilla browser apis.
- [moment](https://momentjs.com/): Time handling library, very handy. But there is a proposal in the works that will resolve some of the issues moment was designed to address: [Temporal](https://tc39.es/proposal-temporal/docs/).
- [lodash](https://lodash.com/): I use this library quite extensively. Lots of very useful capabilities. This is an upgrade from an older library [underscore](https://underscorejs.org/).
- [simpl-schema](https://www.npmjs.com/package/simpl-schema): schema checker for Mongo.

## Frameworks
Today, I am using [Meteor](https://docs.meteor.com/) in the backend (at its core it is really [Node](https://nodejs.org/en)) and a little bit in the front end. For Meris applet development you do not need to worry about this.

For developing the applets I am using [Svelte](https://svelte.dev) with the [Framework7](https://framework7.io/svelte/) user interface framework. This will be your sandbox for creating applets.

## Tools
### Debugging
I do all my client debugging in [Chrome Developer tools](https://developer.chrome.com/docs/devtools), super powerful stuff.  Firefox and Edge have their versions too. I might start debugging my client code in VSCode, the experience is quite good for JS debugging. I am curious about HTML debugging, network reports etc.

For server side debugging I have started using VSCode and it is a fantastic experience. I can stop the code, see what I am doing wrong, inspect variables live and then edit changes. Very quick turnaround compared to going back and forth between editor and debugger;
### Git
Understanding `git` is very important.  My take:
1. It seems like a mess when you first start using it. Lots of commands with lots of options.
2. I have been using [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) to help me visualze and interact with `git`.  Super useful.
2. Once you understand the guts of git the concepts get fairly simple and I have started to admire this repository manager.

### Github
Essentially, this is a repository for sharing your source code. The concept of a pull request is a very powerful organizing tool for handling community updates to code. Most all the meris repositories are private.

### VS code
[just use it](https://code.visualstudio.com/).

## Other tools
- [JSDoc](https://jsdoc.app/) - JavaScript Documentor: documentation with types in source code
- [ESLint](https://eslint.org/) - EcmaScript Linter (VSCode plugin too)
- [Prettier](https://prettier.io/) - code formatter (VSCode plugin too)
- [CoPilot](https://github.com/features/copilot) - OMG, fantastic. (VSCode plugin)
- [caddy](https://caddyserver.com/) - ReverseProxy & static file server. Not necessary with Meteor but it takes care of all static requests. Can verify all source requests and provide fine grained control over headers.
- [mongodb](https://www.mongodb.com/)
- [meteor](https://docs.meteor.com/)
