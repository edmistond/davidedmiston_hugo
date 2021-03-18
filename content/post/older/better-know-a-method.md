---
title: "Better Know a Method! - Ramda's .Cond(), Part 1"
date: 2019-09-28
draft: false
---

After spending a couple years away from C# and node.js programming doing a fair amount of ETL work and a lot of Java, I'm working on getting back into the swing of two of my favorite languages. Yes, I'll admit it - my controversial take is that I actually _like_ JavaScript a lot of the time. Like most languages, it's not perfect, but it gets the job done as long as you're careful (and do some testing) and its flexibility is both a blessing and a curse.

I've also had an interest in functional programming techniques, which [I've written](/parsing-bank-transactions-with-ramda-js/) [about a little](http://parsing-bank-transactions-with-ramda-js-2/) [bit in the past](). I don't claim any expertise here, but working with it and writing about it - even if you're wrong in public - is a great way to learn about it. And of course, if you're _really_ wrong, you can always count on the Internet to let you know about it. 🙂

All that aside, I'm starting on a new series of occasional posts that I'm calling "Better Know A Method!" - loosely inspired by some of [Safia Abdalla's](https://dev.to/captainsafia) posts from a year or two back where she'd pick something and examine it in-depth. I think that's a great way to learn, so today we're going to look at [Ramda.js](https://ramdajs.com/)'s `cond()` method - and in the best tradition of toddlers everywhere, take it apart to see how it works.

For a quick review, `cond()` takes an array of predicate and transformer pairs, each of which is a two element array of functions, and returns a function that takes one element as an argument. When you call this function, it will run through the predicates[0] until it hits one that returns a truthy value, then runs the associated transformer with the value supplied to the function. This is a sort of pattern matching, and more powerful than `switch()` since we can evaluate against multiple expressions, rather than one.

[0] In this context, a predicate is a logical expression which evaluates to true or false, generally to direct the execution path of code.

From one of my [previous articles](/parsing-bank-transactions-with-ramda-js-2/), here's an example of using it:

``` javascript
const classifyPetTransactions = (transactionList) => {
  let care = [];
  let food = [];

  const classifyCare = (t) => R.contains(t['Payee Name'], ["CAMP BOW WOW", "VET", "GROOMER"]);
  const classifyFood = (t) => t['Payee Name'] === "PET STORE";

  const classifier = R.cond([
    [classifyFood, (t) => food.push(t)],
    [classifyCare, (t) => care.push(t)]
  ]);

  R.forEach(classifier, transactionList);

  return [care, food];
}
```

We'll start by cloning down the library from Github. It looks to be nicely organized with all functions pretty easy to find - no in-depth digging needed. And even better, it's extensively documented, _including_ documentation on a bunch of the internal functions it's using. This is already better than some libraries I've looked at!

So, as of September 2019, here's the `cond()` function in its entirety:

``` javascript
var cond = _curry1(function cond(pairs) {
  var arity = reduce(
    max,
    0,
    map(function(pair) { return pair[0].length; }, pairs)
  );
  return _arity(arity, function() {
    var idx = 0;
    while (idx < pairs.length) {
      if (pairs[idx][0].apply(this, arguments)) {
        return pairs[idx][1].apply(this, arguments);
      }
      idx += 1;
    }
  });
});
```

My first reaction is to ask what the heck is going on here, since the code is very terse. Then I remind myself that there's no magic in software development - everything is explainable if you dig enough, especially since we've got good documentation at our fingertips here. Let's get to it.

The internal `_curry1` function wraps the entire method. Currying is not the same as partial function application, and to be clear, I was wrong about that in a previous article. The technique I discussed then was not currying, but partial function application. I'm not totally familiar with how Ramda's internal currying functions work, but let's set that aside for the moment. I want to get to the heart of the function to understand what that's doing, then work back up from there.

From what I can see, this is the central point of the function -- this is how it works: 

``` javascript
while (idx < pairs.length) {
  if (pairs[idx][0].apply(this, arguments)) {
    return pairs[idx][1].apply(this, arguments);
  }
  idx += 1;
}
```

The `pairs` variable is the collection of predicate/transformer pairs that we're pushing in - so what this is doing is taking that array of arrays, iterating through it, and calling `Function.prototype.apply()` on the `[0]` element of each pair to run the predicate. If the predicate returns a truthy value, then it will return the result of applying the transformer. Otherwise, it'll continue iterating until it finds something that matches, or runs out of predicates.

This is one of the features in JavaScript that I think is quite cool: you can create an array, or dictionary, or other data structure that's actually full of functions and apply all sorts of conditional logic to when those functions gets called. Of course, you can do this in other languages too, but JS makes it easy and I love it for that.

In my next post, I'll come back to this and start digging into those `_curry()` and `_arity()` functions to better understand what they're doing. Hope you've enjoyed this and please leave a comment if you have any questions or corrections to offer.