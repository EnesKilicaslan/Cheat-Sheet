# Redux:

- Global state management
- Three concepts: action, reducer and store
- `createStore` function accepts reducer (which holds the state) as a parameter
- `getState` function of the store return the current state
- `subscribe` function accepts another function as an argument and it is called every time on the update of the state.
- `dispatch` is other important function of redux store. It allows us to change the state. It accepts an action(or action creator).
- Instead of extending switch case for each type of action, creating multiple reducers might be more feasible
- To combine multiple reducers into a single reducer "redux" package offers `combineReducers` function
- Middleware allows us to extend redux with custom functionality, such as logging, performing async tasks. It applys to the point between dispatching an action and the moment it reaches the reducer. There is `applyMiddleware` function that takes the function for the specific task.
- `redux-thunk` middleware enables action creator funtion to return an function instead of an action.
