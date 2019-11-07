<div align="center"><img src="https://github.com/bluebill1049/little-state-machine/blob/master/docs/logo.png?raw=true" alt="Little State Machine - React Hooks for state management" width="140px" />
    <h1>♆ Little State Machine</h2>
    
State management made super simple
</div>

<div align="center">

[![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Little-State-Machine&url=https://github.com/bluebill1049/little-state-machine)&nbsp; [![npm downloads](https://img.shields.io/npm/dm/little-state-machine.svg?style=flat-square)](https://www.npmjs.com/package/little-state-machine)
[![npm](https://img.shields.io/npm/dt/little-state-machine.svg?style=flat-square)](https://www.npmjs.com/package/little-state-machine)

</div>

<h2>✨ Features</h2>

- Follow flux application architecture
- Tiny with 0 dependency and simple (less 1kb)
- Persist state by default (`sessionStorage`)
- Build with React Hooks

<h2>📦 Installation</h2>

    $ npm install little-state-machine
 
<h2>🖥 <a href="https://codesandbox.io/s/lrz5wloklm">Demo</a></h2>
Check out the <a href="https://codesandbox.io/s/lrz5wloklm">Demo</a>.
<br />
  
<h2>🕹 API</h2>

##### 🔗 `StateMachineProvider`
This is a Provider Component to wrapper around your entire app in order to create context.

##### 🔗 `createStore`
```
createStore(store, {
 name: string; // rename the store
 middleWares?: Function[]; // function to invoke each action
 syncStores?:  // sync with external store in your session/local storage˚
    | Record<string, string[]>
    | { name: string; transform: Function }
    | undefined;
}})
```

Function to initial the global store, call at app root where `StateMachineProvider` is.

```typescript
import yourDetail from './state/yourDetail';

function log(store) {
  console.log(store);
}

createStore({
  yourDetail, // it's an object of your state { firstName: '', lastName: '' }
}, {
  middleWares: [log], // an array of middleWares, which gets run each actions
  syncStores: { // you can sync with external store and transform the data
    name: 'externalStoreName',
    transform: (externalStore, currentStore) => {
      return { ...externalStore, ...currentStore };
    },
  }
})
```

##### 🔗 `useStateMachine(Action | Actions, Options?) =>`
This hook function will return action/actions and state of the app. 

```typescript
import { updateUserNameAction, removeNameAction } from './actions/yourDetails';

const { action, state } = useStateMachine(updateUserNameAction);
const { actions, state } = useStateMachine({
  removeNameAction,
  updateUserNameAction
});

// The following examples are for optional argument
const { action, state } = useStateMachine(updateUserNameAction, {
  debugName: 'updateUserName' // This will be log in the devTool
});
const { actions, state } = useStateMachine({
  removeNameAction,
  updateUserNameAction
}, {
  removeNameAction: 'removeName',
  updateUserNameAction: 'updateUserName',
});
const { action, state } = useStateMachine(updateUserNameAction, {
  shouldReRenderApp: false // This will prevent App from re-render and only update the store 
});
```

<h2>⚒ DevTool</h2>

Built-in DevTool component to track your state change and action.
```jsx
<StateMachineProvider>
 {process.env.NODE_ENV !== 'production' && <DevTool />}
</StateMachineProvider>
```
<div align="center">
    <img width="500" src="https://github.com/bluebill1049/little-state-machine/blob/master/docs/DevToolScreen.png?raw=true" />
</div>

<h2>📖 Example</h2>

📋 `app.js`
```jsx
import React from 'react'
import yourDetail from './yourDetail'
import YourComponent from './yourComponent'
import { StateMachineProvider, createStore, DevTool } from 'little-state-machine'

// create your store
createStore({
  yourDetail,
});

export default () => {
  return (
    <StateMachineProvider>
      {process.env.NODE_ENV !== 'production' && <DevTool />}
      <YourComponent />
    </StateMachineProvider>
  )
}
```

📋 `yourComponent.js`
```jsx
import React from 'react'
import { updateName } from './action.js'
import { useStateMachine } from 'little-state-machine'

export default function YourComponent() {
  const {
    action,
    state: { yourDetail: { name } },
  } = useStateMachine(updateName);

  return <div onClick={() => action('bill')}>{name}</div>
}
```

📋 `yourDetail.js`
```js
export default {
  name: 'test',
}
```

📋 `action.js`
```js
export function updateName(state, payload) {
  return {
    ...state,
    yourDetail: {
      name: payload
    },
  }
}
```

## Contributors 
Thanks goes to these wonderful people:

<p float="left">
    <a href="https://github.com/flomocommon"><img src="https://avatars2.githubusercontent.com/u/31602074?s=460&v=4" width="50" height="50" /></a>
</p>
