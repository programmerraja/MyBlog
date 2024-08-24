 +++
title = 'React'
date = 2024-01-20T18:57:02.022+05:30
draft = false
+++  


## Notes

##### Context

```js
function App() {
  return (
    <div className="App">
      <AdaptivityProvider>
        <Child1 />
        <Child2 />
      </AdaptivityProvider>
    </div>
  );
}

let child1 = 0;
let child2 = 0;

function Child1() {
 //TO Consume
  const { width } = useSize();
  child1 += 1;
  return <p>{child1}</p>;
}

function Child2() {
  const { isMobile } = useMobile();
  child2 += 1;
  return <p>{child2}</p>;
}
//create context
const SizeContext = createContext({});
const MobileContext = createContext({});

//create provider
export const Provider = (props) => {
  const [width, setWidth] = useState(window.innerWidth);

  useLayoutEffect(() => {
    const onResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize", onResize);
    return () => window.removeEventListener("resize", onResize);
  }, []);

  const isMobile = width <= 680;

  return (
    <SizeContext.Provider value={width}>
      <MobileContext.Provider value={isMobile}>
        {props.children}
      </MobileContext.Provider>
    </SizeContext.Provider>
  );
};

export const useSize = () => {
  return useContext(SizeContext);
};

export const useMobile = () => {
  return useContext(MobileContext);
};

```

Don't have single context for width and mobile beacuse when ever the width change the component that only using mobile (`child2`) will also get re-render. this will applicable only if `Child2` is Mounted to DOM

When should I use Context?
	- Any time you have some value that you want to make accessible to a portion of your React component tree, without passing that value down as props through each level of components.

## Forward Ref and useImperativeHandle

Forward Ref are used pass ref from parent to child and useImperativeHandle is used to limit the ref functionality to the parent who passed or we can use this if we want to access child method on parent.


```js
import React, { forwardRef, useRef, useImperativeHandle } from 'react';

// ChildComponent is a functional component that forwards its ref to the underlying DOM element.
const ChildComponent = forwardRef((props, ref) => {
  const inputRef = useRef();

  // Expose the inputRef as part of the component's ref.
  useImperativeHandle(ref, () => ({
    focus: () => {
	  //we can access the child all var if we want in parent
      inputRef.current.focus();
    },
    getValue: () => {
      return inputRef.current.value;
    },
  }));

  return (
    <input
      type="text"
      placeholder="Type something"
      ref={inputRef}
    />
  );
});

// ParentComponent renders the ChildComponent and uses its ref.
const ParentComponent = () => {
  const childRef = useRef();

  const handleButtonClick = () => {
    // Accessing the forwarded ref methods.
    console.log('Input Value:', childRef.current.getValue());
    childRef.current.focus();
  };

  return (
    <div>
      <ChildComponent ref={childRef} />
      <button onClick={handleButtonClick}>Focus Input</button>
    </div>
  );
};

export default ParentComponent;

```
## Tips And Tricks

- Don't return Null on a component which cause the component to unmount if you want mount again if it based on conditon use boolean it wil be not dispalayed in UI but will be avalible in V-DOM

- **Profiling Tips:** When you do profiling using react profiler try make CPU 2x or some lower bound slow down to find the issue beacuse most of the modern system will do better and one more tips when looking the flame chart check for the single task duration that need to be less then **~16 milliseconds** (`Human eyes are sensitive to motion, and frame rates below 60 fps may result in perceptible stuttering or flickering in animations`) if it taking more then 16 Milliseconds it will be laggy in UI 

-  useState for one-time initializations ` const [resource] = React.useState(() => new Resource())`

- React chilldern will add key `React.Children.toArray(someData.map(item=><div>{item.title}</div>)` 

- Toggle CSS instead of forcing a component to mount and unmount

- Use virtualization for large lists

- Whenever you have JSX repeating itself, extract the logic to a config object and loop through it.
#### UseRef

- use instead of useCallback 

```js
const onClick = useRef(() => setClicks(c => c++)).current;  
// now we can just  
onClick={onClick}
```

- Dont carry current destruct it 
```js
const gesture = useRef({  
startX: 0,  
startY: 0,  
startT: 0,  
}).current;
```

- Getter and setter in ref
```js
function useStateRef(init) {  
const ref = useRef(init);  
const setter = useRef((v) => ref.current = v).current;  
const getter = useRef(() => ref.current).current;  
return [getter, setter];  
}  
// usage example  
const [startX, setStartX] = useStateRef(0);  
return <div  
onTouchStart={(e) => setStartX(e.clientX)}  
onTouchMove={(e) => setOffset(e.clientX - startX())}  
>{children}</div>
```


## Context

- Not all the chilldern under context re-render the chillderen that consuming the value only re-render beacuse we used component composition here passed as chilldren

- When we using useContext() in child component that will only get re-render

- So always keep the useContext where it needed

```js
<MobileContext.Provider value={isMobile}>
  <SizeContext.Provider value={width}>
	{props.children}
 </SizeContext.Provider>
</MobileContext.Provider>
```

- So use the context where it needed if use the context in Modal component it will be re-render on context change.

```js
const ModalClose = () => {
  const { isMobile } = useContext(MobileContext);
  return isMobile ? null : <div className="Modal__close" onClick={onClose} />;
};

const Modal = ({ children, onClose }) => {
  // a lot of modal logic with timeouts, effects and stuff
  return (
    <div className="Modal">
      {/* a lot of modal layout */}
      <ModalClose />
    </div>
  );
};
```

- Always try to pass the primitive data type to provider vaule or wrapp with memo because when the component above provider is changing which cause the Provider to re render and if we using no primitive data type like object which will change and react context will re-render all the chillderen when the vaule change 

```js
function Provider(){
const [width,setWidth] = useState(0);
let isMobile = width > 1400;
//{width,setWidth} -> we passing as object will be created new on each render due the APP state change which cause all the child to render again to avoid wrap with memo  or seprate the setter and getter in different context
//const value = useMemo(()=>{width,setWidth},[])
return (
<MobileContext.Provider value={isMobile}>
  <SizeContext.Provider value={{width,setWidth}}>
	{props.children}
 </SizeContext.Provider>
</MobileContext.Provider>
)
}
//if we using this provider 

function ComponentThatChange() {

const [state,setState] = useState(0);
//when ever state change which casue the provider to re-render 
return (
	<Provider>
		<Child1/>
		<Child1/>
	</Provider>
)
}
```

## Patterns

#### Component composition

**passing components as props to other components**

Multiple components work together to achieve the functionality of a single entity.This will be usefull when we want props drilling we can use this patterns



```js
 <Homepage
	leftNav={
	  <LeftNav>
		<DashboardDropdown />
		<Repositories />
		<Teams />
	  </LeftNav>
	}
	centerContent={
	  <CenterContent>
		<RecentActivity />
		<AllActivity />
	  </CenterContent>
	}
	rightContent={
	  <RightContent>
		<Notices />
		<ExploreRepos />
	  </RightContent>
      }
  />
```

Use this when we have parent component and that have 5 chillderen and we used to change the state of parent that not relvant for the chillderen wrap with composition

```js
function Parent({chillderen}){
	const [state,setState] = useState();

	return (
	//custom logic and parent logic
	{chillderen}
	)

}

function compositonWrapper(){
	return (
		<Parent>
			<Child1/>
			<Child2/>
			<Child3/>
		</parent>
	)
}
```

Refer : [github](https://github.dev/reach/reach-ui/tree/main/packages/tabs)
#### Render Prop

 A render prop is a prop on a component, which value is a function that returns a JSX element. The component itself does not render anything besides the render prop. Instead, the component simply calls the render prop, instead of implementing its own rendering logic. 

It is similar to higher order component

```js

import React, { useState } from 'react';

class MouseTracker extends React.Component {
  state = { x: 0, y: 0 };

  handleMouseMove = (event) => {
    this.setState({ x: event.clientX, y: event.clientY });
  };

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>
        {this.props.render(this.state)}
      </div>
    );
  }
}

function App(){
return (

<MouseTracker 
	render={({ x, y }) => ( <p> Mouse position: ({x}, {y}) </p> )} 
/>
)
}

```


### Higher order component

 To share tracking logic across various UX components. if more one UI will have same function that need to share and UI is differ use this pattern
 
```js
import tracker from './tracker.js';

// HOC
const pageLoadTracking = (ComposedComponent) => class HOC extends Component {
  componentDidMount() {
    tracker.trackPageLoad(this.props.trackingData);
  }

  componentDidUpdate() {
    tracker.trackPageLoad(this.props.trackingData);
  }

  render() {
    return <ComposedComponent {...this.props} />
  }
};

// Usage
import LoginComponent from "./login";

const LoginWithTracking = pageLoadTracking(LoginComponent);

class SampleComponent extends Component {
  render() {
    const trackingData = {/** Nested Object **/};
    return <LoginWithTracking trackingData={trackingData}/>
  }
}
```

Convention need to follow
- A HOC should not change the API of a provided component
- HOCs should have a name following the `withNoun` pattern
- HOCs should not have any parameters aside from the Component itself. HOCs can handle additional parameters via currying. `withNoun("some data")(MyComponent)`
- 
### Compound Components

where a group of components works together to achieve a specific functionality.
have 

```js

// Tabs.js

const TabContext = createContext();

function Tabs({ children, defaultTab }) {
  const [activeTab, setActiveTab] = useState(defaultTab);

  const changeTab = (tab) => {
    setActiveTab(tab);
  };

  return (
    <TabContext.Provider value={{ activeTab, changeTab }}>
      <div>
        {React.Children.map(children, (child) => {
          if (React.isValidElement(child)) {
            return React.cloneElement(child, { activeTab });
          }
          return child;
        })}
      </div>
    </TabContext.Provider>
  );
}

function Tab({ label, children }) {
  const { activeTab, changeTab } = useContext(TabContext);

  return (
    <div>
      <button onClick={() => changeTab(label)}>{label}</button>
      {activeTab === label && <div>{children}</div>}
    </div>
  );
}

export { Tabs, Tab };

// App.js

import React from 'react';
import { Tabs, Tab } from './Tabs';

function App() {
  return (
    <Tabs defaultTab="Tab 1">
      <Tab label="Tab 1">
        <p>Content for Tab 1</p>
      </Tab>
      <Tab label="Tab 2">
        <p>Content for Tab 2</p>
      </Tab>
      <Tab label="Tab 3">
        <p>Content for Tab 3</p>
      </Tab>
    </Tabs>
  );
}

```


## Rendering and Performance

#### Re-render reason
- State change
- Parent re-renders
- Context Provider changes (all components that use this Context will re-render)
- Hooks change (cause the re-render of the host component that using hooks)

#### Preventing Re-render
- Composition: moving state down
- Children as props
- React.Memo
- If Context Provider is placed not at the very root of the app, and there is a possibility it can re-render itself because of changes in its ancestors, its value should be memoized.
- Use **State** only the values that have  effect on the vDOM. else use **useRef**
- Lift the state down as much possible to component that actually need

#### The mystery of React Element, children, parents and re-renders

- if you pass children as a render function. (parent change will cause child re-render)

- Wrapping parent with **React.memo** won't protect the child of the parent 

```js
// wrapping MovingComponent in memo to prevent it from re-rendering

const MovingComponentMemo = React.memo(MovingComponent);

const SomeOutsideComponent = () => {

  // trigger re-renders here with state
  const [state, setState] = useState();

  return (
    <MovingComponentMemo>
<!-- ChildComponent and MovingComponentMemo  will still re-render when SomeOutsideComponent re-renders to avoid wrap child with memo because the chillderen are props so every re-render they change which cause memo to re-render
-->
      <ChildComponent />
    </MovingComponentMemo>
  )

}
```


## Memory leak

When using memoization techniques like `useCallback` to avoid unnecessary re-renders, there are some things to watch out for. `useCallback` will hold a reference to a function as long as the dependencies don't change. Here's an example:

```js
import { useState, useCallback } from "react";

class BigObject {
  public readonly data = new Uint8Array(1024 * 1024 * 10);
}

export const App = () => {
  const [countA, setCountA] = useState(0);
  const [countB, setCountB] = useState(0);
  const bigData = new BigObject(); // 10MB of data

  const handleClickA = useCallback(() => {
    setCountA(countA + 1);
  }, [countA]);

  const handleClickB = useCallback(() => {
    setCountB(countB + 1);
  }, [countB]);

  // This only exists to demonstrate the problem
  const handleClickBoth = () => {
    handleClickA();
    handleClickB();
    console.log(bigData.data.length);
  };

  return (
    <div>
      <button onClick={handleClickA}>Increment A</button>
      <button onClick={handleClickB}>Increment B</button>
      <button onClick={handleClickBoth}>Increment Both</button>
      <p>
        A: {countA}, B: {countB}
      </p>
    </div>
  );
};
```

- The first click on “Increment A” will cause `handleClickA()` to be recreated since we change `countA` - let’s call the new one `handleClickA()#1`.
- `handleClickB()#0` will _not_ get recreated since `countB` didn’t change.
- This means, however, that `handleClickB()#0` will still hold a reference to the previous `AppScope#0`.
- The new `handleClickA()#1` will hold a reference to `AppScope#1`, which holds a reference to `handleClickB()#0`.

The general problem is that different `useCallback` hooks in a single component might reference each other and other expensive data through the closure scopes. The closures are then held in memory until the `useCallback` hooks are recreated. Having more than one `useCallback` hook in a component makes it super hard to reason about what’s being held in memory and when it’s being released. The more callbacks you have, the more likely it is that you’ll encounter this issue.

## Resources
1. [React re-renders guide: everything, all at once]([https://www.developerway.com/posts/react-re-renders-guide](https://www.developerway.com/posts/react-re-renders-guide)
2. [React Element, children, parents and re-renders](https://www.developerway.com/posts/react-elements-children-parents)
3. [When does React re-render components?](https://felixgerschau.com/react-rerender-components/)
4. [https://reacthandbook.dev/react-performance-optimization](https://reacthandbook.dev/react-performance-optimization) 
5. [Blogged Answers: A (Mostly) Complete Guide to React Rendering Behavior](https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/)
6. [Performance using React Profiler](https://medium.com/inato/prevent-re-renders-in-your-react-app-using-react-profiler-93c492110e30 )

#### Patterns
1. [Component composition](https://epicreact.dev/one-react-mistake-thats-slowing-you-down/)
2. [Component composition](https://frontendmastery.com/posts/advanced-react-component-composition-guide/)
3. [React components composition](https://www.developerway.com/posts/components-composition-how-to-get-it-right)
4. [https://felixgerschau.com/react-component-composition/#how-can-composition-help-performance](https://felixgerschau.com/react-component-composition/#how-can-composition-help-performance)
5.  [Effective Higher-Order Components](https://www.bbss.dev/posts/effective-hocs/)
6. https://kentcdodds.com/blog/optimize-react-re-renders

## State Mangement

[Simplifying React state management](https://causal.app/blog/re-re-reselect)
- They use [re select](https://www.npmjs.com/package/re-reselect) npm pkg for manipulating data from different store


Advanced
1. https://medium.com/the-guild/under-the-hood-of-reacts-hooks-system-eb59638c9dba How hooks works under the hood
2. https://stackoverflow.com/questions/53974865/how-do-react-hooks-determine-the-component-that-they-are-for

Pkg
1. [State mangement like context but re-render only when actual val change](https://github.com/dai-shi/react-tracked)
2. [A collection of modern, server-safe React hooks](https://usehooks.com/)
3. 











Need to arrange

React clone element for component compositon


Do you need to calculate the state from a state or props you already have Do that in the component and not in useEffect

Do you update state in useEffect when a prop changes? Wrong !

Using useEffect to update state is wrong because the props are not a side effect.
Instead you can derive the needed data from that prop and use it as it is in your JSX.

Never put any business logic into UI components. Extract all seState, useEffect to custom hook.

Use children props to stop re-rendering the children components

Understand identities when working with lists



Note: `do not use Context for passing user actions between components Use composition or pass props`

Don’t import SVGs as JSX or directly in React




Need to look
1. https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/
2. https://github.com/coryhouse/reactjsconsulting/issues/77 
3. https://molefrog.com/notes/react-tricks
4. https://overreacted.io/a-chain-reaction/

Need to cover
1. Forward ref
2. useSyncExternalStore






Memory leak
- Avoid using usecallback if you have a big object or varible it will be hold on reference by the callback
- https://schiener.io/2024-03-03/react-closures


Internal of react 6hour
- https://www.youtube.com/watch?v=-XKvVyC6si0