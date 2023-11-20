# Redux_toolKit-Beginner
Basic idea about Redux-toolkit in React Js for Beginner

### Redux:
Redux is a state management tool that makes it easier to pass data between components. Redux provides a store which makes the state inside components easier to maintain. Along with stores, react-redux introduces actions and reducers which work simultaneously with stores to make the state more predictable

### Redux ToolKit(RTK):
Redux Toolkit is used for writing redux code but in a more concise way. Redux Toolkit (RTK) solves problems that most of the developer’s face who used redux in a react application. RTK abstracts the basic redux code and provides us boilerplates that enable us to write redux code in less lines of code

Creating a Redux store in a React application using the Redux Toolkit is quite straightforward and recommended for simplifying the process. Redux Toolkit provides a set of utilities to make working with Redux easier and more efficient. Here's a step-by-step guide on how to create a Redux store using Redux Toolkit in a React application:

## Setup Your React Project:
Ensure you have a React project set up. You can create one using tools like Create React App or set up a React project manually.

## Install Redux Toolkit:
Add the Redux Toolkit and React-Redux packages to your project:

```
npm install @reduxjs/toolkit
```
OR 
```
yarn add @reduxjs/toolkit
```
## Create a Redux Store
Create a file named `src/app/store.js`. Import the `configureStore` API from Redux Toolkit. We'll start by creating an empty Redux store, and exporting it:
```javascript
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```
This creates a Redux store, and also automatically configure the Redux DevTools extension so that you can inspect the store while developing.






## Provide the Redux Store to React
Once the store is created, we can make it available to our React components by putting a React-Redux `<Provider>` around our application in `src/index.js`. Import the Redux store we just created, put a `<Provider>` around your `<App>`, and pass the store as a prop:

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
})
```
## Create a Redux State Slice
Add a new file named `src/features/counter/counterSlice.js`. In that file, import the `createSlice` API from Redux Toolkit.

Creating a slice requires a string name to identify the slice, an initial state value, and one or more reducer functions to define how the state can be updated. Once a slice is created, we can export the generated Redux action creators and the reducer function for the whole slice.
```javascript
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    },
  },
})

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```
## Add Slice Reducers to the Store
Next, we need to import the reducer function from the counter slice and add it to our store. By defining a field inside the `reducer` parameter, we tell the store to use this slice reducer function to handle all updates to that state.
```javascript
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```
## Use Redux State and Actions in React Components
Now we can use the React-Redux hooks to let React components interact with the Redux store. We can read data from the store with `useSelector`, and dispatch actions using `useDispatch`. Create a `src/features/counter/Counter.js` file with a `<Counter>` component inside, then import that component into `App.js` and render it inside of `<App>`.
```javascript
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'

export function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```
`UseSelector: UseSelector hooks allow you to extract data or the state from the Redux store using a selector function.`

`UseDispatch: It allows you to send or dispatch an action to the redux store by giving the action as an argument to the dispatch variable.
It is used to update the state of the component and return a new state.`

Now, any time you click the "Increment" and "Decrement" buttons:

+ The corresponding Redux action will be dispatched to the store
* The counter slice reducer will see the actions and update its state
* The `<Counter>` component will see the new state value from the store and re-render itself with the new data

## Why Should I Use Redux?
Redux can help simplify the state management process, especially when dealing with complex and interconnected components. Here are some reasons why you might want to use Redux in your application:

* ### Centralized state management: 
With Redux, you can maintain the state of your entire application in a single store, making it easier to manage and access data across components.
* ### Predictable state updates: 
Redux has a clear flow of data, which means changes to the state can only happen when you create an action and send it through Redux. This makes it easy to understand how your application's data will change in response to user actions.
* ### Easier debugging: 
With Redux DevTools, you have a clear record of all the changes to your application's state. This makes locating and fixing issues in your code easier, saving you time and effort in the debugging process.
* ### Better performance: 
By minimizing the number of state updates and reducing the need for prop drilling, Redux helps improve your application's performance.
