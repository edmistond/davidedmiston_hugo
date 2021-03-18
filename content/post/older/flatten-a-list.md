---
title: "Flattening a List"
date: 2017-03-28
tags: [node, ramda, functional]
---

Getting back to writing articles after spending the better part of a month fighting off a sinus infection and helping my wife get over a nasty cold. Normally I love northeast Ohio, but I'm so over winter right now.

[I read a post a month or so ago](https://shekhargulati.com/2017/02/24/why-its-hard-for-programmers-to-write-a-program-to-flatten-a-list/) asking why it's so difficult for programmers to write code to flatten a list... so naturally, this got me thinking about it and I wanted to tackle it. To start with, let's try tackling it in Javascript.
<!-- more -->

For this sort of question, I actually think a dynamic language like JavaScript or Ruby is probably the ideal choice, and the reason for that is hinted at in the post when he remarks that:

> Candidates fail to write proper method signatures. They get confused about what type of list they should use. Some start with List of integers `List<Integer>` ints. They fail to see how they will store a `List<Integer>` to a `List<Integer>`.

Since JavaScript lets you stuff pretty much anything into an array&mdash;for better or sometimes worse, as the case may be&mdash;the problem is much more straightforward to approach there.

He asks candidates to write test cases. Let's start with a simple stub file and a test&mdash;I'm using [Mocha](https://mochajs.org/) here.

```javascript flatten.js
function flatten(inputArray) {
  return inputArray;
}

module.exports.flatten = flatten;
```

```javascript flatten_test.js
var assert = require('assert');
var f = require('./flatten');

describe("Flatten", () => {
  it("should return a flattened array when passed a nested array", () => {
    var testArray = [1,[2,3], [4, [5,6]]];

    var result = f.flatten(testArray);

    assert.equal(result, [1,2,3,4,5,6]);

  });
});

// [~/source/js_tests]$ mocha flatten_test.js -R list
//
// 1) Flatten Should return a flattened array when passed a nested array:
//
//     AssertionError: [ 1, [ 2, 3 ], [ 4, [ 5, 6 ] ] ] == [ 1, 2, 3, 4, 5, 6 ]
```

Of course, since we're just throwing the original array right back out, it immediately bombs. Not so useful! But on the plus side, this gives us a nice test harness for executing the file, so let's dig into this further. I'm going to `it.skip()` our first test here to reduce test clutter while we work our way back into this. First, let's make sure we're handling a couple of straightfoward base cases correctly, such as single-element arrays that aren't a sub-array:

```javascript flatten.js
function flatten(inputArray) {
  var result = [];

  if (inputArray instanceof Array) {
    if (inputArray.length === 1 && !(inputArray[0] instanceof Array)) {
      // Nothing further to do. Return the original array.
      return inputArray;
    }
  }

  return result;
}
```
```javascript flatten_test.js
it("should return the original array if it's a single element with no nested arrays", () => {
  var testArray = [1];
  var result = f.flatten(testArray);

  assert.equal(testArray, result);
});

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     - should return a flattened array when passed a nested array
//
//
//   1 passing (8ms)
//  1 pending
```

Okay. Now let's try a multi-element array with no sub-arrays:

```javascript flatten.js
if (inputArray instanceof Array) {
  if (inputArray.length === 1 && !(inputArray[0] instanceof Array)) {
    // Nothing further to do. Return the original array.
    return inputArray;
  }

  var processArrayItem = (item) => {
    result.push(item);
  };

  R.forEach(processArrayItem, inputArray);
}
```
```javascript flatten_test.js
it("should return a two element array with no sub-arrays", () => {
  var testArray = [1, 2];
  var result = f.flatten(testArray);
  assert.equal(testArray, result);
});
// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     1) should return a two element array with no sub-arrays
//     - should return a flattened array when passed a nested array
//
//   1 passing (8ms)
//   1 pending
//   1 failing
//
//   1) Flatten should return a two element array with no sub-arrays:
//
      // AssertionError: [ 1, 2 ] == [ 1, 2 ]
```
Alrighty then... wait, what? Why is it saying that `[1, 2]` doesn't equal `[1, 2]`? We need to check using `deepEqual` instead of just `equal`&mdash;it's a bit much to get into here, but [this link](http://dailyjs.com/post/js101-deep-equal) goes into a quick overview of the differences. Interestingly, using `equal` works fine on single-element arrays and doesn't try to tell you that `[1]` and `[2]` are equal.

Change the assertion and the tests pass:

```javascript flatten_test.js
    assert.deepEqual(testArray, result);

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
//     - should return a flattened array when passed a nested array
//
//   2 passing (10ms)
//   1 pending
```

Moving on, now we should be able to focus all our attention on the `processArrayItem` function since that's going to be doing most of the rest of the work. Currently, it naively pushes each array item into the result array, assuming that it's a single element and not a sub-array. Clearly not what we want here so let's see what we can do about that. We'll add a test and then write some code to see if we can get it to go green.

```javascript flatten_test.js
it("should flatten sub-arrays", () => {
  var testArray = [1, [2, 3]];
  var result = f.flatten(testArray);

  assert.deepEqual(result, [1, 2, 3]);
});
```
```javascript flatten.js
var processArrayItem = (item) => {
  if (item instanceof Array) {
    var res = flatten(item);
    console.log('flattened subarray:', res);
    result.push(res);
  } else {
    result.push(item);
  }
};

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
// flattened subarray: [ 2, 3 ]
//     1) should flatten simple sub-arrays
//     - should return a flattened array when passed a nested array
//
//   2 passing (10ms)
//   1 pending
//   1 failing
//
//   1) Flatten should flatten simple sub-arrays:
//
//       AssertionError: [ 1, [ 2, 3 ] ] deepEqual [ 1, 2, 3 ]
      // + expected - actual
```

So yeah, that didn't quite work; the recursive function worked perfectly, but it returned its results all at once. I anticipated this and added a bit of `console.log`ing; you can see we got back an array that got pushed into the results array as a single element, and puts us right back around where we started.

So... how do we attack this? Since we're already using Ramda, we could just use the `concat` method for this:

```javascript flatten.js
var processArrayItem = (item) => {
  if (item instanceof Array) {
    result = R.concat(result, flatten(item));
  } else {
    result.push(item);
  }
};

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
//     ✓ should flatten simple sub-arrays
//     - should return a flattened array when passed a nested array
//
//   3 passing (7ms)
//   1 pending
```

That works! Of course, if we're using Ramda... we could've also just called `R.flatten` on this and then called it a day. :)

I'm also looking at my original flatten function and realizing it's a bit redundant. We're flattening an array; *of course* it's going to take array objects! So... we don't actually need to have everything inside a nested "is this an array?" check. Let's remove that and replace it with a simple guard clause.

```javascript flatten.js
var R = require('ramda');

function flatten(inputArray) {
  var result = [];

  if (!inputArray instanceof Array) {
    return result.push(inputArray);
  }

  if (inputArray.length === 1 && !(inputArray[0] instanceof Array)) {
    // Nothing further to do. Return the original array.
    return inputArray;
  }

  var processArrayItem = (item) => {
    if (item instanceof Array) {
      result = R.concat(result, flatten(item));
    } else {
      result.push(item);
    }
  };

  R.forEach(processArrayItem, inputArray);

  return result;
}

module.exports.flatten = flatten;

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
//     ✓ should flatten simple sub-arrays
//     - should return a flattened array when passed a nested array
//
//   3 passing (7ms)
//   1 pending
```

Since we have tests, we can see we didn't break anything there. Let's try un-skipping that more complicated test, applying that `deepEqual` fix to it, and seeing if we get the behavior we want.

```javascript flatten_test.js
it("should return a flattened array when passed a nested array", () => {
  var testArray = [1, [2, 3], [4, [5, 6]]];

  var result = f.flatten(testArray);

  assert.deepEqual(result, [1, 2, 3, 4, 5, 6]);
});

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
//     ✓ should flatten simple sub-arrays
//     ✓ should return a flattened array when passed a nested array
//
// 4 passing (7ms)
```

Oh&mdash;but we don't have a test case for that guard clause! We know everything works when we pass in an array. Let's also make sure we get the expected behavior if we just pass in a bare element - it should "flatten" it into a single element array. I'll note I've broken with good practice here; by the strict approach to TDD, I should have written a test for this prior to even adding that guard clause. However, I think it's okay to be a little lax while you're spiking concepts (or blogging :)). Even so, let's clean this up.

```javascript flatten_test.js
it("should return an array when passed a bare object", () => {
  var result = f.flatten(5);
  assert.deepEqual(result, [5]);
});
// 1) Flatten should return an array when passed a bare object:
//
// AssertionError: [] deepEqual [ 5 ]
```

Huh... well, crap. Let's tweak that guard clause:

```javascript flatten.js
if (!(inputArray instanceof Array)) {
  return [inputArray];
}
```

... and that gets it. Note that the `instanceof` check needs to be wrapped in extra parentheses to negate it within an `if` statement; it took me a while to run that one down.

All that aside, we've accumulated a bit of clutter here; we don't actually *need* the initial check for single-length arrays, since our processArrayItem function will handle that scenario just fine. Let's go ahead and strip that out and see what happens:

```js flatten.js
function flatten(inputArray) {
  var result = [];

  if (!(inputArray instanceof Array)) {
    return [inputArray];
  }

  var processArrayItem = (item) => {
    if (item instanceof Array) {
      result = R.concat(result, flatten(item));
    } else {
      result.push(item);
    }
  };

  R.forEach(processArrayItem, inputArray);

  return result;
}

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     1) should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
//     ✓ should flatten simple sub-arrays
//     ✓ should return a flattened array when passed a nested array
//     ✓ should return an array when passed a bare object
//
//   4 passing (10ms)
//   1 failing
//
//   1) Flatten should return the original array if it's a single element with no nested arrays:
//
//       AssertionError: [ 1 ] == [ 1 ]
```

We're not returning the same object anymore, so we need to fix our test to use `deepEqual` there too... and that gets it.

Next up - we don't actually *need* to use ramda's `forEach` method here; Javascript itself has `forEach` built in to the array object. It also has a `concat` method, so we can entirely write this in vanilla JavaScript:

```js flatten.js
function flatten(inputArray) {
  var result = [];

  if (!(inputArray instanceof Array)) {
    return [inputArray];
  }

  inputArray.forEach((item) => {
    if (item instanceof Array) {
      result = result.concat(flatten(item));
    } else {
      result.push(item);
    }
  });

  return result;
}

module.exports.flatten = flatten;
```

And to recap, here's what our tests look like right now - I made a minor tweak to keep the result versus test values consistently ordered across tests, since that's more readable while debugging.

```js flatten_test.js
var assert = require('assert');
var f = require('./flatten');

describe("Flatten", () => {
  it("should return the original array if it's a single element with no nested arrays", () => {
    var testArray = [1];
    var result = f.flatten(testArray);

    assert.deepEqual(result, testArray);
  });

  it("should return a two element array with no sub-arrays", () => {
    var testArray = [1, 2];
    var result = f.flatten(testArray);
    assert.deepEqual(result, testArray);
  });

  it("should flatten simple sub-arrays", () => {
    var testArray = [1, [2, 3]];
    var result = f.flatten(testArray);

    assert.deepEqual(result, [1, 2, 3]);
  });

  it("should return a flattened array when passed a nested array", () => {
    var testArray = [1, [2, 3], [4, [5, 6]]];

    var result = f.flatten(testArray);

    assert.deepEqual(result, [1, 2, 3, 4, 5, 6]);
  });

  it("should return an array when passed a bare object", () => {
    var result = f.flatten(5);
    assert.deepEqual(result, [5]);
  });
});
```

All of the tests run and pass. It's a relatively functional approach; our `flatten` function doesn't have any external side effects and we're not using any explicit 'for' loops with an index (I generally dislike these). We *could* replace the `if` statement in the `forEach` with a Ramda `cond` matcher, but honestly - that feels like massive overkill, especially since we don't actually need Ramda for anything else. So, in this case, I think I'd advocate keeping it simple and not introducing a substantial dependency that I don't really need.

The only thing I don't particularly care for is using the `concat` method; it feels like we're cheating a bit since we're getting another array back in some cases and shoving it into our results array. This works, and it solves the problem - but I don't totally like it.

One approach would be adding an accumulator to our `flatten` function allowing us to pass in the original result array, if it exists, and simply append more values to that. Let's try it! We'll write a quick `accumulatingFlatten` function and then just change the `module.exports` to return that for the flatten function instead; that'll let us see if all our tests still work.

```js flatten.js
function accumulatingFlatten(inputArray, acc) {
  if (!(inputArray instanceof Array)) { return [inputArray]; }

  acc = acc || [];

  inputArray.forEach((item) => {
    if (item instanceof Array) {
      accumulatingFlatten(item, acc);
    }
    else {
      acc.push(item);
    }
  });

  return acc;
}

module.exports.flatten = accumulatingFlatten;

// [~/source/js_tests]$ mocha flatten_test.js
//
//   Flatten
//     ✓ should return the original array if it's a single element with no nested arrays
//     ✓ should return a two element array with no sub-arrays
//     ✓ should flatten simple sub-arrays
//     ✓ should return a flattened array when passed a nested array
//     ✓ should return an array when passed a bare object
//
//   5 passing (7ms)
```

Awesome. Still works.

This post got a bit long - I try to post fairly substantial code examples, because I find posts with lots of small snippets hard to follow. Hopefully it's a useful discussion of how you'd reason your way through a toy problem like this, with some demonstration of testing thrown in for good measure. It bears repeating, though - this is a code golf problem, but please don't write your own array flattening function for production use. Lodash and Ramda are both well-written, extensively-tested libraries that already have a method for this, and you're better off just using that.
