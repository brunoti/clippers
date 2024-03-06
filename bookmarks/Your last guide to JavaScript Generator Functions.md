# Your last guide to JavaScript Generator Functions
Generator functions are arguably the most confusing topic in JavaScript.

You may have an idea of how they work, but you may not know if there is a real use case for it.

Interestingly, we use generator functions at work. They don't exist for no reason. They have a purpose.

I decided to take a deep dive into generator functions since I found them confusing at times.

This guide will take you from Zero to Hero.

In the end, we'll go over real-world scenarios where you may want to use generator functions.

[Permalink](#heading-what-are-they "Permalink")What are they?
-------------------------------------------------------------

In JavaScript, functions run and finish, giving you the result at the end. But what if you wanted a function to give you partial results in the middle, pause, and then continue later?

Imagine if you were forced to watch every movie in a single sitting. You're not allowed to pause the movie to get some more snacks to eventually resume it once you're back.

Well, that wouldn't be fun. In JavaScript, generator functions let you pause and resume the execution of a function.

This makes them incredibly powerful for tasks that don't finish in one go. It could be e.g. tasks that are too large to process in one go.

To demonstrate visually real quick:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693205777122/92235a89-6def-4f44-9e9d-4eb386f984e1.png?auto=compress,format&format=webp)

[Permalink](#heading-definition "Permalink")Definition
------------------------------------------------------

Generator functions are defined with a function keyword followed by an asterisk (e.g., `function*`) and use the `yield` keyword to yield values.

[Permalink](#heading-basic "Permalink")Basic
--------------------------------------------

A generator returns a generator object. It lets you resume the function by calling `next` which will yield the next value.

When calling `next()` , an object is returned. The object gives you both the value and tells you whether the generator is done running or not.

The powerful thing here is we don't have to call `next()` on the next line of code after creating the generator. We can do so much later or in a completely different place in the codebase.

```
function* simpleGenerator() {
  yield 1;
  yield 2;
  yield 3;
}


const gen = simpleGenerator();

console.log(gen.next()); 
console.log(gen.next().value); 

console.log(gen.next()); 
console.log(gen.next()); 


console.log(gen.next().done); 

```

[Permalink](#heading-returning-from-the-function "Permalink")Returning from the function
----------------------------------------------------------------------------------------

If you return from a generator function, that marks the end of the function. After the `return` has been processed, any subsequent calls to the generator's `next()` method will yield `{ value: undefined, done: true }`.

```
function* myGenerator() {
  yield 1;
  yield 2;
  return "End of generator";  
  yield 3;  
}

const gen = myGenerator();

console.log(gen.next());  
console.log(gen.next());  
console.log(gen.next());  
console.log(gen.next());  

```

One thing worth noting is the `return` value won't appear if you loop over the generator function.

```
for (const value of myGenerator()) {
    console.log(value);  
}

```

[Permalink](#heading-loops "Permalink")Loops
--------------------------------------------

Since I mentioned loops, let's look into looping over the values yielded by a generator function.

```
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

for (const num of numberGenerator()) {
  console.log(num);  
}

```

### [Permalink](#heading-return-value "Permalink")Return value

The `return` value as previously mentioned won't appear in the loop.

```
function* generatorWithReturn() {
  yield 'a';
  yield 'b';
  return 'c';  
}

for (const char of generatorWithReturn()) {
  console.log(char);  
}

```

### [Permalink](#heading-async-generators "Permalink")Async generators

We'll dive deeper into async generators. But for now, how you'd loop over them:

```
async function* asyncNumberGenerator() {
  yield Promise.resolve(1);
  yield Promise.resolve(2);
  yield Promise.resolve(3);
}

async function runLoop() {
  for await (let num of asyncNumberGenerator()) {
    console.log(num);  
  }
}

runLoop();

```

[Permalink](#heading-yielding-from-other-generators "Permalink")Yielding from other generators
----------------------------------------------------------------------------------------------

You can also compose and yield from other generators:

```
function* generatorA() {
  yield 1;
  yield 2;
}

function* generatorB() {
  yield* generatorA();
  yield 3;
}

const genB = generatorB();
console.log(genB.next()); 

```

[Permalink](#heading-send-values-back-to-the-generator "Permalink")Send values back to the generator
----------------------------------------------------------------------------------------------------

You can send values back to the generator function.

It can't be done the first time you call `next()` because the generator function hasn't yielded yet.

It's a bit confusing and easier to explain with code. This can be done with one and multiple `yield` expressions.

### [Permalink](#heading-basic-1 "Permalink")Basic

```
function* myGenerator() {
  let received = yield "Hello"; 
  console.log(`Received: ${received}`); 
}

const gen = myGenerator();
console.log(gen.next().value); 
gen.next("World"); 

```

When calling `next()` the first time, the function will yield the string `Hello`. How this works is the function yields `Hello` and assigns the yielded value to the variable `received`. The output is of course different. That's what's getting logged in `console.log`.

Once a `yield` is done, it is ready and able to receive a value the next time. So the next time you call `next(value)`, you'd pass the value and it'll be in place of `"Hello"` instead.

### [Permalink](#heading-order-of-calling-matters "Permalink")Order of calling matters

**Be aware. If you call** `next()` **without a value and then again with a value, that won't have an effect. The order of how you call** `next()` **matters.**

```
function* myGenerator() {
  let received = yield "Hello"; 
  console.log(`Received: ${received}`); 
}

const gen = myGenerator();
console.log(gen.next().value); 
console.log(gen.next().value); 
gen.next("World"); 

```

Once the generator is done, you can't resume it.

### [Permalink](#heading-multiple-values "Permalink")Multiple values

What if you've multiple `yield` expressions?

The value you pass in would replace the `yield` where the generator is currently paused.

Let's take a look at a first example:

```
function* multipleYieldGenerator() {
  
  let a = yield "Pause 1"; 

  
  console.log(`Received a: ${a}`);

  
  let b = yield "Pause 2"; 

  
  console.log(`Received b: ${b}`);

  
  let c = yield "Pause 3"; 

  
  console.log(`Received c: ${c}`);

  return "Done";
}


const multiGen = multipleYieldGenerator();


console.log(multiGen.next().value);  


console.log(multiGen.next("Value for a").value);  


console.log(multiGen.next("Value for b").value);  


console.log(multiGen.next("Value for c").value);  

```

The first two times we call `next()` should make it clear.

The first time we call it, the first `yield` will run. The generator then pauses at that `yield`. This means the code between the first and second `yield` will run, hence run the `console.log`.

Now, the first `yield` is ready for the second time `next()` is called. It's ready to receive a value.

The second time we call `next()` with a value, the value will be given to the first `yield`. Both yields will run and the first two console logs will be printed.

After the second time, the generator is paused at the second `yield` .

It's worth noting that if you didn't pass a value the second time, the first console log would print `Received a: undefined`. This means the `yield` expression at which the generator is currently paused is replaced with `undefined`. However, the generator still "yields" that place, meaning it still resumes execution from that pause point.

### [Permalink](#heading-sequence-matters-when-multiple-yields "Permalink")Sequence matters when multiple yields

The sequence of calling `.next(value)` matters.

Each call to `next(value)` corresponds to a specific `yield` expression in the generator function. The value you pass in replaces the `yield` expression where the generator is currently paused.

You can't "skip" `yield` expressions or "go back" to previous ones. They are encountered in the order they appear in the generator function. So if you have multiple `yield` expressions and you want to send values back to them, you must do so in the sequence they appear.

Let's take a look at some code to make it clear:

```
function* multiYieldGen() {
  let first = yield "First Yield";
  console.log(`First received: ${first}`);

  let second = yield "Second Yield";
  console.log(`Second received: ${second}`);

  let third = yield "Third Yield";
  console.log(`Third received: ${third}`);
}

const gen = multiYieldGen();
console.log(gen.next().value);  
console.log(gen.next().value);  
console.log(gen.next("Value for second").value); 
console.log(gen.next("Value for third").value); 

```

In the example above, we didn't pass a value the second time calling `next()` , resulting in the first `yield` receiving undefined.

[Permalink](#heading-async-generator-functions "Permalink")Async generator functions
------------------------------------------------------------------------------------

It's similar to how you'd define normal async functions.

```
async function* myAsyncGenerator() {
  
  let data = await fetchData(); 
  yield data;
}

const gen = myAsyncGenerator();

```

You can consume values from an async generator using `for await...of` loops or the `next()` method.

### [Permalink](#heading-loops-1 "Permalink")Loops

```
for await (const val of gen) {
  console.log(val);
}

```

### [Permalink](#heading-using-next "Permalink")Using .next()

```
async function consume() {
  let result = await gen.next();
  while (!result.done) {
    console.log(result.value);
    result = await gen.next();
  }
}

consume();

```

### [Permalink](#heading-error-handling "Permalink")Error handling

To handle errors, you can have a try-catch block either in the async generator function or the code that consumes the generator.

```

async function* myAsyncGenerator() {
  try {
    let data = await fetchData();
    yield data;
  } catch (error) {
    console.error("An error occurred:", error);
  }
}

```

How you'd handle errors in the consumer code:

```


async function consumeWithCatch() {
  try {
    let result = await gen.next();
    while (!result.done) {
      console.log(result.value);
      result = await gen.next();
    }
  } catch (error) {
    console.error("An error occurred:", error);
  }
}

consumeWithCatch();

```

### [Permalink](#heading-errors-into-generator "Permalink")Errors into generator

What if something happens at the parent level and you want to throw an error into a generator for it then to handle the error?

This is where `.throw()` comes in handy.

It resumes the generator and throw an error into it. If you ever use it, it's expected that you handle the error in a try-catch block. Otherwise it wouldn't make much sense.

```
async function* myAsyncGenWithThrow() {
  try {
    yield 'Step 1';
    yield 'Step 2';
  } catch (error) {
    yield `Caught an error: ${error}`; 
  }
}

const genWithThrow = myAsyncGenWithThrow();
console.log(await genWithThrow.next()); 
console.log(await genWithThrow.throw(new Error('Something bad happened'))); 

```

[Permalink](#heading-stop-a-generator "Permalink")Stop a generator
------------------------------------------------------------------

You can stop generator. We'll dive into more real-world use cases for this in the next section, but for now, a quick explanation:

The `.return()` method returns a given value and finishes the generator. If you call `.next()` after calling `.return()`, the `{ done: true }` object is returned.

```
async function* myAsyncGen() {
  try {
    yield 'Step 1';
    yield 'Step 2';
  } finally {
    
    console.log('Cleaning up...');
  }
}

const gen = myAsyncGen();
console.log(await gen.next()); 
console.log(await gen.return('Stopped')); 
console.log(await gen.next()); 

```

[Permalink](#heading-large-photo-archive-system "Permalink")Large photo archive system
--------------------------------------------------------------------------------------

We have millions of photos we want to load and process. However, we don't want to consume too much memory when doing so.

Let's look at some sample code of how we'd deal with this to not load too many photos into memory:

```



const allPhotos = [
    'http://example.com/photo1.jpg',
    'http://example.com/photo2.jpg',
    
];





function* photoLoader(photos, chunkSize) {
    let index = 0; 

    
    while (index < photos.length) {
        
        yield photos.slice(index, index + chunkSize);
        index += chunkSize; 
                            
    }
}




const processPhotos = (photoChunk) => {
    for (const photo of photoChunk) {
        console.log(`Processing photo: ${photo}`);
        
        
    }
};



const photoChunkGenerator = photoLoader(allPhotos, 50);



let photoChunk; 

while (!(photoChunk = photoChunkGenerator.next()).done)  {
    processPhotos(photoChunk.value);
}

```

[Permalink](#heading-streaming-data-from-server-return "Permalink")Streaming data from server ( .return() )
-----------------------------------------------------------------------------------------------------------

Let's say we are building a newsfeed in a social media application. The feed retrieves posts in a "streaming" fashion, so it keeps an open connection with the server to get new posts as they come in.

The server sends a chunk of posts every few seconds, and we want to stop this when it's no longer needed. In this case, using the `.return()` method can come in handy.

Let's look at some code:

```

async function* streamPosts() {
  try {
    let postId = 1;

    while (true) {
      
      const post = await fetchPostFromServer(postId);

      if (post) {
        yield post;
      }

      
      await new Promise(resolve => setTimeout(resolve, 2000));

      postId++;
    }
  } finally {
    console.log("Stopped streaming posts. Cleaning up resources.");
    
  }
}


async function fetchPostFromServer(postId) {
  return new Promise(resolve => {
    setTimeout(() => resolve(`Post ${postId}`), 500);
  });
}


(async () => {
  const postStream = streamPosts();
  const maxPostsToDisplay = 5;
  let displayedPosts = 0;

  for await (const post of postStream) {
    console.log(`New post received: ${post}`);
    displayedPosts++;

    
    if (displayedPosts >= maxPostsToDisplay) {
      console.log("Max posts reached. Stopping stream.");
      
      postStream.return();
      break;
    }
  }
})();

```

[Permalink](#heading-data-transformation-pipeline-throw "Permalink")Data transformation pipeline ( .throw() )
-------------------------------------------------------------------------------------------------------------

Let's say we're building a system that handles transformation of large datasets. The transformation process is long-running and consists of multiple steps. Clients can start a transformation job, pause it, resume it, and even cancel it.

Because the transformation process is long and time-consuming, it's vital to monitor it for errors or external "Stop" commands from the user.

The `.throw()` method provides a way to inject an error into the generator from outside, enabling you to stop the generator at the currently paused operation and jump to the catch block to handle the error.

```

async function transformData(chunkId) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(`Transformed chunk ${chunkId}`), 1000);
  });
}


async function* dataTransformationPipeline(chunks) {
  try {
    for (let i = 0; i < chunks.length; i++) {
      
      
      const result = await transformData(chunks[i]);

      
      yield result;
    }
    yield 'Transformation complete.';
  } catch (error) {
    yield `Transformation failed with error: ${error.message}. Performing rollback...`;
  }
}


(async () => {
  const chunks = ['data1', 'data2', 'data3', 'data4'];
  const transformer = dataTransformationPipeline(chunks);

  
  console.log(await transformer.next());  

  
  setTimeout(async () => {
    console.log(await transformer.throw(new Error('User-initiated abort.')));  
  }, 2500);

  
  for await (const message of transformer) {
    console.log(message);
  }
})();

```

Generator function in JavaScript is powerful and was created with a purpose.