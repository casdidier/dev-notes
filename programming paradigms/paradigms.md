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