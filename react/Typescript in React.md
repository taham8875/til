# Typescript in React

Empower your React applications with Typescript.

![typescript-with-react](assets/typescript-with-react.png)

## Migrate to Typescript

The most basic way to migrate to Typescript is to rename your `.js` or `.jsx` files to `.ts` or `.tsx`, this allows you to start using Typescript without any additional configuration.

## Variable Types

Typescript allows you to define the type of a variable, this allows you to catch errors before they happen.

```tsx
let myString: string = "Hello World";
```

But this is not always necessary, Typescript can infer the type of a variable.

```tsx
let myString = "Hello World";
```

Hover the variable with your mouse to see the inferred type. Typescript is smart enough to know that `myString` is a string.

Now try to assign a number to `myString`.

```tsx
let myString = 5; // Error: Type 'number' is not assignable to type 'string'
```

## Function Types

Typescript allows you to define the type of a function, let's start by an example of a simple function.

```tsx
function calculateInterest(
  amount: number,
  rate: number,
  years: number
): number {
  return amount * rate * years;
}
```

Now you can use this function and Typescript will check if you are using the correct types.

```tsx
calculateInterest(1000, 0.05, 10); // 500
calculateInterest("1000", 0.05, 10); // Error: Argument of type 'string' is not assignable to parameter of type 'number'
let myString = calculateInterest(1000, 0.05, 10); // Error: Type 'number' is not assignable to type 'string'
```

## React Components Props

Typescript allows you to define the type of a React component props, let's start by an example of a simple button. We want our custom button to accept a prop `backgroundColor`

`Button.tsx`

```tsx
import React from "react";

const Button = () => {
  return <button>Click me!</button>;
};

export default Button;
```

`App.tsx`

```tsx
import Button from "./components/Button";

const App = () => {
  return <Button backgroundColor="red" />;
};

export default App;
```

Typescript will give you an error because `backgroundColor` is not a valid prop for the `button` element.

So we tell the compiler that `backgroundColor` is a valid prop for the `button` element.

```tsx
import React from "react";

import React from "react";

const Button = (props: { backgroundColor: string }) => {
  const { backgroundColor } = props;
  return (
    <button
      style={{
        backgroundColor,
      }}
    >
      Click me!
    </button>
  );
};

export default Button;
```

We can destructure the props to make the code more readable. And add another prop `color` and optional `fontSize`

`Button.tsx`

```tsx
import React from "react";

const Button = ({
  backgroundColor,
  color,
  fontSize,
}: {
  backgroundColor: string;
  color: string;
  fontSize?: number;
}) => {
  return (
    <button
      style={{
        backgroundColor,
        color,
        fontSize,
      }}
    >
      Click me!
    </button>
  );
};

export default Button;
```

`App.tsx`

```tsx
import React from "react";
import Button from "./components/Button";

const App = () => {
  return <Button backgroundColor="red" color="blue" fontSize={30} />;
};

export default App;
```

Typescript helps catching errors early, typing `fontSize="30px"` instead of `fontSize={30}` is a expectable mistake from the developers.

It also provides an intellisense when you are using the component. click `ctrl + space` and vscode will show you the available props.

Another benefit of using Typescript is that it prevents you from doing illegal operations. For example, you can't use the `toUpperCase()` method on the `fontSize` prop.

```tsx
backgroundColor.toUpperCase(); // Valid
fontSize.toUpperCase(); // Error: Property 'toUpperCase' does not exist on type 'number'
```

> Note: Don't use `any` type, it defeats the purpose of using Typescript. If you used the type `any` for the `fontSize` prop, Typescript will not give you an error when you use the `toUpperCase()` method. but the application misbehaves at runtime.
> You can use instead the `unknown` type, but you have to check if the value is a string before using the `toUpperCase()` method. May be use validate it with zod or yup.

The current syntax is a bit verbose, we can use a `type` to make it more readable.

```tsx
import React from "react";

type ButtonProps = {
  backgroundColor: string;
  color: string;
  fontSize?: number;
};

const Button = ({ backgroundColor, color, fontSize }: ButtonProps) => {
  return (
    <button
      style={{
        backgroundColor,
        color,
        fontSize,
      }}
    >
      Click me!
    </button>
  );
};
```

### Union Types

Instead of accepting any string for the `color` prop, we can limit the available colors. It is called the `union` type.

```tsx
type ButtonProps = {
  backgroundColor: "red" | "blue" | "green";
  color: "red" | "blue" | "green";
  fontSize?: number;
};
```

Now we are duplicating the colors, we can use a `type` to define the available colors.

```tsx
type Color = "red" | "blue" | "green";

type ButtonProps = {
  backgroundColor: Color;
  color: Color;
  fontSize?: number;
};
```

### Array Types

May be we want to add `padding` to the button, we can use an array to define the padding.

```tsx
type Color = "red" | "blue" | "green";

type ButtonProps = {
  backgroundColor: Color;
  color: Color;
  fontSize?: number;
  padding?: number[];
};
```

### Tuple Types

Notice that we can add any number of values to the `padding` array, we can use a `tuple` to define the number of values.

```tsx
type Color = "red" | "blue" | "green";

type ButtonProps = {
  backgroundColor: Color;
  color: Color;
  fontSize?: number;
  padding?: [number, number, number, number];
};
```

So if you tried to add more than 4 values to the `padding` array, Typescript will give you an error.

Notice that adding more and more css properties is not practical, we instead pass these style ad an object.

```tsx
type Color = "red" | "blue" | "green";

type ButtonProps = {
  style: {
    backgroundColor: Color;
    color: Color;
    fontSize?: number;
    padding?: [number, number, number, number];
  };
};

const Button = ({ style }: ButtonProps) => {
  return <button style={style}>Click me!</button>;
};
```

Further more, we can use the `React.CSSProperties` type to define the style object.

```tsx
import React from "react";

type ButtonProps = {
  style: React.CSSProperties;
};

const Button = ({ style }: ButtonProps) => {
  return <button style={style}>Click me!</button>;
};

export default Button;
```

## Record Types

We can use the `Record` type to define an object with a dynamic number of keys.

```tsx
type ButtonProps = {
  borderRadius: Record<string, number>;
};

let customStyle: ButtonProps = {
  borderRadius: {
    topLeft: 5,
    topRight: 5,
    bottomLeft: 5,
    bottomRight: 5,
  },
};
```

## Function Types

We can use the `type` keyword to define a function type.

```tsx
type ButtonProps = {
  onClick: () => void;
};

// or

type ButtonProps = {
  onClick: (event: React.MouseEvent<HTMLButtonElement>) => void;
};
```

## Children Types

We can use the `React.ReactNode` type to define the children type.

```tsx
type ButtonProps = {
  children: React.ReactNode;
};
```

`App.ts`

```tsx
import Button from "./components/Button";

const App = () => {
  return <Button>Click me!</Button>;
};

export default App;
```

`Button.tsx`

```tsx
type ButtonProps = {
  children: React.ReactNode;
};

const Button = ({ children }: ButtonProps) => {
  return <button>{children}</button>;
};

export default Button;
```

Note that `React.ReactNode` is a little bit generic, i.e. it accepts string, number, boolean, or a jsx element, if for some reason you want only to accept jsx elements, you may modify the code as this:

```tsx
type ButtonProps = {
  children: JSX.Element;
};
```

Most of the cases, you will use `React.ReactNode` instead of `JSX.Element`

## React Components State

Typescript allows you to define the type of a React component state, let's start by converting the button to be a counter

`App.tsx`

```tsx
import Button from "./components/Button";
import { useState } from "react";

const App = () => {
  const [count, setCount] = useState(0);
  return <Button setCount={setCount}>Click me!</Button>;
};

export default App;
```

```tsx
import React from "react";

type ButtonProps = {
  children: React.ReactNode;
  setCount: React.Dispatch<React.SetStateAction<number>>;
};

const Button = ({ children }: ButtonProps) => {
  return <button>{children}</button>;
};

export default Button;
```

Note that there are no need to memorize the type of `setCount`, you can hover the mouse over it to see the type.

Also no need to provide the `useState` type most of the time, Typescript is smart enough to infer the type of `count` and `setCount`.

If you want to be more explicit, you can use the tell `useState` the type of the state.

```tsx
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<{ name: string; age: number } | null>(null); // null because we may fetch the user from an API

// or

type User = {
  name: string;
  age: number;
};

const [user, setUser] = useState<User | null>(null);
```

## Interface Types

We can use the `interface` keyword to define a type.

```tsx
interface ButtonProps {
  children: React.ReactNode;
  setCount: React.Dispatch<React.SetStateAction<number>>;
}
```

Note that we don't use the `=` sign as we do with the `type` keyword.

But `interface` has some disadvantages, for example:

- You can only use `interface` to define an object type.

```tsx
// Valid

type URL = string;

let myURL: URL = "https://www.google.com";

// Not valid

interface URL = string // Error

let myURL: URL = "https://www.google.com";

// Valid but not practical

interface URL {
  url: string;
}

let url: URL = {
  url: "https://www.google.com",
};
```

So it is recommended to use `type` instead of `interface` to define a type.

## Component Types

When we write a custom component the extends the functionality of a native HTML component, It is tedious to pass these props individually (like `type`, `autoFocus`) we can use the `React.ComponentProps` type to define the props.

```tsx
type ButtonProps = React.ComponentProps<"button">;
type ImgProps = React.ComponentProps<"img">;
// etc
```

If you want to extend the props the the Button component can accept, you can use the `&` operator.

```tsx
type ButtonProps = React.ComponentProps<"button"> & {
  variant: "primary" | "secondary" | "text";
};
```

If you want to do the same with the `interface` keyword, you can use `extends` keyword.

```tsx
interface ButtonProps extends React.ComponentProps<"button"> {
  variant: "primary" | "secondary" | "text";
}
```

## Event Handlers

I we want for example to write a `handleClick` function, we can use the `React.MouseEventHandler` type.

```tsx
import React from "react";

const Button = () => {
  return <button onClick={(event) => console.log("Clicked!")}>Click Me</button>;
};

export default Button;
```

Typescript will infer the type of the `event` object, but if you want to be more explicit, you can hover the mouse over the `event` object to see the type. (No need to memorize it)

```tsx
import React from "react";

const Button = () => {
  const handleClick = (
    event: React.MouseEvent<HTMLButtonElement, MouseEvent>
  ) => {
    console.log("Clicked!");
  };
  return <button onClick={handleClick}>Click Me</button>;
};

export default Button;
```

## ReadOnly Props

If you have a set of URLs, you may type them as this:

```tsx
const URLs = [
  "https://www.google.com",
  "https://www.facebook.com",
  "https://www.twitter.com",
];
```

There are no problem with that, but if you want to make sure that these URLs are not modified, or provide the developer with smarter intellisense, you can type them as this:

```tsx
const URLs = [
  "https://www.google.com",
  "https://www.facebook.com",
  "https://www.twitter.com",
] as const;
```

## Omit

If you want to omit a prop from a type, you can use the `Omit` type.

```tsx
type User = {
  name: string;
  age: number;
  email: string;
};

type Guest = Omit<User, "email">;
```

## Asserting Types

If you want to assert that a variable is of a certain type, you can use the `as` keyword.

```tsx
type ButtonColor = "red" | "blue" | "green";

export const Button = ({ color }: { color: string }) => {
  useEffect(() => {
    const color = localStorage.getItem("color") as ButtonColor;
  }, []);
  return <button style={{ backgroundColor: color }}>Click me!</button>;
};
```

Typescript would infer it as string, but you tell typescript that you are smarter than it, and you know that it is a `ButtonColor` type.

## Share types between files

If you want to share a type between files, create a new file `lib\index.d.ts` and export the type.

```tsx
export type ButtonColor = "red" | "blue" | "green";
```
