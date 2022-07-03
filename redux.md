# Redux:

## Core Concepts

- Global state management
- Three concepts: action, reducer and store
- `createStore` function accepts reducer (which holds the state) as a parameter
- `getState` function of the store return the current state
- `subscribe` function accepts another function as an argument and it is called every time on the update of the state.
- `dispatch` is other important function of redux store. It allows us to change the state. It accepts an action(or action creator).
- Instead of extending switch case for each type of action, creating multiple reducers might be more feasible
- To combine multiple reducers into a single reducer "redux" package offers `combineReducers` function
- Middleware allows us to extend redux with custom functionality, such as logging, performing async tasks. It applys to the point between dispatching an action and the moment it reaches the reducer. There is `applyMiddleware` function that takes the function for the specific task.
- `redux-thunk` middleware enables action creator funtion to return another function instead of an action.


## React-Redux

- The core concept holds true for a React application as well.
- We can create a folder structure according to our needs, one famous is creating a redux folder together with state sub folder and putting all related operations (reducer, action, types) in it.

```
redux
--user
----userTypes
----userActions
----userReducers
--cake
----cakeTypes
----cakeActions
----cakeReducers
```

- `react-redux` library gives us a `Provider` component that provides the state to the react app.

```javascript
import { Provider } from 'react-redux'
import store from './redux/store'

function App () {
  return (
    <Provider store={store}>
      <div className='App'>
      
        ....
        
      </div>
    </Provider>
  )
}
```
- Once we provide the store to the React application, we can get and change the state within any component in the react application.
- 'react-redux' library provides `connect` HOC, we wrap our component with our own `mapStateToProps` and `mapDispatchToProps` as show below. By doing so, we can get state and dispatch an action from the component
```javascript
import React from 'react'
import { connect } from 'react-redux'
import { buyCake } from '../redux'

function CakeContainer (props) {
  return (
    <div>
      <h2>Number of cakes - {props.numOfCakes} </h2>
      <button onClick={props.buyCake}>Buy Cake</button>
    </div>
  )
}

const mapStateToProps = state => {
  return {
    numOfCakes: state.cake.numOfCakes
  }
}

const mapDispatchToProps = dispatch => {
  return {
    buyCake: () => dispatch(buyCake())
  }
}

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(CakeContainer)
```

- React Hooks can be used to access the state and dispatch action instead of HOC
- `useSelector` hook is equivalent of `mapStateToProps` function
- `useDispatch` hook is almost equivalent of `mapDispatchToProps` funciton, it gives use the `dispatch` function itself
- `mapStateToProps` function can take second parameter, named ownProps as convention. The second parameter is the props that has already been passed to the component and it is used for conditional mapping.
- `mapStateToProps` function can take second parameter, named ownProps as convention. Similar usage as in `mapStateToProps`
- if we only want to dispatch actions but not to subscribe to the state changes, then we can pass `null` instead of `mapStateToProps` to the connect function.
- store can be created with redux-thunk middleware as following
```javascript
import { createStore, applyMiddleware } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import thunk from 'redux-thunk'

import rootReducer from './rootReducer'

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(thunk))
)

export default store
```

- async action can be created as shown folowing
```javascript
import axios from 'axios'
import {
  FETCH_USERS_REQUEST,
  FETCH_USERS_SUCCESS,
  FETCH_USERS_FAILURE
} from './userTypes'

export const fetchUsers = () => {
  return (dispatch) => {
    dispatch(fetchUsersRequest())
    axios
      .get('https://jsonplaceholder.typicode.com/users')
      .then(response => {
        // response.data is the users
        const users = response.data
        dispatch(fetchUsersSuccess(users))
      })
      .catch(error => {
        // error.message is the error message
        dispatch(fetchUsersFailure(error.message))
      })
  }
}

export const fetchUsersRequest = () => {
  return {
    type: FETCH_USERS_REQUEST
  }
}

export const fetchUsersSuccess = users => {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: users
  }
}

export const fetchUsersFailure = error => {
  return {
    type: FETCH_USERS_FAILURE,
    payload: error
  }
}
```

