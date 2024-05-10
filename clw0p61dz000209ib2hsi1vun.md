---
title: "How to create exit animations with Framer Motion"
seoTitle: "Exit Animations with Framer Motion"
seoDescription: "Learn to create dynamic exit animations using Framer Motion in React."
datePublished: Fri May 10 2024 13:12:32 GMT+0000 (Coordinated Universal Time)
cuid: clw0p61dz000209ib2hsi1vun
slug: how-to-create-exit-animations-with-framer-motion
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715345723909/c71d9691-fe4c-4302-b1cb-d9dca77a99b5.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1715346157282/e8c2f155-319e-4a6b-a651-e75a658132be.png
tags: css, javascript, web-development, animation, reactjs, typescript, frontend-development, tailwind-css, vite, framer-motion

---

Framer Motion is a powerful library built on top of React that simplifies the creation of animations within your user interface. While core React functionality allows for animations and transitions using CSS, Framer Motion offers a more feature-rich toolset designed to implement dynamic and performant animations.

In this article, you'll learn how to implement exit animations using the `exit` prop and the `AnimatePresence` component. We'll progressively build upon this foundation, demonstrating how to create more advanced exit animations with custom transitions and delayed effects. At the end of this, you'll build a simple pop-up modal using all you learn. I hope you have a lovely read.

## Get Started

**Prerequisites:**

* Node.js and npm (or yarn) installed on your system.
    
* An existing React project.
    
* Basic knowledge of Framer Motion
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">For this guide, we'll use a React-TypeScript project and Tailwind CSS for styling. While these are my preferred choices, you can tailor the setup to your existing project or preferences.</div>
</div>

**1\. Install Framer Motion:**

In your project directory, install Framer Motion using either npm or yarn:

**Using npm:**

```plaintext
npm install framer-motion
```

**Using yarn:**

```plaintext
yarn add framer-motion
```

**2\. Create a basic component:**

Let's create a simple component to play around with. Inside the `src` folder, create a new file named `MyComponent.js`. Here's a basic structure for our component:

```typescript
import React from 'react';

const MyComponent = () => {
  return (
    <div className="w-20 h-20 bg-green-600 rounded-full cursor-pointer" />  
  );
};

export default MyComponent;
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715292325360/f551c6e6-3d6b-4bc5-8cdf-68197ff1d08f.png align="center")

**3\. Import Framer Motion:**

To make your component move with Framer Motion, you need to import the `motion` component. Update your `MyComponent.js` file to include this line at the top:

```typescript
import { motion } from 'framer-motion';
```

The `motion` import from `framer-motion` lets you convert an ordinary HTML tag like `div` to a `motion.div` tag that allows you to animate your React elements.

There you have it! You now have a basic React project set up with Framer Motion and are ready to create awesome exit animations!

## Can CSS Handle Exit Animations?

Sure, CSS transitions seem like a natural fit for exit animations in React at first glance. You can define styles for an element and smoothly transition them to disappear, creating a fade-out or slide-out effect.

However, here's the catch: When you try to hide a React component during an exit animation using CSS, you run into a roadblock. React, by design, removes components from the Document Object Model (DOM) entirely when they are hidden. This removal from the DOM makes them unavailable for animation by CSS. CSS transitions rely on manipulating elements within the DOM, so a removed element can't be smoothly animated.

This presents a challenge for basic exit animations with CSS in React. While CSS might seem viable, it can't handle the core functionality of animating a disappearing component due to React's behavior. This is where Framer Motion steps in, offering a solution specifically designed for handling animations alongside React's component lifecycle.

## Using AnimatePresence

Remember, React entirely removes hidden components from the Document Object Model (DOM). This disrupts the animation process if you directly apply exit animations. `AnimatePresence` solves this challenge by acting as a wrapper around your components.

**How to use**`AnimatePresence`**:**

1. **Define the Exit Animation:**
    
    * We start by defining the exit animation itself using the `exit` prop within Framer Motion variants. This prop specifies the animation that plays when the component exits the scene.
        

Here's an example:

```typescript
const exitAnimation = {
  opacity: 0, // Fade out to zero opacity
};
```

2. **Wrap with AnimatePresence:**
    
    Next, we wrap our components with `AnimatePresence`. This ensures that even when a component is hidden, it remains in the DOM momentarily, allowing Framer Motion to complete the exit animation before it's truly removed.
    

Here's how it looks in practice:

```typescript
import { AnimatePresence } from 'framer-motion';

export function App() {
  const [visible, setVisible] = useState(true);

  const exitAnimation = {
    opacity: 0, // Fade out to zero opacity
  };

  return (
    <AnimatePresence>
      {visible && (
        <motion.div
          exit={exitAnimation}
          className="w-20 h-20 bg-green-600 rounded-full cursor-pointer"
          onClick={() => setVisible(false)}
        />
      )}
    </AnimatePresence>
  );
}
```

#### With `AnimatePresence`:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715293939791/1c72de9a-a8c5-4d3e-8c73-1dcacc5b0a99.gif align="center")

#### Without `AnimatePresence`:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715202267718/0a85075f-95d2-4567-a70c-9f7a5b47739b.gif align="center")

In this example:

* We wrap the `motion.div` with `AnimatePresence`.
    
* The `motion.div` has the defined `exitAnimation` applied using the `exit` prop, which fades out the element.
    
* When the component is clicked (toggling `visible` to `false`), AnimatePresence ensures the exit animation plays before removing the element from the DOM.
    

This is just a basic example, of course. Framer Motion allows you to animate various properties like opacity, scale, translate (position), and more, giving you the freedom to create a wide variety of exit animations.

## Advanced Exit Animations

We've explored the basics of defining exit animations with Framer Motion. Now, let's learn some advanced techniques to create better exit animations:

**1\. Fine-tune with Custom Transitions:**

The `transition` property within the `exitAnimation` object offers even more control over your animation. You can define custom easing functions to control the speed and flow of the animation:

```typescript
const exitAnimation = {
  opacity: 0,
  transition: { duration: 0.5, ease: "easeInOut" }, // Use "easeInOut" for smooth start and end
};
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715294238910/15c363df-fdf1-4a82-b229-7005fef2cd00.gif align="center")

Here, we've added the `ease: "easeInOut"` property to the `transition`. This tells the animation to ease in and out smoothly, creating a more natural feel.

**2\. Combining Multiple Animation Properties:**

Don't limit yourself to a single animation property! Framer Motion allows you to combine multiple properties within your `exitAnimation` object:

```typescript
const exitAnimation = {
  opacity: 0,
  x: "-100vw", // Slide off-screen to the left
  rotate: 90, // Rotate 90 degrees
  transition: { duration: 0.7 },
};
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715263506883/2e56cafe-4914-4acf-9b3a-2c9988635ea0.gif align="center")

In this example, we're fading out the component (`opacity: 0`), sliding it off-screen to the left (`x: "-100vw"`), and adding a 90-degree clockwise rotation (`rotate: 90`) for a more dramatic exit.

**3\. Delayed Exits for Multiple Elements:**

An exit animation can become even more engaging when applied to multiple elements. Framer Motion offers two techniques to create a cascading or wave-like effect: individual delays and stagger.

```typescript
export function MyComponent() {
  const [visible, setVisible] = useState(true);

  const exitAnimationParent = {
    opacity: 0,
    x: "-25vw",
    rotate: 90,
    transition: {
      duration: 0.5,
      ease: "easeInOut",
      delay: 0.5,
    },
  };

  const exitAnimationChild = {
    opacity: 0,
    x: "25vw",
    rotate: 90,
  };

  return (
    <AnimatePresence>
      {visible && (
        <motion.div
          exit={exitAnimationParent}
          className="w-20 h-20 bg-green-600 cursor-pointer rounded-2xl flex justify-evenly items-center"
          onClick={() => setVisible(false)}
        >
          <motion.div
            exit={exitAnimationChild}
            transition={{ duration: 0.5, ease: "easeInOut", delay: 0.2 }}
            className="w-7 h-7 bg-white  rounded cursor-pointer"
          />
          <motion.div
            exit={exitAnimationChild}
            transition={{ duration: 0.5, ease: "easeInOut" }}
            className="w-7 h-7 bg-white rounded cursor-pointer"
          />
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715275016571/66873ed3-a148-4c79-abce-d63978e263a8.gif align="center")

The provided component demonstrates delayed exits for two child elements within a parent container. Here's a breakdown of what's going on:

1. **Parent Container Animation:**
    
    * The parent container (`motion.div`) uses the `exitAnimationParent` variant. The animation properties and configuration remain the same:
        
        * Fades the container out with `opacity: 0`.
            
        * Slides the container off-screen by 25 viewports to the left with `x: "-25vw"`.
            
        * Rotate the container 90 degrees clockwise with `rotate: 90`.
            
        * The animation has a duration of 0.5 seconds, uses an "easeInOut" easing function, and has a delay of 0.5 seconds before starting.
            
2. **Child Element Animations:**
    
    * Each child element (`motion.div`) still uses the `exitAnimationChild` variant defining opacity and x-axis translation.
        
    * However, the key difference is the individual `transition` prop for each child.
        
        * The first child element now has a slight delay of 0.2 seconds within its transition.
            
        * The second child element retains the original transition properties (0.5 seconds duration and "easeInOut" easing).
            

These are just a few ways to create advanced exit animations with Framer Motion. While individual delays create a cascading effect, Framer Motion offers a more scalable approach for complex scenarios: `staggerChildren` and `delayChildren`. We won't cover those in this tutorial.

## Building a Modal Component with Exit Animation

Modal components are a common UI element for displaying additional content or functionality within an overlay. With Framer Motion, we can add a touch of polish with a smooth exit animation when the modal is dismissed.

A modal component typically consists of two parts:

* **Backdrop:** A dimmed background element that creates a visual distinction between the modal and the rest of the application content.
    
* **Modal Content:** The main content area displayed within the modal, often containing information, forms, or interactive elements.
    

**Creating the Modal Component:**

Here's a basic structure for our modal component with Framer Motion:

```typescript
import { motion } from "framer-motion";

interface ModalProps {
  setShowModal: (showModal: boolean) => void;
}

const Modal = ({ setShowModal }: ModalProps) => {
  return (
    <motion.div
      className="overflow-y-auto overflow-x-hidden fixed top-0 bottom-0 mx-auto my-auto right-0 left-0 z-50 justify-center items-center w-full bg-black/50 md:inset-0 flex"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      exit={{ opacity: 0 }}
    >
      <motion.div
        className="h-[270px] w-full relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl sm:my-8 sm:w-full sm:max-w-lg flex flex-col items-center justify-center px-4 pb-4 pt-5 sm:p-6 sm:pb-4 gap-5"
        initial={{ scale: 0.5 }}
        animate={{ scale: 1 }}
        exit={{ scale: 0 }}
      >
        <div className="bg-green-100 rounded-full p-3">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            aria-hidden="true"
            className="text-green-700 w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M4.5 12.75l6 6 9-13.5"
            ></path>
          </svg>
        </div>
        <h4 className="font-semibold text-lg">Payment successful</h4>
        <p className="text-center w-[300px] -mt-2 leading-snug text-gray-600">
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Sit esse
          dolorem officia.
        </p>
        <button
          type="button"
          className="inline-flex w-full justify-center rounded-md bg-[#4f46e5] px-3 py-2 text-white shadow-sm hover:bg-[#3f38b5] transition-colors text-md font-bold "
          onClick={() => setShowModal(false)}
        >
          Close modal
        </button>
      </motion.div>
    </motion.div>
  );
};

export default Modal;
```

**Explanation:**

* **Initial State:** The modal content starts with `opacity: 0` (invisible) and a `scale: 0.5` (scaled down to 50%).
    
* **Animate State:** When the modal opens, the content animates to `opacity: 1` (fully visible) and `scale: 1` (normal size).
    
* **Exit State:** When the modal closes, the content animates back to `opacity: 0` (invisible) and `scale: 0` (scaled down), creating a shrinking and fading-out effect.
    

**Adding the Modal to Your Application:**

Now you can integrate this `Modal` component into your application. Here's an example:

```typescript
import { useState } from "react";
import { AnimatePresence } from "framer-motion";
import Modal from "./modal";

export default function App() {
  const [showModal, setShowModal] = useState(false);

  return (
    <div className="w-full h-[100vh] flex items-center justify-center relative">
      <button
        className="inline-flex w-full justify-center rounded-md bg-[#4f46e5] px-3 py-2 text-md font-bold text-white shadow-sm hover:bg-[#3f38b5] sm:w-auto"
        onClick={() => setOpen(!open)}
      >
        Open modal
      </button>

      <AnimatePresence>
        {open && <Modal setOpen={setOpen} />}
      </AnimatePresence>
    </div>
  );
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715268217741/8a0aa218-8b23-4545-a5a1-5376aaef4192.gif align="center")

In this example, clicking the "Open modal" button sets `showModal` to true, triggering the modal to open with its scale-up animation. Clicking the "Close modal" button sets `showModal` to false and initiates the modal's exit animation.

This is a basic example, of course. You can customize the variants and animation properties to create different exit effects.

## Conclusion

If you made it all the way here, congrats!

Hopefully, you've learned how easy and intuitive implementing exit animations using Framer motion is. Let's recap what we covered:

* We learned why we couldn't use plain CSS transitions to create exit animations in React.
    
* We learned how `AnimatePresence` ensures the exit animation plays before the element is removed from the DOM.
    
* We explored how to implement more advanced exit animations using custom easing functions, multiple properties, and transition delays.
    
* Finally, we built a simple modal component using all we learned in this guide.
    

Framer Motion is a powerful library; this guide has just scratched the surface! You can explore state changes and gestures and even build complex animation sequences. Check out the documentation [here](https://www.framer.com/motion/).

Remember, though, that animations should always be intentional. They can be a fantastic tool to guide users, communicate app state, and enhance the overall experience. But avoid going overboard – animations shouldn't slow down your app or become distracting. Aim for a balance that doesn't impede on user experience.

I'd love to see what you build with your new knowledge. Have fun, and thanks for reading!

---

I would greatly appreciate any constructive feedback you may have. Feel free to share your thoughts! I'm happy to write if you found this helpful and are interested in more Framer Motion-related content. In the coming weeks, you can also expect more blog articles exploring exciting web technologies I come across. I hope you had a lovely read. 🤍

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">I'm also open to new opportunities! If you're looking for a frontend developer experienced with React, Next.js, and Tailwind CSS, check out my portfolio for more of my work: <a target="_blank" rel="noopener noreferrer nofollow" href="https://www.victorwilliams.me/" style="pointer-events: none">https://www.victorwilliams.me</a>. I'm available for full-time, part-time, or freelance gigs. Feel free to reach out if you have any projects in mind!</div>
</div>