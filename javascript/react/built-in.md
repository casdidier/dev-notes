## rendering

A component re-renders when props changing, or useState being called.
Since by default all of SomeComponent’s code re-runs on render, getUrl gets re-defined every render.

```js

function SomeComponent(props){
  function getUrl(id){
    return "https://some-api-url.com/api/" + id + "/"
  }
  //do some other stuff here that might cause re-renders, like setting state
  return <DataFetcher getUrl={getUrl}>

  // If DataFetcher then uses getUrl as part of a useEffect hook,
  //even if you add getUrl to the dependency array, your useEffect would fire every single render.


}

function DataFetcher(props){
    useEffect(() => {
  fetchDataToDoSomething(getUrl);
}, [getUrl]); // 🔴 re-runs this effect every render

  //
  }

```

<!-- How do we stop useEffect from running every render? -->
[https://maxrozen.com/stop-useeffect-running-every-render-with-usecallback/]

The old fashioned way: move getUrl outside of the component, so it doesn’t get re-declared every render:

```js
function getUrl(id){
  return "https://some-api-url.com/api/" + id + "/"
}

function SomeComponent(props){
  //do some other stuff here that might cause re-renders, like setting state
  return <DataFetcher getUrl={getUrl}>
}

```

The Hooks way, which is to wrap getUrl in a useCallback:

```js
function SomeComponent(props){
  const getUrl = useCallback(function (id) {
    return "https://some-api-url.com/api/" + id + "/";
  }, []); // <-- Note that we can't add id to the deps array in this case

  //do some other stuff here that might cause re-renders, like setting state
  return <DataFetcher getUrl={getUrl}>
}


```