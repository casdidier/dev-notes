## use of hooks and context

I would like to store information in a context object and then use later in another component

Below is the context file

```jsx
// SignupFormContext.js
import React, { createContext, useContext, useState } from 'react';

export const SignupFormContext = createContext();

export const useSignupForm = () => useContext(SignupFormContext); // export custom hook

export function SignupFormProvider({ children }) {
  const [profile, setProfile] = useState({});
  const [social, setSocial] = useState({});

  return (
    <SignupFormContext.Provider
      value={{ profile, setProfile, social, setSocial }}
    >
      {children}
    </SignupFormContext.Provider>
  );
}


// social form
import React from 'react';
import { useForm } from 'react-hook-form';
import { useHistory } from 'react-router-dom';
import { useSignupForm } from './SignupFormContext';
import Animator from './Animator';

export default function SocialForm() {
  const { register, handleSubmit, errors } = useForm();
  // const history = useHistory();
  const { social, setSocial } = useSignupForm();

  function onSubmit(data) {
    setSocial(data);
    // history.push('/review');
  }

  return (
    <Animator>
      <form onSubmit={handleSubmit(onSubmit)} noValidate>
        <h2>How can we find you on social?</h2>

        <input
          type="text"
          name="twitter"
          placeholder="What's your Twitter?"
          defaultValue={social.twitter}
          ref={register({ required: true })}
        />
        <p>{errors.twitter && 'Twitter is required.'}</p>

        <input
          type="text"
          name="facebook"
          placeholder="What's your Facebook?"
          defaultValue={social.facebook}
          ref={register({
            required: true,
          })}
        />
        <p>{errors.facebook && 'Facebook is required.'}</p>

        <input type="submit" value="Next" />
      </form>
    </Animator>
  );
}


//  SignupForm.js (inside App.js)

import React from 'react';
import { Switch, Route, useLocation } from 'react-router-dom';
import ProfileForm from './1ProfileForm';
import SocialForm from './2SocialForm';
import Review from './3Review';
import StepLinks from './StepLinks';
import { SignupFormProvider } from './SignupFormContext';

export default function SignupForm() {
  const location = useLocation();

  return (
    <SignupFormProvider>
      <div className="signup-form">
        {/* show the steps and links */}
        <StepLinks />

        {/* show the forms */}
          <Switch location={location} key={location.pathname}>
            <Route path="/" exact component={ProfileForm} />
            <Route path="/social" component={SocialForm} />
            <Route path="/review" component={Review} />
          </Switch>
      </div>
    </SignupFormProvider>
  );
}


```


# ways to share stateful logic between components

## render props


## higher-order components

# pass information between hooks

Since Hooks are functions, we can pass information between them.

Because the useState Hook call gives us the latest value of the recipientID state variable, we can pass it to our custom useFriendStatus Hook as an argument:

```js
const [recipientID, setRecipientID] = useState(1);
  const isRecipientOnline = useFriendStatus(recipientID);

```


# a complex component that contains a lot of local state

## use a `redux` reducer

```js
function todosReducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, {
        text: action.text,
        completed: false
      }];
    // ... other actions ...
    default:
      return state;
  }
}


function useReducer(reducer, initialState) {
  const [state, setState] = useState(initialState);

  function dispatch(action) {
    const nextState = reducer(state, action);
    setState(nextState);
  }

  return [state, dispatch];
}


function Todos() {
  const [todos, dispatch] = useReducer(todosReducer, []);

  function handleAddClick(text) {
    dispatch({ type: 'add', text });
  }

  // ...
}
```

