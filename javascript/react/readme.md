# React

React is a philosophy as much as a framework. When migrating to React from other approaches (eg. traditional MVC frameworks, jQuery etc) you need to *unlearn* the old ways of writing JS applications. [Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html) is a good way to start this journey.

## Components

A key concept to understand when building React applications is [Containers vs Presentational components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.iore1k7hp).

### Containers

Containers should:
- Be [ES6 classes](https://facebook.github.io/react/docs/reusable-components.html#es6-classes) not `React.createClass`
- Follow the [Redux guidelines](https://github.com/unlease-development/wiki/tree/master/javascript/redux#containers)

### Presentational

Presentational components should:
- Be [stateless, functional components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) when lifecycle events are not needed
- Resuable whenever applicable (*is component specific to this domain or could it be used by multiple domains?*)

```js
// `react` is required for JSX rendering
import React from 'react'

const Component = props => {

  return (
    <div>Content here</div>
  )

}

export default Component
```

## Composition

TODO - `children`, https://github.com/krasimir/react-in-patterns/tree/master/patterns/composition

## Callback scope

To maintain the scope of callbacks passed from a parent to a child component it is necessary to `bind` the callback to the parent's scope. This could be done like so:
```html
<button onClick={ this.onButtonClick.bind(this) }>Go!</button>
```

However, this is an anti-pattern as it causes the `bind` function to be called *every* time the button is rendered. Instead, callback bindings should be created in the component's `constructor` method:
```js
class Container extends React.Component {

  constructor() {

    super()

    this.onButtonClick = this.onButtonClick.bind(this)

  }

  onButtonClick() {
    // ...
  }

  render() {

    const onButtonClick = this.onButtonClick

    return (
      <button onClick={onButtonClick}>Go!</button>
    )

  }

}
```

## DOM

TODO - data as props https://github.com/krasimir/react-in-patterns/tree/master/patterns/communication, refs, classes, unidirectional flow

## Classnames

[`classnames`](https://github.com/JedWatson/classnames) can be used to apply dynamic classes to elements.

- `classnames` can be used to apply BEM classes by using a 'base class' and [ES6 computed properties](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer):

```js
const baseClass = 'button'

let className = classNames(baseClass, {
  [`${baseClass}--rounded`]: isRounded,
  [`${baseClass}--disabled`]: isDisabled
})
```

- Only use `classnames` for dynamic classes **not static**:

```js
let className = classNames({
  'input': true,
  'input--red': true,
  'input--border': true
})
```

## Resources
- [React in patterns](https://github.com/krasimir/react-in-patterns)
- [Future React proposals](https://github.com/reactjs/react-future)
