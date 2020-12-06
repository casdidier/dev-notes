## Functionnal component


```ts
type CardProps = {
  title: string,
  paragraph: string
}

// we can use children even though we haven't defined them in our CardProps
export const Card: FunctionComponent<CardProps> = ({ title, paragraph, children }) => <aside>
  <h2>{ title }</h2>
  <p>
    { paragraph }
  </p>
  { children }
</aside>



```

### defaultProps


```ts
import React, { Component } from 'react';

type NoticeProps = {
  msg: string
}

export class Notice extends Component<NoticeProps> {
  static defaultProps = {
    msg: 'Hello everyone!'
  }

  render() {
    return <p>{ this.props.msg }</p>
  }
}

const el = <Notice /> // Will compile in TS 3.0

```


## Events

```ts

import React, { Component, MouseEvent } from 'react';

export class Button extends Component {
  handleClick(event: MouseEvent) {
    event.preventDefault();
    alert(event.currentTarget.tagName); // alerts BUTTON
  }

  /*
   Here we restrict all handleClicks to be exclusively on
   HTMLButton Elements
  */
  handleClickRestricted(event: MouseEvent<HTMLButtonElement>) {
    event.preventDefault();
    alert(event.currentTarget.tagName); // alerts BUTTON
  }

  /*
    Generics support union types. This event handler works on
    HTMLButtonElement and HTMLAnchorElement (links).
  */
  handleAnotherClick(event: MouseEvent<HTMLButtonElement | HTMLAnchorElement>) {
    event.preventDefault();
    alert('Yeah!');
  }

  render() {
    return <button onClick={this.handleClick}>
      {this.props.children}
    </button>
  }
}

```

## PropTypes

npm install --save prop-types
npm install --save-dev @types/prop-types

```ts

import React from "react";
import PropTypes from "prop-types";



export function Article ({ title, price })  {
  return (
    <div className="article">
      <h1>{title}</h1>
      <span>Priced at (incl VAT): {price * 1.2}</span>
    </div>
  );
}

Article.propTypes = {
  title: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
};

// Combined with defaultProps
Article.defaultProps = {
  price: 20
};


```


### Children
```ts
export function ArticleList({
  children
}: InferProps<typeof ArticleList.propTypes>) {
  return <div className="list">{children}</div>;
}

ArticleList.propTypes = {
  children: PropTypes.oneOfType([
    PropTypes.arrayOf(PropTypes.node),
    PropTypes.node
  ]).isRequired
};
```