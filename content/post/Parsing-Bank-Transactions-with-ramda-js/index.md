---
title: "Parsing Bank Transactions With Ramda Js"
date: 2017-03-01
tags: [node, ramda, functional]
---

Recently, I'd wanted to sort through a bunch of transaction data from my bank to figure out what our spending trends were in a couple of areas. I suppose I could've done this quite effectively with Excel or Apple Numbers, but then I said, hey, that's boring. :) I've been doing a lot of documentation and research stuff at work lately and really wanted to get my hands on a little toy project for a change of pace.

That said, I didn't want to spend *too* much time getting bogged down in stuff, so I decided to do it with node.js; node 7.6.0 was released recently and has native async/await, so it seemed like a great choice for a couple hours of hacking.
<!-- more -->

### Setting up

First thing to do was to get a bunch of transaction data from my bank's web site; they make it easy to download in CSV. That returns a file with the following format:

```
"Date","Reference Number","Payee Name","Memo","Amount","Category Name"
```

The payee name field seems to have a hard limit of 15 characters for most entries, unless they originated within the bank's system. Annoying and not totally descriptive, but whatever, we can deal with it.

First order of business: get the file in. I'm using [fast-csv](https://www.npmjs.com/package/fast-csv) to parse the files. The actual process of that is pretty straightforward. Since this is a small file, rather than process the stream events individually, I'd rather just append them all to a collections object, read 'em in, and then return them all at once. If you were doing this on a huge file, that's probably a bad idea. :) However, for ~500 transactions or so, it's no big deal. So let's do that: we're going to read in the file, append all the transactions into an array, then finally resolve the promise and return the data once we hit the end of the file. Along the way, we're also going to reject the promise if the CSV parsing throws an error.

```javascript
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

let processTransactions = (transactionList) => {
  console.log('got', transactionList.length, 'transactions');
};

run();

// > node parse.js
// got 478 transactions
```

### Operating on data with `map`

Excellent. Data. Now how to parse it?

Traditionally, this sort of thing would involve writing a bunch of looping code to iterate through the transactions array, examining each one, and possibly appending it to another array if it was a transaction that was of interest to us. Then we'd write more loops to do other operations, like summing up all the transactions of a given type. Pretty straightforward, but honestly, I find this approach tedious. 

This is where [Ramda](http://ramdajs.com/) comes in. You can use [lodash](http://lodash.com) as well, but I've become rather fond of Ramda over the past year or so. Though I wouldn't go adding it indiscriminately into existing projects that already use lodash, it's been my library of choice for doing map/filter/reduce/sort type operations in my personal projects. I like that it automatically curries functions when you don't supply all the arguments, and that it in fact encourages this use by making the collection/array object the last argument supplied to the function.

I particularly like this approach because I think it's more explicit. If you're using a loop, anyone else reading the code has to take the time to inspect the loop and understand what the code in it is doing. When I see `R.map` called, I immediately think "Okay, this is transforming data somehow." I will acknowledge this may be a little confusing at first to developers who aren't experienced with the concepts. This is where mentoring comes in, though, and I think it's much more straightforward once it "clicks" with folks.

So let's say we want to figure out how much we spent on the dog, and also how much we spent eating out. First, we need to tag our transactions with some additional information:

* Was it income or an expense?
* Was it a restaurant?
* Was it pet-related?

We don't *have* to do it this way, but I'm going to run the transactions array through a map operation that transforms them by adding additional data to them. First we need a function that takes a single transaction, inspects the payee and payment amounts, and tags them with some data:

```javascript
const restaurantPayees = ["MCDONALDS", "CHIPOTLE", "JACKS DELI"];
const petPayees = ["CAMP BOW WOW", "VET", "PET STORE", "GROOMER"];

const tagTransaction = (transaction) => {
  // Note that this is checking transactions by exact payee name match; 
  // "MCDONALDS #372" and "MCDONALDS #778" would not match.
  if(R.contains(transaction['Payee Name'], restaurantNames)) {
    transaction.isRestaurant = true;
  }
  else if (R.contains(transaction['Payee Name'], petPayees)) {
    transaction.isPet = true;
  }

  transaction.isIncome = parseFloat(transaction.Amount) > 0.0;

  return transaction;
};
```

And then, just to confirm it's working correctly, let's also add a quick filter operation on the pet transactions to see how many there were.

```javascript
const processTransactions = (transactionList) => {
  const taggedTransactions = R.map(tagTransaction, transactionList);

  const petTransactions = R.filter(t => t.isPet, taggedTransactions);
  console.log('You had', petTransactions.length, 'purchases for the dog');
};

// > node parse.js
// You had 10 purchases for the dog
```

### Quickly demonstrating currying

But wait! We also have an opportunity to use Ramda's automatic currying of functions. Put simply, this allows us to call a function without supplying its final values, getting back in turn another function that you can apply repeatedly against different values, using the same initial input. That would look like this:

```javascript
const tagTransaction = (transaction) => {
  // Note that this is checking transactions by exact payee name match; 
  // "MCDONALDS #372" and "MCDONALDS #778" would not match.
  const checkPayeeMatch = R.contains(transaction['Payee Name']);

  transaction.isRestaurant = checkPayeeMatch(restaurantPayees);
  transaction.isPet = checkPayeeMatch(petPayees);
  transaction.Amount = parseFloat(transaction.Amount);

  return transaction;
};

// > node parse.js
// You had 10 purchases for the dog
```

As you can see, we built a curried `checkPayeeMatch` function by calling `R.contains` and only supplying the payee name field; we can then use it to check the same payee against both the restaurant and pet-related payee names. It's a small thing, but an example of "don't repeat yourself" in action.

So far, we've imported our transactions into an array (using async/await, even!) written a tagging function to add some additional metadata to our transaction, used ramda to apply that mapping function to the transactions array and get back a new array of modified transactions, and used a simple currying example to reduce repetition in our code.

In the next article on this, we'll look at using R.sum to total up transactions, outputting data with r.forEach, and a new way to implement conditional logic with R.cond.