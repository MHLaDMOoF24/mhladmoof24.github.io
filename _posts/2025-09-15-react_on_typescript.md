---
layout: post
title: "Learning the language found - Components, Imports"
tags: [research, application]
---

## How we doin'
### Setup
Previously we figured out a setup for the TypeScript environment, and decided to focus on frontend due to an API potentially handling backend.  
This leaves us with a TypeScript + React frontend, with the possibility of adding a Node.js backend later, should the need arise.  

### Knowledge going in
While technically speaking some experience with JavaScript exists from working with Blazor, it's not something I actually looked at due to being able to complete the project solely using HTML and C#.  

---

## What we doin'
### The experiment
So the whole JavaScript environment is new to me. This leaves not only a lot of unknowns, but also a complete and utter lack of direction.  
And thus, there's only one  thing to do: Experiment, for science!  
The **goal** is to get a feel for the language. The experiment should be able to cover the following:  
- Basic syntax and constructs (variables, functions, loops, conditionals)
- TypeScript features (types, interfaces, generics)
- React basics (components, props, state)
- Project structure and tooling (npm, build tools, linters)

### Scope, and its limitations
This is sort of a longer list than I'd like for a first experiment, but I don't mind the mini-project getting bloated and unorganized. This is _just_ an experiment, not something I want to work off of.  
Doing it this way also means I don't have to make new experiments to cover each point, and can easily start playing around with mixing and matching to create more complex solutions.  
Timewise, I'm not fuzzed about how quickly the experiment is to conclude, as there's a lot of group-wide time limitations (vacations, operations, etc.), which leads to disjointed work periods. An experiment to go to and from during that time seems ideal, as long as the main project is being kept in mind, so that any learnings can easily be transfered into project progress once everyone is back on track.  

### This blog
Look, I'm not going to document every little thing I do. This experiment will be _messy_, and I do not plan on structuring it for the purpose of readability. I will detail some of the things I found particularly important to take note of, and maybe include an image, video, or article that I found particularly helpful. That's it, I'm not detailing "hi i made variable :)".  
This also means that there'll not be a whole lot of posts, or that the substance will change to slightly less formated ramblings. Just a heads up.  

---

## Notable discoveries
### Components
A component is a reusable piece of UI that can be strapped together to create an interface.  
Here's an example of a simple component:  
  ```typescript
  // ../components/Hello.tsx
  type HelloProps = { name: string; verified?: boolean };

  export function Hello({ name, verified = false }: HelloProps) {
    return <h1>{verified ? `🎉 Hello, ${name}!` : `Hello, ${name}`}</h1>;
  }
  ```
And here's how you might use it:  
  ```typescript
  // ../App.tsx
  import { Hello } from "./components/Hello";
  ...
  function App() {
    return (
      <div>
        <Hello name="Anders" verified={true} />
      </div>
      ...
    );
  }
  ```


### Imports
In TypeScript (and JavaScript), you can import modules or specific parts of modules using the `import` statement. There are two main ways to do this: named imports and default imports.  
Imagine we have the following utility file that we want to use within parts of our program:  
  ```typescript
  // ../utils.ts
  export default function multiply(a: number, b: number): number {
    return a * b;
  }
  export function divide(a: number, b: number): number {
    return a / b;
  }
  export function add(a: number, b: number): number {
    return a + b;
  }
  export function subtract(a: number, b: number): number {
    return a - b;
  }
  ```
This file has a default for multiply, and three other named exports.  
- **Named Imports**: You can import specific functions, objects, or primitives that have been exported from a module. You need to use the exact name of the export. For example:
  ```typescript
  // ../main.ts
  import { add } from './utils';
  console.log(add(2, 3)); // Outputs: 5
  ```
- **Default Imports**: A module can export a single default export. You can import it using any name you choose. For example:
  ```typescript
  // main.ts
  import multiply from './math';
  console.log(multiply(2, 3)); // Outputs: 6
  ```

So far I've only used named imports, and I don't see that changing anytime soon. Named imports make it clear what is being used from a module, which helps with both readability and maintainability.  

---

## Upcoming plans
### Next steps
The experiment is still ongoing, and there's a lot of ground to cover. The immediate next steps are:  
- Dive deeper into React: Learn about hooks, lifecycle methods, and state management.
- Explore TypeScript features: Understand advanced types, decorators, and modules.
- Generally get better with the things I've already touched on.

### Part 2?
The next topic we'll dive into is the functionality of the useState hook, which is a fundamental part of managing state in React components.