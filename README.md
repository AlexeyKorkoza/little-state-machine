<div align="center"><img src="https://github.com/bluebill1049/little-state-machine/blob/master/docs/logo.png?raw=true" alt="Little State Machine - React Hooks for state management" width="140px" />
    <h1>♆ Little State Machine</h2>
    
State management made super simple
</div>

<div align="center">

[![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Little-State-Machine&url=https://github.com/bluebill1049/little-state-machine)&nbsp; [![npm downloads](https://img.shields.io/npm/dm/little-state-machine.svg?style=flat-square)](https://www.npmjs.com/package/little-state-machine)
[![npm](https://img.shields.io/npm/dt/little-state-machine.svg?style=flat-square)](https://www.npmjs.com/package/little-state-machine)

</div>

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
Function to initial the global store, call at app root where `StateMachineProvider` is.

##### 🔗 `useStateMachine(Action | Actions, Options?) =>`
This hook function will return action/actions and state of the app. 

```typescript
// individual action
Action: (store: Object, payload: any) => void;
// multiple actions
Actions: { [key: string] : Action }
// options to name action in debug, and weather trigger global state update to re-render entire app 
Options?: {
  debugName?: string, // unique debug name can really help you :)
  debugNames?: {[key:string]: string},
  shouldReRenderApp?: boolean, 
}
```

<h2>⚒ DevTool</h2>

Built-in DevTool component to track your state change and action.
```jsx
<StateMachineProvider>
 {process.env.NODE_ENV !== 'production' && <DevTool />}
</StateMachineProvider>
```
<img width="700" src="https://github.com/bluebill1049/little-state-machine/blob/master/docs/DevToolScreen.png" />

<h2>🛠 Window Object</h2>

##### 🔗 `window.STATE_MACHINE_DEBUG`
This will toggle the console output in dev tool.

`window.STATE_MACHINE_DEBUG(true)` to turn debug on in console

`window.STATE_MACHINE_DEBUG(false)` to turn off debug on in console

<img width="700" src="https://github.com/bluebill1049/little-state-machine/blob/master/docs/devtool.png" />

##### 🔗 `window.STATE_MACHINE_RESET`
This will reset the entire store.

`window.STATE_MACHINE_RESET()` to reset the localStorage or sessionStorage

##### 🔗 `window.STATE_MACHINE_GET_STORE`
This will return the entire store.

`window.STATE_MACHINE_GET_STORE()`

##### 🔗 `window.STATE_MACHINE_SAVE_TO`
Save into another session/local storage

`window.STATE_MACHINE_GET_STORE(name: string)`

##### 🔗 `window.STATE_MACHINE_LOAD`
Load saved state into your app, you can either supply a name of your session/local storage, or supply a string of data.

`window.STATE_MACHINE_GET_STORE({ storeName?: string, data?: Object })`

`storeName`: external session/local storage name

`data`: string of data

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
