---
layout: post
title: "Learning the language found - State management (useState)"
tags: [research, application]
---

## \{introduction\}
### \{reasoning\}
This is a continuation of the previous post, ["Learning the language found - Components, Imports"](./2025-09-15-react_on_typescript.md), where we started experimenting with the functionality of TypeScript and React.  
This post is a direct continuation of that, and will be mostly consisting of notes and code snippets, explaining my understanding of the different functions I deem potentially necessary for the main project.  
For now, the functionalities themselves are the very basis of TypeScript and React, and therefore no notes of reasoning will be included, although reasoning will become more relevant as we dig into more niche functions.  

---

## New innovations, never before seen! (by me, at least)
### State management
###### useState
Within React, state management is a core concept that allows components to manage and respond to changes in data.  
State is used to hold variable object information.  
When the state changes, React re-renders the component to reflect the new state.  

#### Example
Imagine we're setting up a button that counts clicks. It needs to hold a value, update the value, and render the new value.  
  ```typescript
  // ../components/ClickButton.tsx
  ```
First and foremost, we need to import the `useState` hook from React:  
  ```typescript
  import { useState } from "react";
  ```
Once we have this, we can set up a constant:  
  ```typescript
  const [count, setCount] = useState(0);
  ```
This gives us a `count` variable, initialized to `0`, and a `setCount` function that we can use to update the `count` variable.  
**useState** requires two parts; the current value, and a "setter" function to update that value.  
When the setter function is called, React knows that the state has changed, and will re-render the component to reflect the new state.  
With this in place, we can now rig the state management up with a button:  
  ```typescript
  <button onClick={() => setCount(count + 1)}>
    Clicked {count} times!
  </button>
  ```
The button displays the current count, and each time it's clicked, it calls the `setCount` function to increment the count by 1.  
Putting these parts together, we can make a fully functioning click counter component:  
  ```typescript
  import { useState } from "react";
  export function ClickButton() {
    const [count, setCount] = useState(0);
    return (
      <button onClick={() => setCount(count + 1)}>
        Clicked {count} times!
      </button>
    );
  }
  ```

To recap:  
1. Import `useState` from React.  
2. Declare a constant with a value and a setter function using `useState`.  
3. Use the setter inside functions/events to update state and trigger re-renders.  

#### Alternatives?
Supposedly there are other state management libraries, but this seems to be the built-in way of doing it.  

#### A quick resource
A video I found that helped me understand state management better, even without having run into the issues highlighted in the video myself:  
- [You Are Using useState Wrong (and how to fix it)](https://www.youtube.com/watch?v=MO-w7Y4zRl0)

---

## Part 3?
Beyond useState, there are other React hooks that might be useful, such as useEffect, useContext, and useReducer.  
For the next part, I'll be looking into **useEffect** and a concept known as **props**.