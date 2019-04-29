# @zumper/redux-ignore

**NOTE:** fork of https://github.com/omnidan/redux-ignore

---

_higher-order reducer to ignore redux actions_

[can be used to avoid performance problems in large apps by skipping reducer subtrees](https://twitter.com/dan_abramov/status/656049225566920704)

## Installation

```
yarn add @zumper/redux-ignore
```

## API

```js
import { ignoreActions, filterActions } from '@zumper/redux-ignore'

ignoreActions(reducer, [ARRAY_OF_ACTIONS])
ignoreActions(reducer, (action) => !action.valid)

filterActions(reducer, [ARRAY_OF_ACTIONS])
filterActions(reducer, (action) => action.valid)
```

## Ignoring Actions

`@zumper/redux-ignore` is a reducer enhancer (higher-order reducer), it provides the
`ignoreActions` function, which takes an existing reducer and either:

- An array of actions to be ignored, or...
- A predicate function for filtering out actions.

Firstly, import `@zumper/redux-ignore`:

```js
// Redux utility functions
import { combineReducers } from 'redux'
// redux-ignore higher-order reducer
import { ignoreActions } from '@zumper/redux-ignore'
```

Then, add `ignoreActions` to your reducer(s) like this:

```js
combineReducers({
  counter: ignoreActions(counter, [INCREMENT_COUNTER]),
})
```

Now you won't be able to increment the counter anymore, because the
`INCREMENT_COUNTER` action is ignored.

Alternatively, you can ignore actions via a predicate function:

```js
combineReducers({
  counter: ignoreActions(
    counter,
    (action) => action.type === INCREMENT_COUNTER
  ),
})
```

## Filtering Actions

You can also use `filterActions` to only accept actions that are declared in an array, or that satisfy the predicate function:

```js
import { combineReducers } from 'redux'
import { filterActions } from '@zumper/redux-ignore' // pull in the filterActions function
import { STAY_COOL, KEEP_CHILLIN } from './actions'
import { counter, notACounter } from './reducers'

combineReducers({
  counter: filterActions(counter, (action) => action.type.match(/COUNTER$/)), // only run on actions that satisfy the regex
  notACounter: filterActions(notACounter, [STAY_COOL, KEEP_CHILLIN]), // only run for these specific relaxing actions
})
```

## What is this magic? How does it work?

Have a read of the [Implementing Undo History recipe](http://redux.js.org/docs/recipes/ImplementingUndoHistory.html)
in the Redux documents, which explains in detail how higher-order reducers work.

## License

MIT, see `LICENSE.md` for more information.
