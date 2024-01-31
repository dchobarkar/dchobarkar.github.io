# React - 14: Creating Custom Hooks in React

## Introduction

Custom hooks in React are a powerful feature for extracting and reusing logic across components. This article aims to provide a comprehensive understanding of creating and using custom hooks in React applications.

## What are Custom Hooks?

Custom hooks are JavaScript functions that leverage React's built-in hooks to encapsulate reusable logic.

### Why Custom Hooks?

- **Reusability**: Share logic across multiple components without duplicating code.

- **Separation of Concerns**: Keep the logic separate from the UI, leading to cleaner and more maintainable components.

## Building Your First Custom Hook

Let’s start with a simple custom hook for fetching data.

### useFetch Hook

```jsx
import { useState, useEffect } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((error) => {
        setError(error);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}
```

This hook can be used in any component to fetch data from a given URL.

## Advanced Custom Hooks

Custom hooks can be as simple or as complex as needed. They can manage state, handle form inputs, or even integrate with external libraries.

### useLocalStorage Hook

Creating a hook to interact with the browser's local storage:

```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  const setValue = (value) => {
    try {
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.log(error);
    }
  };

  return [storedValue, setValue];
}
```

## Using Custom Hooks in Components

Custom hooks are used in components just like built-in hooks.

### Example with useFetch

```jsx
function UserData() {
  const { data, loading, error } = useFetch("https://api.example.com/user");

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return <div>Username: {data.username}</div>;
}
```

## Best Practices for Creating Custom Hooks

- **Naming Convention**: Custom hooks should start with use (e.g., `useFormInput`).

- **Keep Hooks Focused**: Each custom hook should serve a single purpose.

- **Testing Custom Hooks**: Ensure hooks are tested separately for reliability.

![React Concepts](images/react_blog_14.png "Creating Custom Hooks")

## Conclusion

Custom hooks are a testament to React's flexibility and power. By understanding how to create and use custom hooks, developers can write more reusable, clean, and concise code. They enable sharing logic across components, enhancing the overall development experience.

Stay tuned for our next article, where we'll discuss the useEffect hook and its role in handling side effects in React applications.

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
