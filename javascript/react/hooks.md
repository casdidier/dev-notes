# useMemo

[https://maxrozen.com/understanding-when-use-usememo/?utm_source=reactdigest&utm_medium=email&utm_campaign=275]

## How it works

useMemo calls a function when dependencies change, and memoizes (remembers) the result of the function between renders.

to save yourself from rerunning an expensive calculation to generate a new value, and you want to use useCallback to store an existing value.


# useCallBack

## How it works


useCallback returns you a new version of your function only when its dependencies change.
 In the example below, that’s only when a or b changes.

```js
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```


## Use cases

- is useful for passing stable references to functions down to the children of a React component,
 particularly when using those functions in a child’s useEffect.



 # useRef

 ## How it works ?
[https://reactjs.org/docs/hooks-reference.html#useref]
 useRef returns a mutable ref object whose .current property is initialized to the passed argument (initialValue).
 `The returned object will persist` for the full lifetime of the component.

 This works because useRef() creates a plain JavaScript object.
  The only difference between useRef() and creating a {current: ...} object yourself is that `useRef will give you the same ref object on every render`.

 ```js
function TextInputWithFocusButton() {
  const inputEl = useRef(null); // initial value
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}


```

 ## Use cases

 If you want to keep a stable reference to an object/array that doesn’t require recalculation, consider using useRef.