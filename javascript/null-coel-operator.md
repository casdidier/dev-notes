

```js

// basic understanding

null      ?? 'hi'       // 'hi'
undefined ?? 'hey'      // 'hey'

false     ?? 'hola'     // false
0         ?? 'bonjour'  // 0
'first'   ?? 'second'   // first


// real use case

let person  // <-- person is undefined here

person ?? { name: 'chris' }       // { name: 'chris' }

const isActive = false

isActive ?? true             // false


null ?? undefined ?? false ?? 'hello'     // false
null ?? '' ?? 'hello'


// real world use case

// code shortened for brevity. using fetch requires more code than this
const firstBlogPost = await fetch('...')
const secondBlogPost = await fetch('...')
const defaultBlogPost = { title: 'Default Featured Post' }

const featuredBlogPost = firstBlogPost ?? secondBlogPost ?? defaultBlogPost


```


# Comparison of ?? with logical operators && and ||

Null coalescing operator skips null, undefined
Logical or operator skips null, undefined, false


```js
false ?? 'hello'    // false
false || 'hello'    // 'hello'

```