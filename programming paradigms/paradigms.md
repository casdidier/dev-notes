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
people.map( function getPreferredName(person){
    return person.nicknames[0] || person.firstName;
} )
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
var getCurrentUser = function outerPartiallyApplied(...outerLaterArgs){
    var getPerson = function innerPartiallyApplied(...innerLaterArgs){
        return ajax( "http://some.api/person", ...innerLaterArgs );
    };

    return getPerson( { user: CURRENT_USER_ID }, ...outerLaterArgs );
}



function partialRight(fn,...presetArgs) {
    return function partiallyApplied(...laterArgs){
        return fn( ...laterArgs, ...presetArgs );
    };
}

// or the ES6 => arrow form
var partialRight =
    (fn,...presetArgs) =>
        (...laterArgs) =>
            fn( ...laterArgs, ...presetArgs );


```


### curry

A technique similar to partial application, where a function that expects multiple arguments is broken down into successive chained functions that each take a single argument (arity: 1) and return another function to accept the next argument

```js
var curriedAjax = curry( ajax );

var personFetcher = curriedAjax( "http://some.api/person" );

var getCurrentUser = personFetcher( { user: CURRENT_USER_ID } );

getCurrentUser( function foundUser(user){ /* .. */ } );


[1,2,3,4,5].map( curry( add )( 3 ) );
// [4,5,6,7,8]


var adder = curry( add );

// It might be helpful in the case where you know ahead of time that add(..) is the function to be adapted, but the value 3 isn't known yet:

// later
[1,2,3,4,5].map( adder( 3 ) );
// [4,5,6,7,8]


```

```The advantage of currying here is that each call to pass in an argument produces another function that's more specialized, and we can capture and use that new function later in the program```

FP use closure to remember the arguments over time until all have been received, and then the original function can be invoked.


```js

function sum(...nums) {
    var total = 0;
    for (let num of nums) {
        total += num;
    }
    return total;
}

sum( 1, 2, 3, 4, 5 );                       // 15

// now with currying:
// (5 to indicate how many we should wait for)
var curriedSum = curry( sum, 5 );

curriedSum( 1 )( 2 )( 3 )( 4 )( 5 );        // 15

// manually curried
function curriedSum(v1) {
    return function(v2){
        return function(v3){
            return function(v4){
                return function(v5){
                    return sum( v1, v2, v3, v4, v5 );
                };
            };
        };
    };
}

// manually curried with ES6
curriedSum =
    v1 =>
        v2 =>
            v3 =>
                v4 =>
                    v5 =>
                        sum( v1, v2, v3, v4, v5 );


// oneline curry
curriedSum = v1 => v2 => v3 => v4 => v5 => sum( v1, v2, v3, v4, v5 );



```