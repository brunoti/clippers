# JSDOM: How to Get Started
As a web developer, one of the thorniest issues I run into is [testing functionality in my web UI](https://www.testim.io/blog/ui-testing/). I understand the [importance of automated software testing](https://blog.thedigitalgroup.com/importance-of-automation-in-software-testing), but writing tests for a UI is difficult. For starters, parsing HTML your server generates is difficult. It is possible to run your tests in a system like Selenium, which automates browsers. An alternative is to use a tool like JSDOM.

**JSDOM is a library which parses and interacts with assembled HTML just like a browser.** The benefit is that it isn’t actually a browser. Instead, it implements web standards like browsers do.

You can feed it some HTML, and it will parse that HTML. Then, you can inspect or modify that HTML in-memory using the normal JavaScript [DOM API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model).

Because Selenium executes test code inside an actual browser, you need to provide an environment that can run a browser. This means full executable support in your [testing environment](https://www.testim.io/blog/test-environment-guide/). Your tests are slower, because they need to wait for a full application to start up.

If you’re running your tests inside a continuous integration environment, setting up something like Selenium is a significant time investment. What’s more, tiny changes in your environment can often break your build process, which slows down every developer on your team.

Facing these exact issues, a team of open-source software developers built the JSDOM library. In this post, we’ll talk about how to get started with JSDOM, and a few of the things that you can do with the library to make [automating your UI tests much easier](https://www.testim.io/blog/author-execute-automated-tests/).

## Expand Your Test Coverage

Fast and flexible authoring of AI-powered end-to-end tests — built for scale.

[Start Testing Free](https://bit.ly/386Q3KG)

## Installing JSDOM

As it turns out, installing JSDOM is very simple. It’s intended to be run using NodeJS, so as you’d expect, there is a [Node Package Manager](https://www.npmjs.com/) package for [JSDOM](https://www.npmjs.com/package/jsdom).  Installing JSDOM on your development machine is as simple as running **npm i jsdom**.

You’ll need to wait for a few moments while the code downloads and installs, but the hard part is done. Once NPM informs you that the installation completed, you’re ready to dive in!

## Your First DOM

Using JSDOM is very simple. JSDOM expects that you pass some valid HTML to its constructor. Then, it’ll parse that HTML just like a browser does. From there, you as a developer have an API which reads and changes the content of the in-memory DOM. Perhaps the simplest explanation is an example.

    const jsdom = require("jsdom");
    const dom = new JSDOM(`<!DOCTYPE html><body><p id="main">My First JSDOM!</p></body>`);
    // This prints "My First JSDOM!"
    console.log(dom.window.document.getElementById("main").textContent);

Let’s dive into just what this code does a little better.

#### Requiring JSDOM

First off, our code uses a [require statement](https://www.freecodecamp.org/news/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8/) to indicate to NodeJS that we need to use the JSDOM library in this script. If you haven’t installed JSDOM via NPM, this step will fail!

#### Initializing the JSDOM Object

Line 2 declares the **dom** variable and assigns it to the output of the JSDOM constructor. You’ll notice that we pass a string to the JSDOM constructor which contains some HTML. This is the HTML that will be loaded into JSDOM’s in-memory [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).

JSDOM is perfectly happy to work with valid HTML, but it’ll error if you pass it invalid HTML. If you’re using it for automated tests, make sure that your code is passing valid HTML to JSDOM before you start your tests.

One thing to note: JSDOM parses your HTML just like a browser would. That means that you can use any HTML element that’s valid in a browser. If you want to check that a script is properly including inside your head element, that’s perfectly possible.

Need to make sure that an iFrame loads properly? No problem. If you have some complicated HTML structure that depends on variable inputs, it’s easy to parse that output and make sure the file contains the right information. It’s also possible to load your HTML from a file, or by making a call to your server, or even an external server. If you’re doing that, there are a few things to remember which we’ll touch on later.

#### Reading the JSDOM Object

Line four of our example above contains the trickiest code, but also the most instructive. There are a couple of significant details here. All we’re trying to do is print out the text content of our paragraph element to the console. However, this is where most people new to JSDOM get stuck. Let’s break down each step of what the fourth line is doing.

The first property we call is **dom.window**. This call trips up a lot of new developers. The object returned from the constructor contains both the data about the parsed HTML document as well as metadata JSDOM uses to parse that document. To actually access the document, we need to read the window property, which is just like the window property from a browser.

From there, we read the document property, which brings us into the actual DOM, just like if we were working in JavaScript in a normal browser. From there, we call the [getElementById](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById) function. That returns an object representing the selected element.

After that, it’s simple: we read the **textContent** property from that object. Our **console.log** function prints that out to the command line, which is where we started the function. If the script runs correctly, you should see “My first JSDOM!” printed to the command line.

## More Advanced Usages

JSDOM is simple and easy to set up. However, most web applications aren’t so simple. If you’re trying to test more complicated pages, the constructor accepts a second parameter. That parameter is a JavaScript object with some specific properties that tell JSDOM how to behave.

We’ll look at a few of those and explain why they’re helpful.

#### runScripts

The most common JSDOM property is the runScripts property. Modern websites aren’t so simple that they just display HTML any more. Instead, they use JavaScript to modify the content of the webpage as the user interacts with it.

Thankfully, JSDOM provides a method to execute scripts inside your in-memory DOM. But, there’s a catch: you need to make sure you trust the scripts you’re running. Before you can execute scripts as part of JSDOM, you need to pass the **runScripts: ‘dangerously’** property to the constructor.

A word of warning: running scripts inside JSDOM is the same as downloading and running that code within your NodeJS environment. It’s not easy, but it is definitely possible for an attacker to break out of JSDOM via these scripts. If an attacker were to break out of JSDOM, they’d have access to anything your NodeJS environment has access to.

This can include things like server configuration files or application secrets. Make sure you trust the scripts you’re running before executing them inside JSDOM!

#### url

By default JSDOM thinks the base URL for your application is **about:blank**. This can cause all sorts of testing errors inside your application, if you’re testing e.g., the content of links to other pages. By passing the URL parameter to JSDOM, you tell it what the in-memory DOM should consider the base URL of your page. This property is a must-have for any serious testing.

#### contentType

Setting the contentType property for your website is critical if you’re not sending standard HTML. Many web applications don’t return HTML from specific endpoints, but provide XML instead. If your endpoint passes XML when JSDOM expects HTML, you’ll receive unexpected results.

The two options for this property are text/html and text/xml. If your web application returns XML and you’re using JSDOM to test it, this property is critical.

## Start Experimenting With JSDOM

The great part about JSDOM is that it’s really quite simple to use. The API it provides is the same API you work with in the browser, but it doesn’t have any of the difficulty of running a browser in your testing environment.

Because it’s so easy to set up and use, the best way to learn is to experiment. You’ve got a working script. Try changing the HTML you pass to the JSDOM constructor, and explore the API to interact with those pages. You’ll learn a lot just by tinkering with the code that we’ve already got running.

Once you’ve tinkered a bit with the running script, head over to the JSDOM GitHub [page](https://github.com/jsdom/jsdom). There is a lot more to learn there, and there’s a great community to help if you get stuck. If you start integrating JSDOM into your testing workflow, take those tests to the next level with a tool like [Testim](https://www.testim.io/).

JSDOM is a powerful library, but thankfully it’s very simple to use. In my experience, getting started is the hardest part. Now that you’re up and running, the best way to learn is by using it. Go out there and get started!

_This post was written by Eric Boersma._ [_Eric_](https://www.linkedin.com/in/eric-boersma/) _is a software developer and development manager who’s done everything from IT security in pharmaceuticals to writing intelligence software for the US government to building international development teams for non-profits. He loves to talk about the things he’s learned along the way, and he enjoys listening to and learning from others as well._
