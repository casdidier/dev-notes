# lazy loading

code splitting, supported by bundlers: create multiple bundles that can be dynamically loaded at runtime

```js
// with functions
import("./math").then(math => {
  console.log(math.add(16, 26));
});

// with react
const OtherComponent = React.lazy(() => import('./OtherComponent'));


// with Suspense and React.lazy
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```


# use of IndexDB instead of localStorage

[https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/indexeddb-best-practices]

- can be a great way to speed up the load time for repeat visits.
The app can then sync up with any API services in the background and update the UI with new data lazily, employing a stale-while- revalidate strategy.

- to store user-generated content,
 either as a temporary store before it is uploaded to the server or as a client-side cache of remote data - or, of course, both.

 # Reducing wasted renders

 Wasted renders are rerenders of components that do not need to be rendered. This is because React rerenders all the components in a tree when a parent component rerenders. This causes wastage of CPU cycles.

 * Use `React.PureComponent` (better than shouldComponentUpdate)
 * "Highlight Updates" in React Dev tools
    - Use `shouldComponentUpdate`to let React know that the component's result is not affected by the state change to control when a component should re-render.
 * https://www.npmjs.com/package/@welldone-software/why-did-you-render to detect them

## example

```js
// Use memo or PureComponent (designed to comparing old and new props before re-rendering)

//  <ComponentB> will re-render even if only props.propA changes value
// because MyApp is actually re-evaluated (or re-rendered 😏) and <ComponentB> is in there.
import React from 'react';

const MyApp = (props) => {
  return (
    <div>
      <ComponentA propA={props.propA}/>
      <ComponentB propB={props.propB}/>
    </div>
  );
};

const ComponentA = (props) => {
  return <div>{props.propA}</div>
};

const ComponentB = (props) => {
  return <div>{props.propB}</div>
};

//  using memo
import React, { memo } from 'react';

// 🙅‍♀️
const ComponentB = (props) => {
  return <div>{props.propB}</div>
};

// 🙆‍♂️
const ComponentB = memo((props) => {
  return <div>{props.propB}</div>
});


//  using PureComponent
import React, { Component, PureComponent } from 'react';

// 🙅‍♀️
class ComponentB extends Component {
  render() {
    return <div>{this.props.propB}</div>
  }
}

// 🙆‍♂️
class ComponentB extends PureComponent {
  render() {
    return <div>{this.props.propB}</div>
  }
}


// TODO example
// useMemo as a similar way to prevent unnecessary computational work

```


# Better tree shaking

  * Tree shaking is the process of removing unused or dead code from the bundles.
  This is especially important when using utility libraries like Lodash where you do not need all features of the imported library.

# Service Workers and PWA

# Prefetching

# Optimizing list rendering with React Virtualized List:
[https://www.simform.com/react-performance/]

# Avoid Anonymous Functions

```js

import React from 'react';

function Foo() {
  return (
    <button onClick={() => console.log('boop')}> // 🙅‍♀️
      BOOP
    </button>
  );
}

```

Since anonymous functions aren’t assigned an identifier (via const/let/var),
they aren’t persistent whenever this functional component inevitably gets rendered again.
 This causes JavaScript to allocate new memory each time this component is re-rendered instead of allocating a single piece of memory only once when using “named functions”:


```js

import React, { useCallback } from 'react';

// Variation 1: naming and placing handler outside the component
const handleClick = () => console.log('boop');
function Foo() {
  return (
    <button onClick={handleClick}>  // 🙆‍♂️
      BOOP
    </button>
  );
}

// Variation 2: "useCallback"
function Foo() {
  const handleClick = useCallback(() => console.log('boop'), []);
  return (
    <button onClick={handleClick}>  // 🙆‍♂️
      BOOP
    </button>
  );
}

```

# Avoid Object Literals

This performance tip is similar to the previous section about anonymous functions. Object literals don’t have a persistent memory space,
 so your component will need to allocate a new location in memory whenever the component re-renders:

 [https://www.digitalocean.com/community/tutorials/react-keep-react-fast#avoid-object-literals]

```js
 function ComponentA() {
  return (
    <div>
      HELLO WORLD
      <ComponentB style={{  {/* 🙅‍♀️ */}
        color: 'blue',
        background: 'gold'
      }}/>
    </div>
  );
}

function ComponentB(props) {
  return (
    <div style={this.props.style}>
      TOP OF THE MORNING TO YA
    </div>
  )
}
```

better below

```js

const myStyle = {  // 🙆‍♂️
  color: 'blue',
  background: 'gold'
};
function ComponentA() {
  return (
    <div>
      HELLO WORLD
      <ComponentB style={myStyle}/>
    </div>
  );
}

function ComponentB(props) {
  return (
    <div style={this.props.style}>
      TOP OF THE MORNING TO YA
    </div>
  )
}

```


# Avoid unmounting/mounting

Many times we’re used to making components disappear using a ternary statement (or something similar):

```js
import React, { Component } from 'react';
import DropdownItems from './DropdownItems';

class Dropdown extends Component {
  state = {
    isOpen: false
  }
  render() {
    <a onClick={this.toggleDropdown}>
      Our Products
      {
        this.state.isOpen
          ? <DropdownItems>
          : null
      }
    </a>
  }
  toggleDropdown = () => {
    this.setState({isOpen: !this.state.isOpen})
  }
}

```

n order to mitigate this, it’s advisable to avoid completely unmounting components.
Instead, you can use certain strategies like setting the CSS opacity to zero, or setting CSS visibility to “none”. This will keep the component in the DOM,
while making it effectively disappear without incurring any performance costs.