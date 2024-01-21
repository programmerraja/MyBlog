 +++
title = 'React'
date = 2024-01-20T18:57:02.022+05:30
draft = true
+++  


## Notes

#### Context

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
- Don't have single context for width and mobile beacuse when ever the width change the component that only using mobile (`child2`) will also get re-render. this will applicable only if `Child2` is Mounted to DOM
- When should I use Context?
	- Any time you have some value that you want to make accessible to a portion of your React component tree, without passing that value down as props through each level of components.

## Tips And Tricks

- Don't return Null on a component which cause the component to unmount if you want mount again if it based on conditon use boolean it wil be not dispalayed in UI but will be avalible in V-DOM

- **Profiling Tips:** When you do profiling using react profiler try make CPU 2x or some lower bound slow down to find the issue beacuse most of the modern system will do better and one more tips when looking the flame chart check for the single task duration that need to be less then **~16 milliseconds** (`Human eyes are sensitive to motion, and frame rates below 60 fps may result in perceptible stuttering or flickering in animations`) if it taking more then 16 Milliseconds it will be laggy in UI 

-  useState for one-time initializations ` const [resource] = React.useState(() => new Resource())`

- React chilldern will add key `React.Children.toArray(someData.map(item=><div>{item.title}</div>)` 

- Toggle CSS instead of forcing a component to mount and unmount

- Use virtualization for large lists
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

Need to cover
1. Forward ref
2. useSyncExternalStore

Advanced
1. https://medium.com/the-guild/under-the-hood-of-reacts-hooks-system-eb59638c9dba How hooks works under the hood
2. https://stackoverflow.com/questions/53974865/how-do-react-hooks-determine-the-component-that-they-are-for

Pkg
1. [State mangement like context but re-render only when actual val change](https://github.com/dai-shi/react-tracked)











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

Whenever you have JSX repeating itself, extract the logic to a config object and loop through it.

Use composition instead of Context



Render Props
