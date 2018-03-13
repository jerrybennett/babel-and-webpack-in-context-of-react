# Babel and Webpack

## Overview

In this lesson, we'll unpack what Babel and Webpack bring to the table when developing React applications.

## Objectives

1. Learn what Babel and Webpack are
2. Understand the role of Babel and Webpack in our React applications

## Babel
![Tower of Babel](http://www.ancient-origins.net/sites/default/files/field/image/tower-of-babel-2.jpg)

First things first: [the origin story][origin-myth]. If you don't have time to read a great story, and want to get on with learning programming, allow us to provide the [TL;DR][TL;DR] and why it is relevant to the Babel tool we use.

The Tower of Babel was a great tower, being built by a united humanity speaking the same language, with the intention of reaching such heights that heaven itself could be accessed. While it was being constructed, the God in the origin myth, (for debated reasons), smote the united humans by confounding their speech. This ensured they could no longer communicate.

Babel, ([the one we use][babel]), does the opposite of this God's smiting: it turns many languages/syntaxes/versions into one! As you may already know, JavaScript (based on the ECMAScript [ES] standard) is an evolving language. Over time we have had several iterations. For the most part, ECMAScript's evolution has changed to incorporate more features and language constructs over time (think ES6 arrow functions, class syntax, `let`, abd `const` vs. their absence in ES5!).

Babel originally gained popularity because it [compiled/transpiled][transpile-compile] newer ES6 syntax and language features into ES5 language features. This was especially important at the time because many browsers had not yet updated their JavaScript engines to interpret the new language features of ES6.

As of 2018, you are in _significantly_ less danger of your target browsers _not_ implementing ES6 syntax. For example, open up your browser's developer console and attempt to assign `let y = 4; console.log(y)`. If you are in Chrome, your Chrome Boi won't complain!

<p align="center">
  <img src='https://learn-verified.s3.amazonaws.com/chrome-boi-wont-complain.png' height=500 width=300/>
</p>

#### Then why is Babel important?

> "To Babel or not to Babel: ay, there's the rub"  
> &nbsp;&nbsp;&nbsp;&nbsp;&#8211; _William Shakespear's Hamlet, [direct quote][hamlet]_

Earlier on in the lesson, we talked about how we'd use something called [Babel][Babel] to compile our JSX down to browser-readable JavaScript. Babel also allows us to use new JS features before they are standardized and implemented in the browser, allowing us to write the most modern code we can, without worrying about browser support. Babel transpiles everything back down to JavaScript that _all_ browsers can understand. Neat!

In the last couple of labs we have been using `npm start` to run our code in the browser and `npm test` to run our tests. The commands have been running Webpack and Babel to transpile our code into readable JS for all browsers. If you take a look in the root directory you will see a `.babelrc` file. This contains the Babel plugins that we use to transpile our code.

[Webpack][Webpack] lets us require modules using Node's version of the CommonJS module system. This means that we can include modules in our file (both local files as well as `node_modules` installed with `npm`). When compiling a file with Webpack, it'll check every file for stuff that it needs to import, and also include that code. In more technical terms, it's traversing the dependency tree and inlining those dependencies in our script. What we'll end up with is one big JS file that includes _all_ of our code, including any dependencies (like `React`, or our own components) in that file too. That way, we also only need to have one script reference in our HTML: the bundled version!

One of the benefits of Webpack is that it wraps every module in an [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE), ensuring that no variable is global (unless you force it by setting something on `window`). This allows for pretty powerful modularization — we only export what we want other modules to use, and the rest is 'private' by default.

Webpack also lets us _transform_ the code we're bundling. That's where Babel comes in again: we'll use a Babel transform to compile our code down to readable JS that browsers understand.

## Writing Modular Code

Let's practice writing some modular code in our application.

Our `index.js` file is still empty at this point. Let's practice writing modular code by creating a new file in `/src/components/foo.js` (you'll also need to create the `/src/components/` directory). In that file, we'll add this content:

```js
export const message = "I am a component!";
```

We can import this component in our `index.js` by using `import` and referencing the origin file:

```js
import { message } from './components/foo';
```

Note that files are always referred to using a relative path (even if they are in the same directory). This way Node knows whether to look for a local module or one found in `node_modules`, or in the global modules. Adding the `.js`  extension is not required.

Back to the exporting stuff! Using CommonJS, we have two options of exporting things out of our files: either through named exports (exporting multiple things) or a default export (exporting one thing).


### Named exports
Named exports allow us to export several things at once. This is useful for utility modules or libraries. Exporting several things at once is done by exporting an object. Because we are exporting this object as default without a name, we can assign it any name when we import it (in this case "fruit").

```js
// In a file called `fruits.js`
export default {
  apple: 'red',
  banana: 'yellow',
};

// In a file in the same directory
import fruit from './fruits';
console.log(fruit.apple); // prints 'red'

// In another file, also in the same directory
import { apple } from './fruits';
console.log(apple); // prints 'red'
```

When using named exports, we can choose to either import the entire thing and then reference the keys on the exported object, or we can import one specific key.

### Default export
A default export means we're exporting just one thing. This is useful for exporting components in their own file, since there's only one thing there: the component itself. Exporting one thing only is done by exporting a reference to what we want to export. You can also inline the value of what you want to export.

```jsx
// In a file called `Tweet.js`
import React from 'react';

class Tweet extends React.Component {
  render() {
    return (
      <div className="tweet">
        <img src="http://twitter.com/some-avatar.png" className="tweet__avatar" />
        <div className="tweet__body">
          <p>We are writing this tweet in JSX. Holy moly!</p>  
        </div>
      </div>
    );
  }
}

export default Tweet;

// In a file in the same directory
import Tweet from './Tweet';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <Tweet />,
  document.getElementById('root')
);
```

You'll mostly be using this method. It's important to correctly export your components, otherwise the tests can't access the code you've written, causing them to fail!

## Future labs
It's very important to know how this stuff works on a high level, because most of the React code nowadays is being compiled in one way or another — be it using Webpack, Browserify or something else. However, we don't want to create unnecessary busywork for you. Every lab from now on already has the bundling stuff set up for you. You just need to run `npm start` to start the compiling process. This will watch your code anytime you save your code and reload your browser. That's it!

## Resources
- [Webpack]: http://webpack.github.io
- [Babel]: http://babeljs.io/
- [Babelify]: https://github.com/babel/babelify
- [JSX]: https://facebook.github.io/react/docs/jsx-in-depth.html

<p class='util--hide'>View <a href='https://learn.co/lessons/react-jsx'>JSX</a> on Learn.co and start learning to code for free.</p>

[origin-myth]: https://en.wikipedia.org/wiki/Tower_of_Babel
[TL;DR]: https://en.wikipedia.org/wiki/TL;DR
[babel]: http://babeljs.io/
[transpile-compile]: https://stackoverflow.com/questions/43968748/is-babel-a-compiler-or-transpiler
[chrome-boi]: https://learn-verified.s3.amazonaws.com/chrome-boi-wont-complain.png
[hamlet]: https://en.wikipedia.org/wiki/To_be,_or_not_to_be
