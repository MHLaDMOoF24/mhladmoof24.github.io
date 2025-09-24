---
layout: post
title: "Learning the language found - Props, Side effects (useEffect)"
tags: [research, application]
---

## Assertion of knowledge
In the previous post, ["Learning the language found - State management (useState)"](./2025-09-18-state_management.md), we detailed State Management. From that, there were two main paths we could, and should, explore: **Props and Side effects**.  
Since we're able to handle creating a component, then importing it and managing its state, the next step is to pass variables into it.  
At the same time, there's an offshoot of state management that deals with things happening outside of React's normal rendering flow, which is what side effects are for.  

And spoiler warning, this is the last post of three out-of-project experimentation posts. More on that later.  

---

## Prop-erly handling side effects
### Props
###### Properties
Relatively simple concept, considering what else we've covered, but the handling of it is very core to what differentiates TypeScript from JavaScript.  
Props are data passed into components from the parent of said component. This is part of why components are so reusable, as you can change looks or behaviours by passing different props into two instances of the same component.  

Let's have a quick showcase on how this works.  
First, a component with props:  
  ```typescript
  type GreetingProps = {
    name: string;
    greeting: boolean;
  };

  export function Greeting({ name, greeting }: GreetingProps) {
    return <h1>{greeting ? `Hello` : `Goodbye`}, {name}!</h1>;
  }
  ```
The exported function takes in a `name` prop, and uses it to display one of two greetings, depending on a given `greeting` prop.  
It does this by taking in the props as an object, and deconstructing it through the GreetingProps type.  
Once that is done, the function can use the props as normal variables, and return the information to the parent.  
  ```typescript
  import { Greeting } from "./Greeting";

  export function App() {
    return (
     <div>
        <Greeting name="Alice" greeting={true} />
        <Greeting name="Bob" greeting={false} />
      </div>
    );
  }
  ```
The example above will render the following text:  
  ```
  Hello, Alice!
  Goodbye, Bob!
  ```

Quick recap:  
1. Props are values passed from parent → child.  
2. They make components configurable and reusable.  
3. In TypeScript, you define a props type to keep them safe and predictable.  

#### TypeScript is special
In JavaScript, the following would've been valid for the component above:  
  ```javascript
  export function Greeting({ name, greeting }) {
    return <h1>{greeting ? `Hello` : `Goodbye`}, {name}!</h1>;
  }
  ```
This would work, but it lacks the type safety that TypeScript provides, in that anything could be parsed into those props, be it strings, bools, ints, you name it.  


### Side effects
###### useEffect
TL;DR Side effects are things that happens as a result of another change.  

In React, some things don’t fit into the normal flow of rendering a component. For example: fetching data from an API, setting up a timer, or updating something outside the component. These are called **side effects**.  
React gives us the `useEffect` hook to handle these. It takes two inputs:  
1. A function with the code you want to run (your side effect).  
2. An optional “dependency array” that tells React *when* to run it again\*.  

\*If you don’t give it the array, the effect runs after **every render**. If you give it an empty array (`[]`), it only runs once when the component first loads.  

#### Example  
Imagine we want to set the document title to show how many times a button has been clicked.  
We’ll use both `useState` (to count clicks) and `useEffect` (to update the title).  

First, import the hooks:  
  ```typescript
  import { useState, useEffect } from "react";
  ```
Then, set up the state for counting clicks:  
  ```typescript
  const [count, setCount] = useState(0);
  ```
With this in place, lets set up the useEffect to update the document title:  
  ```typescript
  useEffect(() => {
    document.title = `Clicked ${count} times`;
  }, [count]);
  ```
And finally we need the button to increment the count:  
  ```typescript
  <button onClick={() => setCount(count + 1)}>
    Clicked {count} times!
  </button>
  ```
Putting this together in a component, we end up with:  
  ```typescript
  import { useState, useEffect } from "react";

  export function ClickTitleButton() {
    const [count, setCount] = useState(0);

    useEffect(() => {
      document.title = `Clicked ${count} times`;
    }, [count]);

    return (
      <button onClick={() => setCount(count + 1)}>
        Clicked {count} times!
      </button>
    );
  }
  ```

Threecap: 
1. Import `useEffect` from React.
2. Write a function inside a useEffect object for the side effect you want.
3. Use the dependency array to control when it runs (empty = once, value = runs on change, none = every render).

#### Why useEffect?
In the example above, we have a React component that counts clicks. This means that useState will automatically proliferate into other parts that React handles.  
The issue is that the document title is **outside of React’s "ownership"**, so useEffect is needed to have the state change spread outside of React's normal rendering.  


---

## Resources
- [12 useState & useEffect Mistakes Junior React Developers Make](https://www.youtube.com/watch?v=-yIsQPp31L0)

---

## Okay, so what now?
Now, we make the project. I'm pretty sure the basics are covered, at least enough that it can be begun. Everything that I need to learn can be either done within the project, or put out into the experimentation project for testing.  
As it stands, the experiments are done. Time to get cooking.