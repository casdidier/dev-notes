# React + Typescript

ressource => https://slides.benmvp.com/2020/tsconf/react.html#/1
[Cheat sheets](https://github.com/typescript-cheatsheets/react)
[learning plan](https://github.com/antonjb/TypeScript-Learning-Plan)
[playground](https://www.typescriptlang.org/docs/handbook/intro.html)

## SETUP: start a project with TS

### with Create React App

`npx create-react-app my-app --template typescript
[https://create-react-app.dev/docs/adding-typescript/]

### without Create React App

#### @babel/preset-typescript

[https://babeljs.io/docs/en/babel-preset-typescript]

#### using a CLI tool tsdx

https://github.com/formium/tsdx

### TS eslint

https://github.com/typescript-eslint/typescript-eslint/tree/master/packages/eslint-plugin

### Node.js start [https://basarat.gitbook.io/typescript/nodejs]

TypeScript has had first class support for Node.js since inception. Here's how to setup a quick Node.js project:
Note: many of these steps are actually just common practice Node.js setup steps

1. Setup a Node.js project package.json. Quick one : npm init -y
2. Add TypeScript (npm install typescript --save-dev)
3. Add node.d.ts (npm install @types/node --save-dev)
4. Init a tsconfig.json for TypeScript options with a few key options in your tsconfig.json
   `npx tsc --init --rootDir src --outDir lib --esModuleInterop --resolveJsonModule --lib es6,dom --module commonjs`
   That's it! Fire up your IDE (e.g. code .) and play around. Now you can use all the built in node modules (e.g. import \* as fs from 'fs';) with all the safety and developer ergonomics of TypeScript!
   All your TypeScript code goes in src and the generated JavaScript goes in lib.

## Props

```ts
interface AppProps {
  // define props and types
}

const App = (props : AppProps) {
  // use props and render UI
}


```

1. Props must be listed
2. Props are required by default
   - Prop can be set optional below

```ts
interface AppProps {
  // define props and types
  message : string[],
  count? : number // optional
}

const App = ({ players, count = 2 } : AppProps) { // use of default destructuring to handle the required and non required
  // use props and render UI

}

```

3. Prop refactors are caught
4. Can't avoid defining complex objects

```ts
interface Address {
  /* city, is Primary, etc... */
}

interface User {
  name: string;
  addresses: {
    shipping: Address;
    billing?: Address;
  };
}

interface AppProps {
  user: User;
}

const App = ({ user: User }) => {
  const { city, isPrimary } = user.addresses.shipping;
};
```

5. Functions props have explicit signature

```ts
interface Props {
  children: (user: User) => React.ReactNode;
}

const UsersList = ({ users, children }: Props) => {
  <>
    {users.map((user) => {
      <div>{children(user)}</div>;
    })}
  </>;
};
```

6. Rest Props are also typed

```ts
interface Props {
  children: (user: User) => React.ReactNode;
  type: string;
  name: string;
}

const UsersList = ({ users, children, ...otherProps }: Props) => {
  <>
    {users.map((user) => {
      <div>{children(user)}</div>;
    })}
  </>;
};

const User = ({ name, type, lastName }: Props) => {
  // will fail because lastName is not defined in the interface Props
};
```

## Hooks

1. useState

with initial value, TS uses typeinference so you already are typedsafe

```ts
const App = () => {
  const [count, setCount] = useState(0);

  const Add = () => {
    setCount((prevCount) => prevCount + 1);
  };

  return <button onClick={add}>Add</button>;
};
```

```ts
const App = () => {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    getUserApi().then((data) => {
      setUser(data.user);
    });
  }, []);

  return user ? <User user={user} /> : null;
};
```

### What are TS generics ? [https://www.youtube.com/watch?v=nePDL5lQSE4&list=WL&index=282&t=1s]

2. useEffect

TS makes sure you just return a clean up function

3. useReducer

using **Discriminating Unions**

```ts
// using **Discriminating Unions** to enforce the types of pairs that can occur
type State =
  | { status: "loading" }
  | { status: "failed"; message: string }
  | { status: "success"; items: Item[] };

type Action =
  | { type: "loading" }
  | { type: "success"; payload: Item[] }
  | { type: "failed"; error: Error };

const reducer = (state: State, action: Action) => {
  switch (
    action.type
    // do things
  ) {
  }

  const initialState = { status: "loading" };

  const Items = () => {
    const [state, dispatch] = useReducer(reducer, initialState);

    useEffect(() => {
      getItems().then(
        (data) => dispatch({ type: "success", payload: data }),
        (error) => dispatch({ type: "failed", error })
      );
    }, []);
  };
};
```

## Advanced Patterns [https://www.youtube.com/watch?v=xfcPUP2_J9E&feature=emb_title]

1. One with the other

//TODO

2. Extending HTML components

**Omit** //TODO

3. Parametrized props
