# Functional programming (FP)

## FP with javascript

[Great book to start with FP](https://mostly-adequate.gitbooks.io/mostly-adequate-guide)

FP is the process of building software by composing pure functions, avoiding shared state, mutable data, and side-effects.

FP is declarative rather than imperative, and application state flows through pure functions.

FP helps a lot `simplifying reading code`, then `maintain` and also by its design (no side effect or shared state) to `test`.

`we're trading ease-of-writing for pain-of-reading`

<!-- Clean code FP -->

<!-- Rule 1 : prefer named function to anonymous : Name every single function
if you sit there stumped, unable to come up with a good name for some function you've written,
 I'd strongly suggest you don't fully understand that function's purpose yet -- or it's just too broad or abstract.
  You need to go back and re-design the function until this is more clear.
And by that point, a name will become more apparent.

In my practice, if I don't have a good name to use for a function, I name it TODO initially.
 I'm certain that I'll at least catch that later when I search for "TODO" comments before committing code.

 -->

```js
people.map(function getPreferredName(person) {
  return person.nicknames[0] || person.firstName;
});
// ..
```

<!-- Rule 2: Avoid using `this` keyword -->

```js
var Auth = {
    authorize() {
        var credentials = `${this.username}:${this.password}`;
        this.send( credentials, resp => {
            if (resp.error) this.displayError( resp.error );
            else this.displaySuccess();
        } );
    },
    send(/* .. */) {
        // ..
    }
};

var Login = Object.assign( Object.create( Auth ), {
    doLogin(user,pw) {
        this.username = user;
        this.password = pw;
        this.authorize();
    },
    displayError(err) {
        // ..
    },
    displaySuccess() {
        // ..
    }
} );

Login.doLogin( "fred", "123456" );


// becomes


// ..

authorize(ctx) {
    var credentials = `${ctx.username}:${ctx.password}`;
    Auth.send( credentials, function onResp(resp){
        if (resp.error) ctx.displayError( resp.error );
        else ctx.displaySuccess();
    } );
}

// ..

doLogin(user,pw) {
    Auth.authorize( {
        username: user,
        password: pw
    } );
}

// ..

```

## Building blocks of FP

### unary function

### identity function

```js
// version 1
var getCurrentUser = function partiallyApplied(...laterArgs) {
  return ajax(
    "http://some.api/person",
    { user: CURRENT_USER_ID },
    ...laterArgs
  );
};

// version 2
var getCurrentUser = function outerPartiallyApplied(...outerLaterArgs) {
  var getPerson = function innerPartiallyApplied(...innerLaterArgs) {
    return ajax("http://some.api/person", ...innerLaterArgs);
  };

  return getPerson({ user: CURRENT_USER_ID }, ...outerLaterArgs);
};

function partialRight(fn, ...presetArgs) {
  return function partiallyApplied(...laterArgs) {
    return fn(...laterArgs, ...presetArgs);
  };
}

// or the ES6 => arrow form
var partialRight = (fn, ...presetArgs) => (...laterArgs) =>
  fn(...laterArgs, ...presetArgs);
```

### curry

A technique similar to partial application, where a function that expects multiple arguments is broken down into successive chained functions that each take a single argument (arity: 1) and return another function to accept the next argument

```js
var curriedAjax = curry(ajax);

var personFetcher = curriedAjax("http://some.api/person");

var getCurrentUser = personFetcher({ user: CURRENT_USER_ID });

getCurrentUser(function foundUser(user) {
  /* .. */
});

[1, 2, 3, 4, 5].map(curry(add)(3));
// [4,5,6,7,8]

var adder = curry(add);

// It might be helpful in the case where you know ahead of time that add(..) is the function to be adapted, but the value 3 isn't known yet:

// later
[1, 2, 3, 4, 5].map(adder(3));
// [4,5,6,7,8]
```

`The advantage of currying here is that each call to pass in an argument produces another function that's more specialized, and we can capture and use that new function later in the program`

FP use closure to remember the arguments over time until all have been received, and then the original function can be invoked.

```js
function sum(...nums) {
  var total = 0;
  for (let num of nums) {
    total += num;
  }
  return total;
}

sum(1, 2, 3, 4, 5); // 15

// now with currying:
// (5 to indicate how many we should wait for)
var curriedSum = curry(sum, 5);

curriedSum(1)(2)(3)(4)(5); // 15

// manually curried
function curriedSum(v1) {
  return function (v2) {
    return function (v3) {
      return function (v4) {
        return function (v5) {
          return sum(v1, v2, v3, v4, v5);
        };
      };
    };
  };
}

// manually curried with ES6
curriedSum = (v1) => (v2) => (v3) => (v4) => (v5) => sum(v1, v2, v3, v4, v5);

// oneline curry
curriedSum = (v1) => (v2) => (v3) => (v4) => (v5) => sum(v1, v2, v3, v4, v5);
```

### Point free style

[https://github.com/getify/Functional-Light-JS/blob/master/manuscript/ch3.md]

```js

// convenience to avoid any potential binding issue
// with trying to use `console.log` as a function
function output(txt) {
    console.log( txt );
}

function printIf( predicate, msg ) {
    if (predicate( msg )) {
        output( msg );
    }
}

function isShortEnough(str) {
    return str.length <= 5;
}

var msg1 = "Hello";
var msg2 = msg1 + " World";

printIf( isShortEnough, msg1 );         // Hello
printIf( isShortEnough, msg2 );

function isLongEnough(str) {
    return !isShortEnough( str );
}

printIf( isLongEnough, msg1 );
printIf( isLongEnough, msg2 );          // Hello World


// what you would do ... to simplify

function isLongEnough(str) {
    return !isShortEnough( str );
}

printIf( isLongEnough, msg1 );
printIf( isLongEnough, msg2 );          // Hello World

// with point free style

function not(predicate) {
    return function negated(...args){
        return !predicate( ...args );
    };
}

// or the ES6 => arrow form
var not =
    predicate =>
        (...args) =>
            !predicate( ...args );


var isLongEnough = not( isShortEnough );

printIf( isLongEnough, msg2 );          // Hello World

// That's pretty good, isn't it? But we could keep going. printIf(..) could be refactored to be point-free itself.

// We can express the if conditional part with a when(..) utility:

function when(predicate,fn) {
    return function conditional(...args){
        if (predicate( ...args )) {
            return fn( ...args );
        }
    };
}

// or the ES6 => form
var when =
    (predicate,fn) =>
        (...args) =>
            predicate( ...args ) ? fn( ...args ) : undefined;



Partial application is a technique for reducing the arity (that is, the expected number of arguments to a function) by creating a new function where some of the arguments are preset.

Currying is a special form of partial application where the arity is reduced to 1, with a chain of successive chained function calls, each which takes one argument. Once all arguments have been specified by these function calls, the original function is executed with all the collected arguments. You can also undo a currying.

Other important utilities like unary(..), identity(..), and constant(..) are part of the base toolbox for FP.

Point-free is a style of writing code that eliminates unnecessary verbosity of mapping parameters ("points") to arguments, with the goal of making code easier to read/understand.

All of these techniques twist functions around so they can work together more naturally. With your functions shaped compatibly now, the next chapter will teach you how to combine them to model the flows of data through your program.

```

## composition

```js
// words(..) splits a string into an array of words.
function words(str) {
  return String(str)
    .toLowerCase()
    .split(/\s|\b/)
    .filter(function alpha(v) {
      return /^[\w]+$/.test(v);
    });
}

// unique(..) takes a list of words and filters it to not have any repeat words in it.
function unique(list) {
  var uniqList = [];

  for (let v of list) {
    // value not yet in the new list?
    if (uniqList.indexOf(v) === -1) {
      uniqList.push(v);
    }
  }

  return uniqList;
}

var text =
  "To compose two functions together, pass the \
output of the first function call as the input of the \
second function call.";

var wordsFound = words(text);
var wordsUsed = unique(wordsFound);

wordsUsed;
// ["to","compose","two","functions","together","pass",
// "the","output","of","first","function","call","as",
// "input","second"]

// after improvement
var wordsUsed = unique(words(text));

// Though we typically read the function calls left-to-right -- unique(..) and then words(..) -- the order of operations will actually be more right-to-left, or inner-to-outer. words(..) will run first and then unique(..). Later we'll talk about a pattern that matches the order of execution to our natural left-to-right reading, called `pipe(..)`.

// after Lego-ing the 2 functions
// wordsUsed <-- unique <-- words <-- text  (flow of data)
function uniqueWords(str) {
  return unique(words(str));
}
```
