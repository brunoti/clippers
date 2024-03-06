# Why would anyone need JavaScript generator functions?
Generators are an _odd_ part of the JavaScript language. And some people find them a bit of a puzzle. You might be a successful developer for decades and never feel the need to reach for them. Which raises the question, if you can go so long without ever needing them, what are they good for?

Generators have a funny syntax, too. They have these strange starred function definitions; you can’t define them with arrow functions; they add this mysterious `yield` keyword. If you’re not familiar with what they’re doing, they can make code impossible to read.

Part of the trouble is that generators are a low-level construct. That is, they’re kind of like a tool for building tools. And it’s those tools we build that solve day-to-day problems. But if you’re looking at generator functions in isolation, it can be hard to see why you’d ever want them.

Generators are quite a powerful construct, though. And they’re handy to have in your metaphorical toolbox. Like most tools, you might not need them for every single job. But for certain jobs, they make life much easier. Once you understand the tool better, you begin to see where they’re helpful. And, equally important, you begin to see when _not_ to use them.

What, then, are generators good for?

## Tim Tams and lazy iterators

Generators have _lots_ of uses. But the most immediate and obvious application is to make _lazy iterators_.

Now, I’m hoping you’re already familiar with JavaScript’s [iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols). These protocols are another low-level language feature. They let us tell the JavaScript engine that some object can be used in a `for...of` loop or with [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).

The simplest thing we can do is define a generator function that yields some values. We can then use a `for...of` loop to iterate over them:

    function* culturalAchievements() {
        yield 'Amazing coffee';
        yield 'The Sydney Opera House';
        yield 'The invention of Wi-Fi';
    }

    for (achievement of culturalAchievements()) {
        console.log(`Australia is known for: ${achievement}`);
    } 

And on the surface, that looks pointless. You could do all that with a lot less rigmarole using arrays. But, ironically, it’s generator’s similarity to arrays that makes them so useful.

To explain why I must introduce you to the pinnacle of Australia’s cultural achievement. And no, it’s not the invention of Wi-Fi. Nor is it the Sydney Opera House. And it’s not even the superlative coffee.[1](#fn:1 "see footnote") Arguably, Australia’s greatest cultural achievement is the [_Tim Tam_](https://en.wikipedia.org/wiki/Tim_Tam).

![](https://jrsinclair.com/assets/tim-tams.jpeg)

A plate of Tim Tams, with one in the centre broken open to show the biscuit and chocolate cream filling. [Photograph by Bilby](https://commons.wikimedia.org/wiki/File:Tim_Tams.jpg). Licensed under the [Creative Commons Attribution-Share Alike 3.0](https://creativecommons.org/licenses/by-sa/3.0/deed.en) license.

A Tim Tam “consists of two malted biscuits separated by a light hard chocolate cream filling and coated in a thin layer of textured chocolate.”[2](#fn:2 "see footnote") These “creamy bricks of chocolatey goodness”[3](#fn:3 "see footnote") will serve as an analogy for us. A Tim Tam represents an item of data that we wish to process.

And we ‘process’ a Tim Tam through a ritual known as the [_Tim Tam Slam_](https://firstwefeast.com/eat/2014/10/how-to-do-a-tim-tam-slam). The steps are as follows:

1.  Select a single Tim Tam.
2.  Bite a small chunk from one corner, 2–5 mm from the apex.
3.  Repeat the bite on the diagonally opposite corner.
4.  Insert one of the bitten corners into a hot beverage. ([Milo](https://www.milo.com.au/) is traditional, but coffee, tea, or hot chocolate is also acceptable).
5.  Place your lips over the opposite corner, and draw liquid through the Tim Tam as if it were a straw.
6.  As soon as liquid enters your mouth, immediately consume the entire Tim Tam. It’s important to do this quickly before it loses its structural integrity.
7.  Repeat until there are no more Tim Tams, or you feel physically ill.

Here’s Australian singer Natalie Imbruglia demonstrating on the _Graham Norton Show_:

Some people might find the Australian and British accents difficult to understand. If that’s the case for you, [Neil deGrasse Tyson breaks it down in a more recent YouTube video](https://www.youtube.com/watch?v=lSnD5vs_35A).

Now, suppose we can consume at most five Tim Tams before starting to feel ill. If we were to represent this process using JavaScript, it might look like the following:

    const MAX_TIMTAMS = 5;

    function slamTimTamsArray(timtams) {
        return timtams
            .map(biteArbitraryCorner)
            .map(biteOppositeCorner)
            .map(insertInBeverage)
            .map(drawLiquid)
            .map(insertIntoMouth)
            .slice(0, MAX_TIMTAMS);
    } 

We have described the process of the Tim Tam Slam as a logical sequence of steps. And this is a good thing. But, there are serious problems with how we’ve written this function. A standard packet of original Tim Tams contains eleven biscuits. If we processed the packet as if it were an array, we’d take out each biscuit, bite a corner off, and place it in a new packet. Then we’d take each biscuit in turn and bite off the diagonally opposite corner. And we’d have a packet of eleven biscuits with two corners bitten off. But it all becomes ridiculous when we attempt the next step. That would be where we insert eleven biscuits into our beverage.

Even if we were to rearrange the order so that we ran the `.slice()` operation first, we’d still have trouble. We’d end up with five biscuits in our beverage at once. We could also compose all the functions from the `.map()` operations together (using [`flow()`](https://jrsinclair.com/articles/2022/javascript-function-composition-whats-the-big-deal/#flow)). Combining that with moving `.slice()` to the start would improve the situation.

    const flow = (...fns) => x0 => fns.reduce(
        (x, f) => f(x),
        x0
    );

    function slamTimTamsUsingFlow(timtams) {
        return timtams
            .slice(0, MAX_TIMTAMS)
            .map(flow(
                biteArbitraryCorner,
                biteOppositeCorner,
                insertInBeverage,
                drawLiquid,
                insertIntoMouth));
    } 

This approach isn’t _too_ bad. But it’s not as clean and clear as our first attempt. But generators, being lazy, offer us a way to keep that neat sequencing from the first example. To make it work, though, we need to define some utility functions. First, we’ll define a counterpart for Array’s `.map()` method. It will look like this:

    const map = f => function*(iterable) {
      for (let x of iterable) yield f(x);
    }; 

Note the `function*()` syntax. It means that this `map()` function will return a generator. And the generator implements the [iterable protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol). This means that we can take that return value and use it in another `for...of` structure. And we can create another function that will limit the number of items:

    const take = n => function*(iterable) {
      let i = 0;
      for (let x of iterable) {
        if (i >= n) return;
        yield x;
        i++;
      }
    }; 

With these two utility functions, we can attempt to solve our problem. (With perhaps a little help from [`pipe()`](https://jrsinclair.com/articles/2022/javascript-function-composition-whats-the-big-deal/#pipe)):

    const pipe = (x0, ...fns) => fns.reduce(
        (x, f) => f(x),
        x0
    );

    function slamTimTamsWithGenerators(timtams) {
        return pipe(timtams,
            map(biteArbitraryCorner),
            map(biteOppositeCorner),
            map(insertInBeverage),
            map(drawLiquid),
            map(insertIntoMouth),
            take(MAX_TIMTAMS)
        );
    } 

We’ve now achieved a nice, neat description of the process, much like our array methods version. Because generators are lazy, we won’t end up with five biscuits in a mug. Except, there is a problem with this function. In its current form, running `slamTimTamsWithGenerators()` won’t do anything. This is because generators are so lazy, they won’t do _anything_. That is, unless we take some action to pull values out. There are two common ways to do this:

1.  Convert the generator object into an array using spread syntax; or
2.  Iterate over the generator _without_ yielding any values.

If we take the second approach, we can write a `forEach()` utility, much like the array method `.forEach()`. It might look like so:

    const forEach = effect => function(iterator) {
      for (let x of iterator) {
        effect(x);
      }
    } 

Notice that there’s no asterisk or `yield` keyword in this utility. It just takes an effect and runs it on each value from the iterator. With that in place, if we add another imaginary function, `eat()`, to our pipeline, we can make use of our new utility:

    function slamTimTamsAndEat(timtams) {
        return pipe(timtams,
            map(biteArbitraryCorner),
            map(biteOppositeCorner),
            map(insertInBeverage),
            map(drawLiquid),
            map(insertIntoMouth),
            take(MAX_TIMTAMS),
            forEach(eat)
        );
    } 

And now our function will slam five Tim Tams in the correct order. But it will still limit processing to five (that is, `MAX_TIMTAMS`).

This feature of ‘laziness’ is the first thing that makes generator functions useful. It allows us to change the way in which we process data. We can, for example, process large data sets by loading one item at a time into memory. And, on its own, this is enough to make generators interesting. But laziness has other advantages.

## Infinite Iterators

Back in the 1990s, Arnotts ran a series of commercials for Tim Tams. The first of these featured Cate Blanchett meeting a genie. And involved her character wishing for a packet of Tim Tams that never runs out.

The [whole](https://www.youtube.com/watch?v=wz3hHxgDAnY) [series](https://www.youtube.com/watch?v=-p7thvWWLL0) of ads focussed around the concept of an infinite packet of Tim Tams. And, much like a packet of Tim Tams that never runs out, generators can create infinite iterators.

We could, for example, create a generator function that gives us an infinite sequence of ones:

    function* allTheOnes() {
        while (true) { 
            yield 1;
        }
    }

    console.log([...take(7)(allTheOnes())]); 

Now, that might be interesting. But perhaps not so useful. We could, though, make this a little more general by specifying the value we want to repeat:

    function* repeat(val) {
        while (true) {
            yield val;
        }
    }

    console.log([...take(3)(repeat('Tim Tam'))]); 

Still, that may not seem so useful. But we could use this to build other sequences. Suppose we defined a function called `scanl()`. It works a little bit like Array’s `.reduce()` method. But, instead of returning a single value, it yields a sequence of values.

    function* scanl(reducer, initalVal, iterator) {
      let b = initalVal;
      yield b;
      for (const x of iterator) {
        b = reducer(b, x);
        yield b;
      }
    } 

And using `scanl()`, we could produce the sequence of natural numbers:

    const add = (a, b) => a + b;
    const genNat = pipe(
        repeat(1),
        (ones) => scanl(add, 0, ones)
    ); 

Although to be fair, it’s easier to write:

    function* genNat() {
      for (let i = 0; true; i++) yield i;
    } 

The point here isn’t to show you the most effective way to generate a lazy list of positive integers. It’s that tools like `repeat()` and `scanl()` allow us to build complex infinite sequences out of simple ones.

I’ll admit, though, generating an infinite list of positive integers isn’t all that interesting. Let’s try something genuinely useful. For example, something that’s useful for cryptography or creating the appearance of randomness. We’ll generate a sequence of prime numbers.

To achieve this, we need two more helper functions. First, we will want to filter a generated sequence:

    function* filter(p, xs) {
      for (const x of xs) if (p(x)) yield x;
    } 

And our second utility function, we’ll call `pop()`:

    const pop = (iterator) => {
      const result = iterator.next();
      if (result.done) return;
      return [result.value, iterator];
    } 

This `pop()` function reveals a weakness of generators (and iterators in general). The weakness is that they are mutable. Popping one item from the sequence means we can’t get that original sequence anymore. This is something to be careful of as you’re working with generators. For now, though, we have these two helper functions, so we can put our prime number generator together. And we’ll do it using a technique called [_the Sieve of Eratosthenes_](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). The algorithm works as follows:

1.  Start with a list of all natural numbers, beginning from 2.
2.  Take the first element in the list, then remove all multiples of that number from the list.
3.  Go to step 2, and repeat with the next number in the list.

To make this work in JavaScript, we create two functions. One to do the work of sieving through the list of numbers, and another that kicks everything off. Our sieving function looks like so:

    function* sieve(nums) {
      const result = pop(nums);
      if (!result) return;
      const [n, rest] = result;
      yield n;
      yield* sieve(filter(x => ((x % n) !== 0), rest));
    } 

Note that `yield*` keyword there. If we have an iterable object, `yield*` says ‘yield everything from this other iterable.’ This allows us to do something much like `.flatMap()` for arrays.

Now, with our sieve function ready to go, we need to kick it off with the list of all natural numbers starting from two. We can generate that with help from one more utility function:

    function* drop(n, iterable) {
      let i = 0;
      for (const val of iterable) {
        if (i >= n) yield val;
        else i++;
      }
    } 

And we can now put it all together:

    const primes = () => sieve(drop(2, genNat()));
    console.log([...take(9)(primes())]); 

Of course, there are other, more efficient ways to generate prime numbers (probably). But the point here is not about prime numbers. Rather, it’s to show how infinite sequences allow us to think differently about a problem. We can also use laziness and infinite sequences for applications like:

-   Generating a series of unique identifiers;
-   Generating all the possible moves in a game; or
-   Seeking a particular value (or values) amongst a bunch of permutations and combinations.

## Why aren’t all these utility functions built-in?

Through all the examples so far, we’ve been writing our own little utility functions. These include functions like `map()`, `take()`, `forEach()`, `filter()`, `scanl()`, and `drop()`. And it _is_ a little annoying that generators don’t have built-in methods like Arrays do. If you feel that way, you’re not the only one. That’s why there’s a stage 2 TC39 proposal to add [Iterator Helpers](https://www.proposals.es/proposals/Iterator%20helpers) to the ECMAScript standard.

In the meantime, if you’d rather not write helpers yourself, you can find libraries that will help. Two popular examples include:

-   [Itertools](https://www.npmjs.com/package/itertools) (based on a popular Python library); and
-   [IxJS](https://github.com/ReactiveX/IxJS) (like RxJS, but for iterables).

If you’re after something more lightweight, you can take a look at my own personal toolkit, [Dynamo](https://codesandbox.io/s/dynamonoodling-on-generators-xwidr?file=/src/Dynamo.ts). A word of warning though, it’s written in TypeScript rather than the usual plain JavaScript I use here.

## Message passing

The laziness of generators is fascinating and useful. But that’s not all generators can do. Generators also allow us to pass messages in two directions between a pair of functions. Now, to be fair, functions can do this already. We pass a message to a _called_ function by giving it parameters. And then the _calling_ function receives a message back via the return value. But it’s a one-shot thing. We get one message, one way, at the start. And one message, the other way, at the end. But generators allow us to send lots of messages back and forth.

The most commonly cited example of this is emulating `async`/`await` syntax. For example, suppose we’re writing some Node code. The function we’re writing needs to:

1.  Read config from a file;
2.  Make a fetch call to get an auth token; and
3.  Make another fetch call with the token to get some data.

Using `async`/`await`, it might look like so:

    const getData = async () => {
      const cfg = await readConfig();
      const authDetails = extractAuthDetails(cfg);
      const token = await fetchAuthToken(authDetails);
      const data = await secureFetch(DATA_URL, token);
      return data;
    }; 

But, before we had `async`/`await`, we could do something like this using generators:

    const getData = asyncDo(function*() {
      const cfg = yield readConfig();
      const authDetails = extractAuthDetails(cfg);
      const token = yield fetchAuthToken(authDetails);
      const data = yield secureFetch(DATA_URL, token);
      return data;
    });

    const promiseForData = getData(); 

And that looks _rather_ like the `async`/`await` version, wouldn’t you say? But, we need to do a bit of extra plumbing to make it work. And that extra plumbing looks like so:

    const asyncDo = (runTasks) => (...args) => {
      
      
      
      const generator = runTasks(...args);
      
      
      const resolve = (next) => {
        
        
        
        if (next.done) return Promise.resolve(next.value);
        
        
        
        
        return next.value.then(data => resolve(generator.next(data)));
      }
      
      return resolve(generator.next());
    }; 

This is neat. But perhaps not so useful. Most of us won’t need to emulate `async`/`await`. But, that said, perhaps you use Babel (or similar) to make your code work with older browsers. If that’s the case, transpiler will use generators like this to make `async`/`await` work for you. And, because generators are so low-level, we can tinker with this `yield` pattern in ways we can’t with `.next()`. This allows authors to create interesting libraries that go beyond `async`/`await`. For example, Kyle Simpson’s [_Cancellable Async Flows (CAF)_](https://github.com/getify/CAF):

> CAF \[…] is a wrapper for `function*` generators that treats them like async functions, but with support for external cancellation via tokens. In this way, you can express flows of synchronous-looking asynchronous logic that are still cancelable (Cancelable Async Flows).

Still, you may not need cancellable asynchronous processing (yet). Even so, there’s more to generator message passing than working with promises. We can, for example, use them to simplify error handling.

### Message passing to abstract away error handling

I’ve written before about [handling errors using Either](https://jrsinclair.com/articles/2019/elegant-error-handling-with-the-js-either-monad/). The Either structure lets us represent the result of an operation that might fail. By convention, we use a class called `Left` to represent a failure. And we use a class called `Right` to represent success. Here’s a simplified version of the two Either classes:

    class Left {
      constructor(val) { this._val = val; }
      map() { return this; }
      flatMap() { return this; }
      isLeft() { return true; }
    }

    class Right {
      constructor(val) { this._val = val; }
      map(fn) { return new Right(fn(this._val)); }
      flatMap(fn) { return fn(this._val); }
      isLeft() { return false; }
    }

    const right = x => new Right(x);
    const left = x => new Left(x); 

Now, in that article, we ran through an example of parsing a CSV file. And we came up with a function like this for parsing a single row:

    function processRowChained(headers, row) {
      return right(row)
        .map(splitFields)
        .flatMap(zipRow(headers))
        .flatMap(addDateStr);
    } 

This is a function that returns an Either. But with generators and message passing, we can use `yield` to paper over the Eithers a little. It looks something like the following:

    const processRow = eitherDo(function* (headers, row) {
      const fieldData = splitFields(row);
      const rowObj = yield zipRow(headers)(fieldData);
      const withDateStr = yield addDateStr(rowObj);
      return withDateStr;
    }); 

In this case, we expect the generator function to always yield Eithers. And the `eitherDo` function works like so:

     const eitherDo = (genFn) => (...args) => {
      
      
      
      const generator = genFn(...args);
      
      
      let next = generator.next();

      
      
      do {
        
        
        if (next.done) return right(next.value);

        
        
        if (next.value.isLeft()) return next.value;

        
        
        
        next = generator.next(next.value._val);
      } while (!next.done);
    } 

At the end of everything, we still get an Either value back from `processRow()`. And it’s doing the same thing as `processRowChained()`. But this style of writing the code may feel more comfortable for some. Especially if they’re not familiar with chained method calls.

## Wrapping up

If you’re not familiar with Generators, they may seem a little weird at first. And because they’re so a low-level language, we can use them for _lots_ of different applications. And that can make it difficult to see what you might want them for, day-to-day. But, as we’ve seen, they’re useful for tasks like:

-   Efficiently processing large data sets;
-   Working with infinite sequences; and
-   Passing messages between two functions.

Now, you may never feel the need to reach for generators. The kind of work you’re doing might not suit them. But, it’s still handy to have them in the toolbox, just in case. And if you start looking, you’ll find generators working away behind the scenes in lots of places. And now and then, you might need to dig into some library code to work out what it’s doing. In those cases, it’s good to have an idea of how they work.

* * *

**P.S.:** If looking at code in different ways interests you. Or, if you find yourself needing to adjust your coding style to meet your team’s expectations…. I’ve written something I think you’ll like. It’s called [_A Skeptic’s Guide to Functional Programming with JavaScript_](https://jrsinclair.com/skeptics-guide).
