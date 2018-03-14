# Babel and Webpack

## Overview

In this lesson, we'll unpack what **Babel** and **Webpack** bring to the table when developing React applications.

## Objectives

1. Learn what Babel is
2. Take a stretch break
3. Learn what Webpack is
4. Understand the role of Babel and Webpack in our React applications
5. Frame Babel and Webpack's relative importance at this stage in learning React

# Babel
![Tower of Babel](http://www.ancient-origins.net/sites/default/files/field/image/tower-of-babel-2.jpg)

First things first: [the origin story][origin-myth]. If you don't have time to read a great story, and want to get on with learning programming, allow us to provide the [TL;DR][TL;DR] and why it is relevant to the Babel tool we use:

The Tower of Babel was a colossal tower project long ago. It was under construction by a united humanity speaking the same language, with the intention of reaching such heights that heaven itself could be accessed. While it was being constructed, the God in the origin myth, (for debated reasons), smote the united humans by confounding their speech. This ensured they could no longer communicate. Before you have any epiphanies on us, this is most likely _not_ where the term 'babbling' comes from.

Babel, ([the one we use][babel]), seeks to do the opposite of the aforementioned God's smiting: it turns many languages/syntaxes/versions into one! As you may already know, JavaScript (based on the ECMAScript [ES] standard) is an evolving language. Over time we have had several iterations. For the most part, ECMAScript's evolution has changed to incorporate more features and language constructs over time (think ES6 arrow functions, class syntax, `let`, abd `const` vs. their absence in ES5!).

Babel originally gained popularity because it [compiled/transpiled][transpile-compile] newer ES6 syntax and language features into the older ES5. This was especially important when ES6 came out because many browsers had not yet updated their JavaScript engines to interpret the new language features of ES6.

As of 2018, you are in less danger of your target browsers _not_ implementing ES6 syntax. For example, open up your browser's developer console and attempt to assign `let y = 4; console.log(y)`. Better believe Chrome Boi won't complain!

<p align="center">
  <img src='https://learn-verified.s3.amazonaws.com/chrome-boi-wont-complain.png' height=500 width=300/>
</p>

#### Then why is Babel important?

> "To Babel or not to Babel: ay, there's the rub"  
> &nbsp;&nbsp;&nbsp;&nbsp;&#8211; _William Shakespear's Hamlet, [direct quote][hamlet]_

Even though most major browsers are up to date with the new ES6 features, Babel is still invaluable when creating web application, and especially React applications. Babel turns JSX into React function calls. That is, **Babel turns JSX into normal JavaScript written with the React library**. Enough theory! Let's see it in action:

```JavaScript
var profile = (
  <div>
    <img src="avatar.png" className="profile" />
    <h3>{[user.firstName, user.lastName].join(' ')}</h3>
  </div>;  
)
```
...when the above is run through Babel, we receive:
```JavaScript
var profile = (
  React.createElement("div", null,
  React.createElement("img", { src: "avatar.png", className: "profile" }),
  React.createElement("h3", null, [user.firstName, user.lastName].join(" ")))
);
```

While you don't need Babel as a dependency when writing React code, it means you have to write in the non-JSX syntax seen in the output above. For now, we will be teaching and writing with Babel compiled JSX in our React applications.

#### Not Just For JSX

In addition to the JSX magic it provides, Babel can also compile other features and syntactical sugar that is not yet, or never will be, a part of ECMAScript! One example of this is a babel plugin that enables the usage of [language features in ES7][babel-es7] (or proposed to be in ES7) that have not even been officially included!

#### We must go deeper!

No way! _we mustn't_! _we shouldn't_! _we couldn't!_ But If you _need_ know more about how React incorporates Babel, feel free to run [`npm run eject`][eject] _at your own risk_ in this repository. For the most part, we will be using the handy `create-react-app` when initializing new React projects, which obfuscates pre-configured files from us for user-friendliness and to avoid boiler-plate code. `npm run eject` will undo that obfuscation, and expose you to these pre-built configurations (read: "world of hurt"). For now, we recommend staying focused on improving your high-level React development skills, and leaving the general project configuration up to React.

<p align="center">
  <img src='https://learn-verified.s3.amazonaws.com/npm-run-eject.jpeg' height=500 width=300/>
</p>

---

That was quite a bit of information, and we have even more coming up. It is expected that this isn't falling into place immediately. If this is your first time being exposed to a tool like Babel, treat yourself. Stand up, stretch your legs, look at a real human that's not in meme format. Treat yourself: you deserve it. When you come back, we will get started on **Webpack**.

---

# Webpack

Welcome back! If you didn't take a break, shame on you.

In the last couple of labs we have been using `npm start` to run our code in the browser and `npm test` to run our tests. The commands have been running Babel and **Webpack** to transpile our code into executable JS for all browsers.

[Webpack][Webpack] lets us require modules using Node's version of the CommonJS module system. This means that we can include modules in our JavaScript files (both local files as well as `node_modules` installed with `npm`). When compiling a React application with Webpack, it'll check every file for dependencies that it needs to import, and also include that code. In more technical terms, it's traversing the dependency tree and inlining those dependencies in our script. What we'll end up with is one big JS file that includes _all_ of our code, including any dependencies (like `React`, or our own components) in that file too. The convenience of this is not to be underestimated: one file, with _all_ of our code, means we only need to transfer a single thing to our clients when they ask for our React applications!

Enough theory, let's take a look at a rudimentary example of how Webpack does this. Let's assume we have the following application on our server that we want to share with the world:

### Simplified Webpack example

The files we want our client to have, which constitute one whole dank web application:

```JavaScript
// reveal.js (pre Webpack digestion)
function reveal(person, realIdentity) {
  person.identity = realIdentity
}

export default reveal
```
```JavaScript
// main.js (pre Webpack digestion)
import reveal from './reveal.js'

const gutMensch = {
  name: "Andrew Cohn",
  identity: "Friendly Neighborhood Flatiron Teacher",
}

reveal(gutMensch, "Chrome Boi")
```

Without Webpack, we would need to find some way to send both files to our client and ensure they are playing  nicely together. We couldn't just send the `main.js` file wizzing over the internet, through a [series of tubes][tubes], to our client expecting it to make use of the `reveal` function: the client hasn't even received the `reveal.js` file in this case! While we have several ways we could make this work, most of them are headaches and someone else has already made an excellent solution: Webpack.

Instead of writing our own bespoke, artisanal, Etsy&trade; sell-able dependency solution, we can just use Webpack!


**The result after we unleash Webpack on these files:**

```JavaScript
// bundle.js (post Webpack digestion)
function reveal(person, realIdentity) {
  person.identity = realIdentity
}

const gutMensch = {
  name: "Andrew Cohn",
  identity: "Friendly Neighborhood Flatiron Teacher",
}

reveal(gutMensch, "Chrome Boi")
```

If we were to first pre-digest our files with Webpack, we would instead have a single, all-encompassing, file that ensures our dependencies are right where they belong.

TODO: removed the code along/named export part that was here (see curriculum/react-jsx repo). Check lab and put there? Don't want to reincorporate: long enough as it is.

## Summary

You have just read a lot of information about two tools you likely have not worked directly with before. Luckily, they are easy to summarize:

**Babel** enables us to use syntax that browsers won't natively recognize by **pre-compiling** it into syntax that browsers _do_ natively recognize. When used with React, this will include JSX.

In React, **Webpack** manages pesky dependency loading for us by **pre-digesting** our many files code and outputting a single 'bundle', which contains all of our code, with dependencies properly placed, in one file.

## Looking Forward
You may, understandably, be asking yourself:
  - "How important is this Webpack/Babel jargon?"
  - "How much do I need to learn about the different tools that improve React development experience vs. actual React programming?"
  - "_Why_ on _Earth_ did I _ever_ `npm run eject`?"
  - "Why didn't I `npm run eject`? Isn't that devil `create-react-app` hiding information from me?
  - Screw it. All systems go: `npm run eject` + `enter`

Because `create-react-app` certainly isn't transparent with you, allow us to [open the kimono][open-kimono-not-offensive], so to speak.

At Flatiron, we are constantly balancing an explanation of the fundamentals against practice on the real skills that will get you producing valuable applications the quickest. We believe that, while learning React basics, it's important to know how these tools (Webpack + Babel) work on a _high level_. Let's justify both in turn:

Most React code nowadays is being compiled one way or another — be it using **Webpack**, an alternative such as [Browserify][browserify], or something else. We want to use it, but we don't want to create unnecessary busywork for ourselves or distract with peripherals.

Additionally, there are a lot of juicy nectarines (read: low hanging fruit) that aren't present in the ES6 standard, i.e.: JSX, ES7 features, etc., which we can pluck with **Babel**. Don't you want to sink your teeth into those [syntactic sugary][syntactic-sugar] stone fruits?

Now I've had my share of technical writing for the moment, and I reckon you have had your share of technical reading. Let's end on a true story about you:

> You are stuck on an island the natives call: Eyewanam&auml;keawesumthingsinreeakt. The island is some kilometres from the nearest coastal town, which the natives revere as the land of Dhankahps, (which you have a vested interest in going to).

>The natives want to help you on your way. While they know you could swim there with some effort, they could also put you in a hollowed out tree-trunk rowboat, give you an ore, and send you on your way. They see the value in the rowboat strategy: you will arrive at shore having learned a lot about the difficulties of open sea travel (not to mention, you will be totally [swol][swol] when you show up. The residents of Dhankahps love swol seafarers).

> Regardless, the natives have hatched a better plan and are more excited about the prospect of equipping you with the quickest, most relevant boat. They give each other toothy smiles as they use logs to roll their [hydrofoil][hydrofoil] machine down to the shore. They laugh with you and slap your back with their rough, machine-shop hands imagining you blasting into Dhankahps, enthralling the locals with your speed on the sea (read: deployment speed). Before you go they proudly initiate you as one of their own, always with a home on the island, and gather at the beach to see you off. As your sail catches wind and you are making it out of the breakers into the open water, you reflect:

> No, you won't know all of the ins and outs of hydrodynamics (the boat manages that for you).  

> No, you didn't get there on an Etsy&trade; guaranteed artisan boat (you want to get to Dhankahps, afterall, not make boats for others to get there).  

> What you did do is fly, (literally, [the things freaking fly][they-fly]), into the Sun's set, learning the peripherals on the open ocean.

TL;DR: Every lab from now on already has the bundling stuff set up for you. You just need to run `npm start` to complete the Babel compiling, Webpack bundling, process. This will also watch your code anytime you save your code and re-push it to your browser (a.k.a. hot-reload).



## Resources
- [Webpack]: https://webpack.js.org/
- [Babel]: http://babeljs.io/

<p class='util--hide'>View <a href='https://learn.co/lessons/react-jsx'>JSX</a> on Learn.co and start learning to code for free.</p>

[origin-myth]: https://en.wikipedia.org/wiki/Tower_of_Babel
[TL;DR]: https://en.wikipedia.org/wiki/TL;DR
[babel]: http://babeljs.io/
[transpile-compile]: https://stackoverflow.com/questions/43968748/is-babel-a-compiler-or-transpiler
[chrome-boi]: https://learn-verified.s3.amazonaws.com/chrome-boi-wont-complain.png
[hamlet]: https://en.wikipedia.org/wiki/To_be,_or_not_to_be#Text
[babel-es7]: https://babeljs.io/docs/plugins/preset-es2017/
[eject]: https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#npm-run-eject
[webpack]: https://webpack.js.org/
[tubes]: https://en.wikipedia.org/wiki/Series_of_tubes
[pied-piper]: http://www.piedpiper.com/
[open-kimono-not-offensive]: https://www.investopedia.com/terms/o/open-kimono.asp
[browserify]: http://browserify.org/
[syntactic-sugar]: https://en.wikipedia.org/wiki/Syntactic_sugar
[swol]: https://scontent.cdninstagram.com/t51.2885-15/s640x640/sh0.08/e35/13109122_818162874981972_854250567_n.jpg?ig_cache_key=MTI0MDEwMTQwNDQ5MDUyOTM2MQ%3D%3D.2.l
[hydrofoil]:https://www.google.com/search?q=hydrofoil+catamaran&source=lnms&tbm=isch&sa=X&ved=0ahUKEwia5Yyls-rZAhWIjVkKHdd-A3MQ_AUICygC&biw=1280&bih=659#imgrc=JhI18wkkvwakwM:
[they-fly]:https://www.youtube.com/watch?v=a49jy9ba4FQ&t=06m
