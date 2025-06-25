# Technical Writing Assignment

For guidance on setting up and submitting this assignment, refer to the Marcy lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/fullstack-curriculum/how-tos/working-with-assignments#how-to-work-on-assignments).

## Prompt 1

In your own words, explain why React is a popular choice for building user interfaces. Make sure to mention at least one benefit, such as how it simplifies development, supports reusable components, or helps optimize performance. Feel free to include any specific features you find particularly helpful.

### Response 1

React is a popular choice for building user interfaces primarily because it simplifies the development process and promotes better code management. One of its key benefits is the concept of reusable components. This means developers can break down complex user interfaces into smaller, self-contained pieces of code, known as components. These components can be reused across the application, reducing redundancy and making the code easier to maintain.

Additionally, React provides a virtual DOM, which helps optimize performance. The virtual DOM is an in-memory representation of the actual DOM. When changes occur in a React application, React updates the virtual DOM first and then compares it to the real DOM. This allows React to make the minimal set of updates necessary, ensuring faster rendering of UI changes.

React's declarative syntax is also a standout feature. Instead of worrying about how to manually manipulate the DOM, developers describe how the UI should look based on the current state, and React handles the rest.

## Prompt 2

Explain how the useState hook is used in React to manage state within functional components. In your response, include an example of how useState might be used in a simple application and why managing state is important in building interactive user interfaces.

### Response 2

The useState hook in React allows functional components to manage and track their state. Before the introduction of hooks, state management was only possible within class components, but now, with useState, you can easily add state to functional components.

Here’s a simple example of how useState might be used:
import React, { useState } from "react";

function Counter() {
// Declare a state variable named 'count' with an initial value of 0
const [count, setCount] = useState(0);

// A function to increment the count
const increment = () => setCount(count + 1);

return (

<div>
<h1>Count: {count}</h1>
<button onClick={increment}>Increase Count</button>
</div>
);
}

export default Counter;
In this example, useState(0) initializes the count state variable with a value of 0. The setCount function is used to update the state when the button is clicked. This allows React to re-render the component with the updated count.

Managing state is crucial in building interactive user interfaces because the state represents dynamic data that can change over time. Without proper state management, components would be static, and the UI would not respond to user actions or real-time data updates, reducing the interactivity of the application. By using useState, you can ensure that your components react to user input and other dynamic data, enhancing the user experience.

## Prompt 3

Describe the different ways the useEffect hook can be triggered in a React component. Include an explanation of how the dependency array influences its behavior. If possible, provide a code example for each scenario to illustrate your explanation.

### Response 3

The useEffect hook in React allows developers to perform side effects in functional components, such as data fetching, subscriptions, or manually updating the DOM. How and when useEffect runs is determined by the dependency array, which is the second argument passed to useEffect.

Here are the three main ways useEffect can be triggered:

1- useEffect(() => {
console.log("Effect runs after every render");
});
Explanation: If you omit the dependency array, the effect will run after every render, including the initial one. This can lead to performance issues if not managed carefully.

2- useEffect(() => {
console.log("Effect runs only on initial render");
}, []);
Explanation: An empty array tells React to run the effect only once—after the initial render. This is commonly used for data fetching or setting up event listeners when a component mounts.

3- useEffect(() => {
console.log(`Count is now ${count}`);
}, [count]);
Explanation: If you pass values inside the array (e.g., count), the effect will only re-run if those values change. This ensures efficient updates based on specific state or prop changes.

## Prompt 4

The component below makes a mistake when using useEffect. When running this code, we will get an error from React! Please fix this code.

```js
const DogDisplay = () => {
  const [imgSrc, setImgSrc] = useState(
    "https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg"
  );

  useEffect(async () => {
    try {
      const response = await fetch("https://dog.ceo/api/breeds/image/random");
      if (!response.ok) throw new Error(`Error: ${response.status}`);
      const data = await response.json();
      setImgSrc(data.message);
    } catch (error) {
      console.error(error);
    }
  }, []);

  return <img src={imgSrc} />;
};
```

After fixing the code provide and explanation to what you fixed and why it needed to be fixed.

### Response 4

import React, { useState, useEffect } from "react";

const DogDisplay = () => {
const [imgSrc, setImgSrc] = useState(
"https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg"
);

useEffect(() => {
const fetchDogImage = async () => {
try {
const response = await fetch("https://dog.ceo/api/breeds/image/random");
if (!response.ok) throw new Error(`Error: ${response.status}`);
const data = await response.json();
setImgSrc(data.message);
} catch (error) {
console.error(error);
}
};

    fetchDogImage();

}, []);

return <img src={imgSrc} alt="A random dog" />;
};

export default DogDisplay;

The original code incorrectly used an async function directly inside useEffect,

useEffect should not take an async function directly. This is because useEffect expects its callback to either return nothing or a cleanup function—not a promise. Using async causes React to see a returned promise, which leads to warnings or unexpected behavior.
