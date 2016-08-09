# Redux

* [Constants](#constants)
* [Action Types](#action-types)
* [Action Creators](#action-creators)
* [Reducers](#reducers)
* [Selectors](#Selectors)
* [Containers](#containers)
* [Resources](#resources)

## Constants

Constants should:
- Be used to define any shared config used by a domain or feature
- Expose a `name` that can be used to namespace Actions and Selectors

#### Example
```js
export const name = 'someDomain'

export const errors = {
  SOME_ERROR: 'Some error'
}
```

## Action Types

Action Types should:
- Be defined in a separate file to Action Creators and Reducers so that a single Action Type can be used by both consumers
- Be namespaced to a domain
- Clearly describe their purpose

#### Example
```js
import { name } from '../constants/domain'

export const SOME_ACTION = `${name}/SOME_ACTION`
```

## Action Creators

Action Creators should:
- Be [Flux Standard Actions](https://github.com/acdlite/flux-standard-action)
- Be [Idiomatic](https://egghead.io/lessons/javascript-redux-simplifying-the-arrow-functions)
- Receive props rather than generate them

#### Example
```js
export const someAction = props => ({
  type: actions.SOME_ACTION,
  payload: {
    ...props
  }
})
```

## Reducers

Reducers should:
- Be pure and free of side-effects
- Only receive data from Actions
- Use `state.merge` wherever possible

#### Example
```js
import { fromJS } from 'immutable'

import * as actions from '../actionTypes/carousel'

export const initialState = fromJS({})

switch(action.type) {

  case actions.INIT: return state.merge({ ...action.payload })
  case actions.UPDATE: return state.mergeDeep({ ...action.payload })
  case actions.TEARDOWN: return state.delete(action.payload.id)
  case actions.RESET: return state.clear()

  default: return state

}
```

## Selectors

Selectors should:
- Be used to memoize state props
- Be used to abstract Immutable `get|getIn` operations away from components
- Be used to select computed/derived data (remove this data from state)

#### Example
```js
import { createSelector } from 'reselect'
import { name } from '../constants/domain'

export const getAll = state => state[name]
export const getSomeProp = state => state[name].get('someProp')

export const getSomeComputedProp = createSelector([ getSomeProp ], (someProp) => someProp + 1)
```

## Containers

Containers should:
- Map state to props
- Map dispatch to props (instead of `this.props.dispatch`)
- Not include markup or styles

#### Examples
```js
import React, { Component } from 'react'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'

import * as DomainActions from '../../actionCreator/domain'

export class Container extends Component {

  componentWillMount() {

    const { domainActions: { init } } = this.props

    init()

  }

  render () {

    const { domain: { someProp } } = this.props

    return (
      <h1>{ someProp }</h1>
    )

  }

}

export default connect(state => ({
    domain: state.domain
  }),
  dispatch => ({
    domainActions: bindActionCreators(DomainActions, dispatch)
  })
)(Container)
```

##### Container with Selectors:
```js
import React, { Component } from 'react'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'
import { createStructuredSelector } from 'reselect'

import * as DomainActions from '../../actionCreator/domain'
import * as DomainSelectors from '../../selectors/domain'

export class Container extends Component {

  render () {

    const { domain: { someProp } } = this.props

    return (
      <h1>{ someProp }</h1>
    )

  }

}

// NOTE: selectors can be nested (and namespaced) to expose multiple parts of the store to component
export default connect(createStructuredSelector({
    domain: createStructuredSelector({
      someProp: DomainSelectors.getSomeProp,
    })
  }),
  dispatch => ({
    domainActions: bindActionCreators(DomainActions, dispatch)
  })
)(Container)
```

## Resources

- [Idiomatic Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)
- [Redux FAQ](https://github.com/reactjs/redux/blob/master/docs/FAQ.md)
