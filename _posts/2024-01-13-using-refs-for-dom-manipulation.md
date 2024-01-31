# React - 13: Using Refs for DOM Manipulation in React

## Introduction

Refs in React provide a way to access and interact with DOM nodes directly. This article delves into the use of refs for DOM manipulation, covering various scenarios where they are particularly useful.

## What are Refs in React?

Refs (short for references) allow direct access to DOM elements in functional and class components. They bypass the usual data flow and offer a way to interact with the DOM nodes.

### Creating Refs

- **useRef Hook**: In functional components, refs are created using the `useRef` hook.

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);

  const onButtonClick = () => {
    inputEl.current.focus();
  };

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

- **React.createRef**: In class components, refs are created using `React.createRef`.

```jsx
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  focusTextInput = () => {
    this.textInput.current.focus();
  };

  render() {
    return (
      <>
        <input type="text" ref={this.textInput} />
        <button onClick={this.focusTextInput}>Focus the input</button>
      </>
    );
  }
}
```

## Use Cases for Refs

Refs are not just for focusing elements. They can be used for various purposes:

- **Managing Focus, Text Selection, or Media Playback**: Controlling focus, text selection, or media playback.

- **Integrating with Third-Party DOM Libraries**: Useful when needing direct access to the DOM for using external libraries.

- **Animation**: For complex animations where direct DOM access is necessary.

## Refs with Function Components

Refs work slightly differently with function components due to the absence of instances.

- **useImperativeHandle Hook**: Customize the instance value that is exposed to parent components when using refs.

## Best Practices and Caveats

- **Avoid Overuse**: Refs should be used sparingly and only when necessary, as they can break the typical React data flow.

- **Refs and Functional Updates**: Remember that refs don’t trigger re-renders. Use state for data that changes over time.

- **Accessing Refs**: The `.current` property of the ref is used to access the DOM element.

![React Concepts](images/react_blog_13.png "Using Refs for DOM Manipulation")

## Conclusion

Understanding and correctly using refs can significantly enhance your React applications, especially in scenarios requiring direct DOM access. While powerful, it’s essential to use refs judiciously and in line with React's philosophy of declarative UI.

Stay tuned for our next article, where we will explore creating custom hooks in React.

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
