---
title: 'Bank Transactions with Ramda, part 2'
date: 2017-03-02
tags: [node, ramda, functional]
---

When we left off, we'd gotten our data imported from the CSV, run a map operation on it to add some extra metadata, demonstrated how to use Ramda to filter it, and had a quick demonstration of simple currying. Let's move on. To recap, this is where we're starting from:
<!-- more -->

```javascript parse.js
const fs = require('fs');
const csv = require('fast-csv');
const R = require('ramda');

function getTransactionsFromFile(fname) {
  return new Promise(async (resolve, reject) => {

    if (!fs.existsSync(fname)) {
      return reject('file does not exist!');
    }

    let transactions = [];
    let stream = fs.createReadStream(fname);

    csv.fromStream(stream, { headers: true })
      .on('data', (data) => { transactions.push(data); })
      .on('error', (err) => { return reject(error); })
      .on('end', () => { return resolve(transactions); });
  });
}

async function run() {
  try {
    let transactionList = await getTransactionsFromFile('./data.csv');
    processTransactions(transactionList);
  }
  catch (ex) {
    console.log('I caught an error:', ex);
    return;
  }
}

const tagTransaction = (transaction) => {
  // Note that this is checking transactions by exact payee name match; 
  // "MCDONALDS #372" and "MCDONALDS #778" would not match.
  const checkPayeeMatch = R.contains(transaction['Payee Name']);

  transaction.isRestaurant = checkPayeeMatch(restaurantPayees);
  transaction.isPet = checkPayeeMatch(petPayees);
  transaction.Amount = parseFloat(transaction.Amount);

  return transaction;
};

const processTransactions = (transactionList) => {
  const taggedTransactions = R.map(tagTransaction, transactionList);

  const petTransactions = R.filter(t => t.isPet, taggedTransactions);
  console.log('You had', petTransactions.length, 'purchases for the dog');
};

// > node parse.js
// You had 10 purchases for the dog
```

### Summing up

Okay, so let's dive in. Next up, we'd like to see how much we spent taking care of the dog in the past couple of months - day care, vet bills, purchases at the pet store, stuff like that. This is pretty straightforward in the transaction processing block, fortunately:

```javascript
const processTransactions = (transactionList) => {
  const taggedTransactions = R.map(tagTransaction, transactionList);

  const petTransactions = R.filter(t => t.isPet, taggedTransactions);
  console.log('You had', petTransactions.length, 'purchases for the dog');

  let petTotal = R.sum(R.map(t => t.Amount, petTransactions));
  console.log("You spent", petTotal.toFixed(2), "on the dog last year.");
};

// > node parse.js
// You had 10 purchases for the dog
// You spent -924.60 on the dog last year.
```

Note that since we wanted to total up the amount specifically and what we have is an array of transaction objects, we first had to map the transactions collection to extract just the value of the `Amount` property. Afterward, it's very straightforward and we just drop it straight into `R.sum`, which takes a single array argument.

That said, we can apply some additional tools here to again create general summarizer function that will take *any* array of objects that have an `Amount` property and generate a summary for any of them - so if we had collections of both pet totals and restaurant totals, we could create the function once and run either array through it, like so:

```javascript
  const summarizer = R.pipe(R.map(t => t.Amount), R.sum);
  let petTotal = summarizer(petTransactions);
  let restaurantTotal = summarizer(restaurantTransactions);
```

### Simple printer functions

So that's pretty cool. Next, let's put together a simple printer function to generate a one-line summary of a transactions block, again using some of the same principles we've seen illustrated here so far:

```javascript
const processTransactions = (transactionList) => {
  const taggedTransactions = R.map(tagTransaction, transactionList);

  const petTransactions = R.filter(t => t.isPet, taggedTransactions);
  const summarizer = R.pipe(R.map(t => t.Amount), R.sum);
  const generateSummary = (transactions) => {
    return `${transactions.length} transactions for a total of \$${Math.abs(summarizer(transactions)).toFixed(2)}`;
  };

  console.log('Pet expenses:', generateSummary(petTransactions));
}

// > node parse.js
// Pet expenses: 10 transactions for a total of $924.60
```

### Pattern matching-ish with `cond`

Finally, let's take a look at using `R.cond` to achieve something that behaves kind of like pattern matching in functional languages like Elixir. It's not a *perfect* translation of the concept, but the technique does work pretty well. For a contrived example, let's say we wanted to break out the different sorts of pet-related transactions: food transactions from the pet store, versus "care" things like vet visits, grooming, and day care.

```javascript
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

const processTransactions = (transactionList) => {
  const taggedTransactions = R.map(tagTransaction, transactionList);

  const petTransactions = R.filter(t => t.isPet, taggedTransactions);
  const summarizer = R.pipe(R.map(t => t.Amount), R.sum);
  const generateSummary = (transactions) => {
    return `${transactions.length} transactions for a total of \$${Math.abs(summarizer(transactions)).toFixed(2)}`;
  };

  const [petCare, petFood] = classifyPetTransactions(petTransactions);

  console.log('Pet expenses:', generateSummary(petTransactions));
  console.log('Food expenses:', generateSummary(petFood));
  console.log('Care expenses:', generateSummary(petCare));
}

// > node parse.js
// Pet expenses: 10 transactions for a total of $924.60
// Food expenses: 3 transactions for a total of $174.90
// Care expenses: 7 transactions for a total of $749.70
```

In `classifyPetTransactions` we're running each item through `R.cond`; what happens there is that `cond` takes an array of `[predicate, transformer]` elements and returns a function. Predicates should return a "truthy" value. You then pass an object to the function which was returned, and that object will be passed to the predicate, in the order they were defined, until it hits the first one to return a truthy value and applies the transformer function. In our case, the transformer doesn't actually transform, but instead pushes the transaction into a predefined array. We could, of course, have also have set a property on the transaction, or performed some other action:

```javascript
const classifier = R.cond([
  [classifyFood, (t) => t.tag = "food"],
  [classifyCare, (t) => t.tag = "care"]
]);
```

All of this is, of course, a relatively simple and contrived example, but I find that's often helpful. I found `cond` a bit difficult to understand at first, and it was by working with relatively simple examples like this one that I finally gained an understanding of it.

### In Conclusion

Some questions you may be asking: why is this useful? Why would I bother with this when existing language constructs allow me to accomplish all of this already?

That's a fair question! For a very simple and contrived example like this, it's true - you can do this very easily without needing any of the functionality offered by Ramda or Lodash. Even with more complicated stuff, you can still get by without doing it. However, I've applied these techniques in production applications and found that they result in logic that's significantly easier to understand and modify.

One great example of that: I worked on a system with a requirement that if the customer selected a package of services, we should show them similar package that would be upgrades in some way from what they had selected. The initial solution that comes to mind is, of course, to have some kind of table of packages and how they related to each other. The problem with that approach was that packages were unique to geographic areas and changed frequently - so the preferred solution was building that information solely from the package web service.

In fact, in the original version of the application, that's exactly how my coworker implemented it. Unfortunately, when we were rewriting that portion of the application backend, we weren't able to use the original code as written. Since I thought it was rather confusing as implemented, I took a crack at rewriting it along the principles described in these two posts - though in this case, since we'd already taken a dependency on Lodash, I used that and a heavy dose of the Lodash [`curry`](https://lodash.com/docs/#curry) method. What I found was that this broke the code down into a series of straightforward steps that were easy to reason about.

It's not going to be a hammer for every nail that you run across, but when you have these sorts of problems that you need to break down, it's a fantastic tool to have in the box. Even if it doesn't end up being the approach I use, I've found it influences the way I think about and break down problems.

It's also worth noting this doesn't have to be exclusive to JavaScript, either - the approach will work just as well in other languages! Ruby has many of the same functions (`map`, `find`, `reduce`, `sum`, etc. as well as currying) on its own enumerable methods. C# and LINQ also allow this (`Select`, `Where`, `Sum`, `Aggregate`) - though I haven't tried to do currying in C# and it appears to be a bit more complex there.

To recap, here's what our final file looks like:

```javascript
const fs = require('fs');
const csv = require('fast-csv');
const R = require('ramda');

function getTransactionsFromFile(fname) {
  return new Promise(async (resolve, reject) => {

    if (!fs.existsSync(fname)) {
      return reject('file does not exist!');
    }

    let transactions = [];
    let stream = fs.createReadStream(fname);

    csv.fromStream(stream, { headers: true })
      .on('data', (data) => { transactions.push(data); })
      .on('error', (err) => { return reject(error); })
      .on('end', () => { return resolve(transactions); });
  });
}

async function run() {
  try {
    let transactionList = await getTransactionsFromFile('./data.csv');
    processTransactions(transactionList);
  }
  catch (ex) {
    console.log('I caught an error:', ex);
    return;
  }
}

const tagTransaction = (transaction) => {
  // Note that this is checking transactions by exact payee name match; 
  // "MCDONALDS #372" and "MCDONALDS #778" would not match.
  const checkPayeeMatch = R.contains(transaction['Payee Name']);

  transaction.isRestaurant = checkPayeeMatch(restaurantPayees);
  transaction.isPet = checkPayeeMatch(petPayees);
  transaction.Amount = parseFloat(transaction.Amount);

  return transaction;
};

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

const processTransactions = (transactionList) => {
  const taggedTransactions = R.map(tagTransaction, transactionList);

  const petTransactions = R.filter(t => t.isPet, taggedTransactions);
  const summarizer = R.pipe(R.map(t => t.Amount), R.sum);
  const generateSummary = (transactions) => {
    return `${transactions.length} transactions for a total of \$${Math.abs(summarizer(transactions)).toFixed(2)}`;
  };

  const [petCare, petFood] = classifyPetTransactions(petTransactions);

  console.log('Pet expenses:', generateSummary(petTransactions));
  console.log('Food expenses:', generateSummary(petFood));
  console.log('Care expenses:', generateSummary(petCare));
}
```

Thanks for reading, and I hope you found it useful!