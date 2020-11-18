## getting started

npm install -g expo-cli

### how to start a project

`npm install`
`expo start`
`a`

## Fundamentals

```js
import React from "react";
import { Text, View } from "react-native";

const HelloWorldApp = () => {
  return (
    <View // A View is useful as a container for other components, to help control style and layout.
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      <Text>Hello, world!</Text>
    </View>
  );
};
export default HelloWorldApp;
```

```js
import React from "react";
import { Text, View, StyleSheet } from "react-native";

const styles = StyleSheet.create({
  center: {
    alignItems: "center",
  },
});

const Greeting = (props) => {
  return (
    <View style={styles.center}>
      <Text>Hello {props.name}!</Text>
    </View>
  );
};

const LotsOfGreetings = () => {
  return (
    <View style={[styles.center, { top: 50 }]}>
      <Greeting name="Rexxar" />
      <Greeting name="Jaina" />
      <Greeting name="Valeera" />
    </View>
  );
};

export default LotsOfGreetings;
```

### reminder State vs Props

State#
Unlike props that are read-only and should not be modified, the state allows React components to change their output over time in response to user actions, network responses and anything else.

What's the difference between state and props in React?#
In a React component, the props are the variables that we pass from a parent component to a child component. Similarly, the state are also variables, with the difference that they are not passed as parameters, but rather that the component initializes and manages them internally.

```js
// ReactJS Counter Example using Hooks!

import React, { useState } from 'react';



const App = () => {
  const [count, setCount] = useState(0);

  return (
    <div className="container">
      <p>You clicked {count} times</p>
      <button
        onClick={() => setCount(count + 1)}>
        Click me!
      </button>
    </div>
  );
};


// CSS
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}



// React Native Counter Example using Hooks!

import React, { useState } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  return (
    <View style={styles.container}>
      <Text>You clicked {count} times</Text>
      <Button
        onPress={() => setCount(count + 1)}
        title="Click me!"
      />
    </View>
  );
};

// React Native Styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});






```

## UI toolkit

React Native Elements (My Recommendation): https://react-native-elements.github.io/react-native-elements/

React Native Paper: https://callstack.github.io/react-native-paper/

Native Base: https://nativebase.io/

## Core Components and Native Components

### Native Components (community + yours)

- community-contributed components => https://reactnative.directory/
- you can build your own native components

### [Core Components](https://reactnative.dev/docs/components-and-apis)

## Debugging

CTRL + M : access the menu for the phone debugging menu
