# Material UI Cheat Sheet

<p align="center">
  <a href="https://mui.com/" rel="noopener" target="_blank"><img width="150" src="./assets/logo.svg" alt="MUI logo"></a>
</p>

Material UI is a React UI framework that follows the Material Design guidelines. It is production-ready and one of the most popular React UI frameworks. It provides a set of reusable components that are customizable and accessible. This enables developers to build consistent and high-quality web apps in quick time.

# Installation

## Create a React App

To start using Material UI, create a react app to play around with the components. You can use the following command to create a new react app with vite.

```bash
$ npm create vite@latest
```

vite will prompt you some questions, choose a name for your application and choose react with typescript as the template.

Now run the following command to install dependencies and run the application.

```bash
$ cd <your-app-name>
$ npm install
$ npm run dev
```

If things go well, open `http://localhost:5173` in your browser and you should see your vite app running. open the source code and remove the boilerplate code.

## Adding Material UI

To install Material UI, run the following command.

```bash
$ npm install @mui/material @emotion/react @emotion/styled
```

# Typography

Use typography to present your design and content as clearly and efficiently as possible.

Before starting, add the roboto font to your app, it is up to you to use a CDN or install it via npm.

```html
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
/>
```

```bash
$ npm install @fontsource/roboto
```

Then, you can import it in your entry-point.

```ts
import "@fontsource/roboto/300.css";
import "@fontsource/roboto/400.css";
import "@fontsource/roboto/500.css";
import "@fontsource/roboto/700.css";
```

## Typography Variants

Material UI provides a set of predefined typography variants that you can use to style your text. The following table lists the available variants.

| Variant   |
| --------- | ------------------------------ |
| h1        | Equivalent to h1 in HTML.      |
| h2        | Equivalent to h2 in HTML.      |
| h3        | Equivalent to h3 in HTML.      |
| h4        | Equivalent to h4 in HTML.      |
| h5        | Equivalent to h5 in HTML.      |
| h6        | Equivalent to h6 in HTML.      |
| subtitle1 | Smaller the h6 variant.        |
| subtitle2 | Smaller the subtitle1 variant. |
| body1     | Normal paragraph text.         |
| body2     | Smaller than body1 variant.    |

create a new component called `MultiVariantTypography` and add the following code.

```tsx
import { Typography } from "@mui/material";

export default function MultiVariantTypography() {
  return (
    <>
      <Typography variant="h1">h1. Heading</Typography>
      <Typography variant="h2">h2. Heading</Typography>
      <Typography variant="h3">h3. Heading</Typography>
      <Typography variant="h4">h4. Heading</Typography>
      <Typography variant="h5">h5. Heading</Typography>
      <Typography variant="h6">h6. Heading</Typography>

      <Typography variant="subtitle1">
        subtitle1. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
        Quos blanditiis tenetur
      </Typography>
      <Typography variant="subtitle2">
        subtitle2. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
        Quos blanditiis tenetur
      </Typography>

      <Typography variant="body1">
        body1. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quos
        blanditiis tenetur unde suscipit, quam beatae rerum inventore
        consectetur, neque doloribus, cupiditate numquam dignissimos laborum
        fugiat deleniti? Eum quasi quidem quibusdam.
      </Typography>
      <Typography variant="body2">
        body2. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quos
        blanditiis tenetur unde suscipit, quam beatae rerum inventore
        consectetur, neque doloribus, cupiditate numquam dignissimos laborum
        fugiat deleniti? Eum quasi quidem quibusdam.
      </Typography>
    </>
  );
}
```

If you want to force a semantic element, you can use the `component` prop.

```tsx
<Typography variant="h1" component="h2">
  h1. Heading
</Typography>
```

Inspect the elements in the browser and you will see that the `h1` element is rendered as `h2`.

Another prop is `gutterBottom` which adds a bottom margin to the element.

```tsx
<Typography variant="h3" gutterBottom>
  h3. Heading
</Typography>
```

Open the browser and you will see that the `h3` element has a bottom margin.

# Buttons

Buttons allow users to take actions, and make choices, with a single tap.

## Button Variants

Material UI provides a set of predefined button variants that you can use to style your buttons. The following table lists the available variants.

| Variant   | Description                                                                                                                               |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| text      | Text buttons are typically used for less-pronounced actions, including those located in dialogs and cards.                                |
| outlined  | Outlined buttons are medium-emphasis buttons. They contain actions that are important, but aren’t the primary action in an app.           |
| contained | Contained buttons are high-emphasis, distinguished by their use of elevation and fill. They contain actions that are primary to your app. |

create a new component called `ButtonVariants` and add the following code.

```tsx
import { Button } from "@mui/material";

export default function ButtonVariants() {
  return (
    <>
      <Button variant="text">Text</Button>
      <Button variant="outlined">Outlined</Button>
      <Button variant="contained">Contained</Button>
    </>
  );
}
```

To add more life to the buttons, you can use the `href` prop to make the button behave like an anchor tag.

```tsx
<Button variant="text" href="https://google.com">
  Google
</Button>
```

## Button Colors

To color the buttons, you can use the `color` prop. There are 6 colors available (primary, secondary, error, info, success, warning). Create a new component called `ButtonColors` and add the following code.

```tsx
import { Button } from "@mui/material";

export default function ButtonColors() {
  return (
    <>
      <Button variant="contained" color="primary">
        Primary
      </Button>
      <Button variant="contained" color="secondary">
        Secondary
      </Button>
      <Button variant="contained" color="error">
        Error
      </Button>
      <Button variant="contained" color="info">
        Info
      </Button>
      <Button variant="contained" color="success">
        Success
      </Button>
      <Button variant="contained" color="warning">
        Warning
      </Button>
    </>
  );
}
```

Try to change the variant between `text`, `outlined` and `contained` and see how the shape of the button changes.

> Note you can change the default color palette of Material UI, we will cover the customization in a later section.

## Button Sizes

To change the size of the button, you can use the `size` prop. There are 3 sizes available (small, medium, large). Create a new component called `ButtonSizes` and add the following code.

```tsx
import { Button } from "@mui/material";

export default function ButtonSizes() {
  return (
    <>
      <Button variant="contained" size="small">
        Small
      </Button>
      <Button variant="contained" size="medium">
        Medium
      </Button>
      <Button variant="contained" size="large">
        Large
      </Button>
    </>
  );
}
```

## Add Icons

To start using Material UI icons, you need to install the following package.

```bash
$ npm install @mui/icons-material
```

Then, you can import the icons in your entry-point, go to the documentation to see the list of available icons and copy the import statement for the icon you want to use.

Create a new component called `ButtonWithIcon` and add the following code.

```tsx
import { Button } from "@mui/material";
import SendIcon from "@mui/icons-material/Send";

export default function ButtonWithIcon() {
  return (
    <>
      <Button variant="contained" startIcon={<SendIcon />}>
        Send
      </Button>

      <Button variant="contained" endIcon={<SendIcon />}>
        Send
      </Button>
    </>
  );
}
```

Note that you can use the `startIcon` and `endIcon` props to add icons to the button in different positions.

If you want a button with just icon without any text, you can use the `IconButton` component.

> It is recommended to add `aria-label` to the `IconButton` for accessibility.

```tsx
import { IconButton } from "@mui/material";
import SendIcon from "@mui/icons-material/Send";

export default function IconButtonWithIcon() {
  return (
    <>
      <IconButton>
        <SendIcon />
      </IconButton>
    </>
  );
}
```

Note the `color` and `size` props are also available for the `IconButton` component.

Another two props you may be interested in are `disabledElevation` and `disableRipple` , `disabledElevation` will remove the elevation of the button when it is disabled, and `disableRipple` will remove the ripple effect when the button is clicked. To better understand the props, create three buttons with the following props and see them in the browser.

```tsx
<Button variant="contained">
  Disabled
</Button>
<Button variant="contained" disabled disabledElevation>
  Disabled Elevation
</Button>
<Button variant="contained" disabled disableRipple>
  Disabled Ripple
</Button>
```

The final prop is `onClick` which is a callback function that will be called when the button is clicked.

```tsx
<Button variant="contained" onClick={() => alert("clicked")}>
  Click Me
</Button>
```

If you want to disable the button, you can use the `disabled` prop.

```tsx
<Button variant="contained" disabled>
  Disabled
</Button>
```

## Button Groups

To group a series of buttons together, you can use the `ButtonGroup` component.

```tsx
import { ButtonGroup, Button } from "@mui/material";

export default function ButtonGroups() {
  return (
    <>
      <ButtonGroup variant="contained">
        <Button>One</Button>
        <Button>Two</Button>
        <Button>Three</Button>
      </ButtonGroup>
    </>
  );
}
```

Note that the `variant` prop is passed to the `ButtonGroup` component instead of the `Button` component.

You can also use the `orientation` prop to change the orientation of the buttons.

```tsx
<ButtonGroup variant="contained" orientation="vertical">
  <Button>One</Button>
  <Button>Two</Button>
  <Button>Three</Button>
</ButtonGroup>
```

`color` and `size` props are also available for the `ButtonGroup` component.

As in the icon buttons, it is recommended to add `aria-label` to the `ButtonGroup` for accessibility.

## Toggle Buttons

Toggle buttons are buttons that can be pressed to change a setting or turn a feature/s on or off.

To create toggle buttons, you can use the `ToggleButton` component.

```tsx
import { ToggleButton, ToggleButtonGroup } from "@mui/material";
import { useState } from "react";

export default function ToggleButtons() {
  const [alignment, setAlignment] = useState<string[]>([]);

  const handleChange = (
    _event: React.MouseEvent<HTMLElement>,
    newAlignments: string[]
  ) => {
    setAlignment(newAlignments);
    console.log("newAlignments", newAlignments);
  };

  return (
    <>
      <ToggleButtonGroup
        value={alignment}
        onChange={handleChange}
        aria-label="text alignment"
        color="primary"
      >
        <ToggleButton value="left" aria-label="left aligned">
          Left
        </ToggleButton>
        <ToggleButton value="center" aria-label="centered">
          Center
        </ToggleButton>
        <ToggleButton value="right" aria-label="right aligned">
          Right
        </ToggleButton>
        <ToggleButton value="justify" aria-label="justified" disabled>
          Justify
        </ToggleButton>
      </ToggleButtonGroup>
    </>
  );
}
```

An important prop of the `ToggleButtonGroup` is exclusive, if it is set to true, only one button can be selected at a time.

```tsx
// ...
<ToggleButtonGroup
  value={alignment}
  onChange={handleChange}
  aria-label="text alignment"
  color="primary"
  exclusive
>
  {/* ... */}
</ToggleButtonGroup>
```

Note that this will require you to change the type of the `alignment` state to `string` instead of `string[]`. (or you can use enums)

```tsx
const [alignment, setAlignment] = useState<string>("");
// or
const [alignment, setAlignment] = useState<
  "left" | "center" | "right" | "justify"
>("");
```

and the handle change function will be changed to accept a string instead of a string array.

`orientation` prop is also available for the `ToggleButtonGroup` component, try to set it to `vertical` and see the result.

# Text Fields

Text fields allow users to enter text into a UI. They typically appear in forms and dialogs.

## Basic Text Field

To create a basic text field, you can use the `TextField` component.

```tsx
import { TextField } from "@mui/material";

export default function BasicTextField() {
  return <TextField label="Basic TextField" />;
}
```

## Text Field Variants

The default variant of the text field is `outlined`, you can change it to `standard` or `filled` by using the `variant` prop.

```tsx
import { TextField } from "@mui/material";

export default function TextFieldVariants() {
  return (
    <>
      <TextField label="Outlined TextField" variant="outlined" />
      <TextField label="Standard TextField" variant="standard" />
      <TextField label="Filled TextField" variant="filled" />
    </>
  );
}
```

`color` and `size` props are also available for the `TextField` component. Try to change the color to `secondary` and the size to `small` and see the result.

Another props you may be interested in are :

- `required`: if set, the label will be displayed with a `*` at the end.

```tsx
<TextField label="Required TextField" required />
```

- `helperText`: if set, the helper text will be displayed below the text field.

```tsx
<TextField label="please enter your name" helperText="Helper Text" />
```

- `type`: if set, the text field will be displayed with the specified type.

```tsx
<TextField label="Password" type="password" />
```

- `disabled`: if set, the text field will be disabled.

```tsx
<TextField label="Disabled TextField" disabled />
```

- `error`: if set, the text field will be displayed with an error.

```tsx
<TextField label="Error TextField" error />
```

This is not meaningful as it is, a use case is (for example) display an error when the user enters no password.

```tsx
const [password, setPassword] = useState("");

const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  setPassword(event.target.value);
};

<TextField
  label="Password"
  type="password"
  value={password}
  onChange={handleChange}
  error={password === ""}
  helperText={password === "" ? "Please enter a password" : ""}
/>;
```

## Text Field with prefix and suffix

To add a prefix or suffix to the text field, you can use the `InputAdornment` component.

```tsx
import { TextField, InputAdornment } from "@mui/material";

export default function TextFieldWithPrefixAndSuffix() {
  return (
    <>
      <TextField
        label="Weight"
        InputProps={{
          startAdornment: <InputAdornment position="end">Kg</InputAdornment>,
        }}
      />
      <TextField
        label="Price"
        InputProps={{
          endAdornment: <InputAdornment position="start">$</InputAdornment>,
        }}
      />
    </>
  );
}
```

Instead of text, you can use icons as prefix or suffix, try to add the visibility icon as a suffix to the password text field.

# Select

Selects allow users to select from a single-option menu.

## Basic Select

To create a basic select, you can use the `Select` component.

```tsx
import { TextField, MenuItem } from "@mui/material";

function BasicSelect() {
  const [country, setCountry] = useState("");

  console.log("country", country);

  const handleChange = (event) => {
    setCountry(event.target.value);
  };

  return (
    <TextField
      select
      label="Select country"
      value={country}
      onChange={handleChange}
      helperText="Please select your country"
    >
      <MenuItem value="USA">USA</MenuItem>
      <MenuItem value="Canada">Canada</MenuItem>
      <MenuItem value="Mexico">Mexico</MenuItem>
    </TextField>
  );
}
```

> Tip: A common approach is instead of hardcoding the options, you can fetch them from an API, then use the `map` function to render the options.

# Radio Buttons

Radio buttons allow users to select one option from a set.

## Basic Radio Buttons

To create a basic radio button, you can use the `Radio` component.

```tsx
import {
  Radio,
  RadioGroup,
  FormControl,
  FormControlLabel,
} from "@mui/material";

export function BasicRadio() {
  return (
    <FormControl>
      <FormLabel id="job">Job Experience</FormLabel>
      <RadioGroup aria-label="job" name="job" defaultValue="junior">
        <FormControlLabel value="junior" control={<Radio />} label="Junior" />
        <FormControlLabel value="senior" control={<Radio />} label="Senior" />
        <FormControlLabel value="manager" control={<Radio />} label="Manager" />
      </RadioGroup>
    </FormControl>
  );
}
```

To keep track of the selected value, you can use the `value` prop of the `RadioGroup` component.

```tsx
const [value, setValue] = useState("");

console.log("value", value);

const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  setValue((event.target as HTMLInputElement).value);
};
return (
  <FormControl>
    <FormLabel id="job">Job Experience</FormLabel>
    <RadioGroup
      aria-label="job"
      name="job"
      value={value}
      onChange={handleChange}
      defaultValue="junior"
    >
      <FormControlLabel value="junior" control={<Radio />} label="Junior" />
      <FormControlLabel value="senior" control={<Radio />} label="Senior" />
      <FormControlLabel value="manager" control={<Radio />} label="Manager" />
    </RadioGroup>
  </FormControl>
);
```

# Checkboxes

Checkboxes allow the user to select one or more items from a set.

## Basic Checkboxes

To create a basic checkbox, you can use the `Checkbox` component. Let's create a checkbox for the user to accept the terms and conditions.

```tsx
import { Checkbox, FormControlLabel } from "@mui/material";

export default function BasicCheckbox() {
  const [checked, setChecked] = useState(false);

  console.log("checked", checked);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setChecked(event.target.checked);
  };

  return (
    <FormControlLabel
      control={
        <Checkbox
          checked={checked}
          onChange={handleChange}
          inputProps={{ "aria-label": "controlled" }}
        />
      }
      label="I accept the terms and conditions"
    />
  );
}
```

You can also use icons as the checkbox, try to use the `checkedIcon` and `icon` props to change the icons.

```tsx
<Checkbox
  checked={checked}
  onChange={handleChange}
  checkedIcon={<Favorite />}
  icon={<FavoriteBorder />}
  inputProps={{ "aria-label": "controlled" }}
/>
```

# Switches

Switches toggle the state of a single setting on or off.

## Basic Switches

To create a basic switch, you can use the `Switch` component. Let's create a switch to toggle the dark mode.

```tsx
import { Switch, FormControlLabel } from "@mui/material";

export default function BasicSwitches() {
  const [darkMode, setDarkMode] = useState(false);

  console.log("darkMode", darkMode);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setDarkMode(event.target.darkMode);
  };

  return (
    <FormControlLabel
      control={
        <Switch
          checked={darkMode}
          onChange={handleChange}
          inputProps={{ "aria-label": "controlled" }}
        />
      }
      label="Dark Mode"
    />
  );
}
```

# Rating

Ratings allow users to provide numerical feedback on a set of items.

## Basic Rating

To create a basic rating, you can use the `Rating` component.

```tsx
import { Rating } from "@mui/material";

export default function BasicRating() {
  const [value, setValue] = useState<number | null>(null);

  console.log("value", value);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setValue(Number(event.target.value));
  };

  return (
    <Rating
      name="simple-controlled"
      value={value}
      onChange={handleChange}
      precision={0.5}
      max={10}
    />
  );
}
```

# Autocomplete

Autocomplete allows the user to select a value from a pre-populated list.

## Basic Autocomplete

To create a basic autocomplete, you can use the `Autocomplete` component.

```tsx
function BasicAutocomplete() {
  const [value, setValue] = useState<string | null>(null);

  console.log("value", value);

  const handleChange = (
    event: React.ChangeEvent<HTMLInputElement>,
    newValue: string | null
  ) => {
    // if you used event.target.value, you will 0 as value (bug you can experience)
    console.log("newValue", newValue, "event.target.value", event.target.value);
    setValue(newValue);
  };

  const top100Films = [
    "The Shawshank Redemption",
    "The Godfather",
    "The Godfather: Part II",
    "The Dark Knight",
    "Inception",
    "Whiplash",
  ];

  return (
    <Autocomplete
      value={value}
      onChange={handleChange}
      options={top100Films}
      renderInput={(params) => <TextField {...params} label="Movies" />}
    />
  );
}
```

By default, the autocomplete accept only the options provided, if you want to allow the user to enter a custom value, you can use the `freeSolo` prop.

```tsx
<Autocomplete
  // ...
  freeSolo
  // ..
/>
```

A common practice is to to fetch and update objects from an API, so you may pass a list of objects instead of array of strings. Try it and see the result. You will encounter a warning, read [this](https://stackoverflow.com/questions/61947941/material-ui-autocomplete-warning-the-value-provided-to-autocomplete-is-invalid) from stackoverflow to fix it.

# Box

The Box component serves as a wrapper component for most of the CSS utility needs.

## Basic Box

To create a basic box, you can use the `Box` component.

```tsx
import { Box } from "@mui/material";

export default function BasicBox() {
  return <Box>Hello World</Box>;
}
```

Inspect the source, you will see the the text Hello World is wrapped in a `div`. You may want to use another element (e.g. `section`, `article`) instead of `div`, you can use the `component` prop to change the element.

```tsx
return <Box component="section">Hello World</Box>;
```

But why not to use the element directly? The `Box` component provides some useful props that you can use to style the element. For example, you can use the `sx` prop to add styles to the element.

```tsx
return (
  <Box
    component="section"
    sx={{
      backgroundColor: "primary.main",
      color: "primary.contrastText",
      padding: "1rem",
      "&:hover": {
        backgroundColor: "secondary.main",
      },
    }}
  >
    Hello World
  </Box>
);
```

If ypu tried to apply inline style to the element, you cannot access either the palette colors or selectors like `&:hover`.

Another way to style the `Box` component is to use the material ui system props. Where you can write css as props.

```tsx
return (
  <Box
    display="flex"
    bgcolor="primary.main"
    color="primary.contrastText"
    p="1rem" // or p={4}
  >
    Hello World
  </Box>
);
```

For more on system props, check the material ui documentation.

# Stack

The Stack component is a layout component that allows you to manage the children components in one direction (1D layout) (row or column (default)) and add spacing between them. (`spacing` prop)

This get translated to a flexbox layout. (e.g. column to `display: flex; flex-direction: column;`, row-reverse to `display: flex; flex-direction: row-reverse;`)

## Basic Stack

To create a basic stack, you can use the `Stack` component.

```tsx
import { Stack, Button } from "@mui/material";

export default function BasicStack() {
  return (
    <Stack spacing={2} direction="row">
      <Button variant="contained">Button 1</Button>
      <Button variant="contained">Button 2</Button>
      <Button variant="contained">Button 3</Button>
    </Stack>
  );
}
```

Material UI provides another component called `Divider` which you can use to add a divider between the children.

```tsx
import { Stack, Button, Divider } from "@mui/material";

export default function BasicStack() {
  return (
    <Stack
      spacing={2}
      direction="row"
      divider={<Divider orientation="vertical" flexItem />}
    >
      <Button variant="contained">Button 1</Button>
      <Button variant="contained">Button 2</Button>
      <Button variant="contained">Button 3</Button>
    </Stack>
  );
}
```

# Grid

The Grid component is a layout component that allows you to manage the children components in a grid layout. (2D layout)

Grid component has two variations, for the parent and for the children.

For the parent, you can use the `container` prop

For the children, you can use the `item` prop

Notes :

- The grid component under the hood uses flexbox module.
- Like bootstrap, the grid system is based on 12 columns.
- Each item in the grid takes one or more columns.
- There are five breakpoints (xs, sm, md, lg, xl) and each breakpoint has a value (you can override the default values).

## Basic Grid

To create a basic grid, you can use the `Grid` component.

```tsx
import { Grid, Button } from "@mui/material";

export default function BasicGrid() {
  return (
    <Grid container spacing={2}>
      <Grid item xs={12} sm={6} md={4} lg={3} xl={2}>
        <Box bgcolor="primary.main">Box 1</Box>
      </Grid>
      <Grid item xs={12} sm={6} md={4} lg={3} xl={2}>
        <Box bgcolor="primary.main">Box 2</Box>
      </Grid>
      <Grid item xs={12} sm={6} md={4} lg={3} xl={2}>
        <Box bgcolor="primary.main">Box 3</Box>
      </Grid>
      <Grid item xs={12} sm={6} md={4} lg={3} xl={2}>
        <Box bgcolor="primary.main">Box 4</Box>
      </Grid>
      <Grid item xs={12} sm={6} md={4} lg={3} xl={2}>
        <Box bgcolor="primary.main">Box 5</Box>
      </Grid>
      <Grid item xs={12} sm={6} md={4} lg={3} xl={2}>
        <Box bgcolor="primary.main">Box 6</Box>
      </Grid>
    </Grid>
  );
}
```

Try to check the documentation and play around with the props.

# Paper

The Paper component is a layout component that allows you to create a surface to display content.

## Basic Paper

To create a basic paper, you can use the `Paper` component.

```tsx
import { Paper } from "@mui/material";

export default function BasicPaper() {
  return (
    <Paper sx={{ padding: "1rem" }} elevation={3}>
      <Box>Hello World</Box>
    </Paper>
  );
}
```

# Card

The Card component contains content and actions about a single subject.

## Basic Card

To create a basic card, you can use the `Card` component.

```tsx
import { Card, CardContent, CardActions, Button, CardMedia } from "@mui/material";

export default function BasicCard() {
  return (
    <Card>
      <CardMedia
        component="img"
        height="100"
        image="https://source.unsplash.com/random"
      />
      <CardContent>
        <Typography variant="h5" component="div">
          Card 1
        </Typography>
        <Typography variant="body2">
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Autem totam
          unde, quae magni voluptas debitis quia animi soluta necessitatibus
          tempore libero est asperiores voluptatibus esse eum quibusdam?
          Pariatur, voluptas excepturi?
        </Typography>
      </CardContent>
      <CardActions>
        <Button variant="text">Action 1</Button>
        <Button variant="text">Action 2</Button>
      </CardActions>
    </CardM>
  );
}
```

Note that the card component is build on top of the paper component.

# Accordion

The Accordion component is a layout component that allows you to create a collapsible content.

## Basic Accordion

To create a basic accordion, you can use the `Accordion` component.

```tsx
import { Accordion, AccordionSummary, AccordionDetails } from "@mui/material";

import ExpandMoreIcon from "@mui/icons-material/ExpandMore";

export default function BasicAccordion() {
  return (
    <Accordion>
      <AccordionSummary
        expandIcon={<ExpandMoreIcon />}
        aria-controls="panel1a-content"
        id="panel1a-header"
      >
        <Typography sx={{ color: "text.secondary" }}>
          I am an accordion
        </Typography>
      </AccordionSummary>
      <AccordionDetails>
        <Typography>
          Nulla facilisi. Phasellus sollicitudin nulla et quam mattis feugiat.
          Aliquam eget maximus est, id dignissim quam.
        </Typography>
      </AccordionDetails>
    </Accordion>
  );
}
```

Try to add more accordions and play around with the props. A common practice is to make tge user able to open only one accordion at a time. Try to do this with `useState` hook, `expanded` and `onChange` prop.

# Navbar

To create a navbar with material ui, you can use the `AppBar` and `Toolbar` components.

```tsx
import { AppBar, Toolbar, Typography } from "@mui/material";

import AcUnitIcon from "@mui/icons-material/AcUnit";

export default function BasicNavbar() {
  return (
    <AppBar position="static">
      <Toolbar>
        <IconButton
          size="large"
          edge="start"
          color="inherit"
          aria-label="menu"
          sx={{ mr: 2 }}
        >
          <AcUnitIcon />
        </IconButton>
        <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
          News
        </Typography>
        <Button color="inherit">Login</Button>
        <Button color="inherit">About</Button>
        <Button color="inherit">Contact</Button>
        <Button color="inherit">Career</Button>
      </Toolbar>
    </AppBar>
  );
}
```

# Menu

To add a menu to the navbar, you can use the `Menu` component.

```tsx
import { useState } from "react";
import {
  AppBar,
  Toolbar,
  Typography,
  IconButton,
  Button,
  Menu,
  MenuItem,
} from "@mui/material";

import AcUnitIcon from "@mui/icons-material/AcUnit";

export default function BasicNavbar() {
  const [anchorEl, setAnchorEl] = useState<null | HTMLElement>(null);
  console.log("anchorEl", anchorEl);
  const open = Boolean(anchorEl);
  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    setAnchorEl(event.currentTarget);
  };
  const handleClose = () => {
    setAnchorEl(null);
  };
  return (
    <AppBar position="static" color="transparent">
      <Toolbar>
        <IconButton size="large" edge="start" color="inherit" aria-label="logo">
          <AcUnitIcon />
        </IconButton>
        <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
          Navbar
        </Typography>
        <Stack direction="row" spacing={2}>
          <Button color="inherit">Features</Button>
          <Button color="inherit">Pricing</Button>
          <Button color="inherit">About</Button>
          <Button
            color="inherit"
            id="resources-button"
            aria-controls={open ? "resources-menu" : undefined}
            aria-haspopup="true"
            aria-expanded={open ? "true" : undefined}
            endIcon={<KeyboardArrowDownIcon />}
            onClick={handleClick}
          >
            Resources
          </Button>
          <Button color="inherit">Login</Button>
        </Stack>
        <Menu
          id="resources-menu"
          anchorEl={anchorEl}
          open={open}
          onClose={handleClose}
          anchorOrigin={{
            vertical: "bottom",
            horizontal: "right",
          }}
          transformOrigin={{
            vertical: "top",
            horizontal: "right",
          }}
          MenuListProps={{
            "aria-labelledby": "resources-button",
          }}
        >
          <MenuItem onClick={handleClose}>Blog</MenuItem>
          <MenuItem onClick={handleClose}>Podcast</MenuItem>
        </Menu>
      </Toolbar>
    </AppBar>
  );
}
```

# Link

As the name suggests, the `Link` component is used to create links.

```tsx
import { Link } from "@mui/material";

export default function BasicLink() {
  return (
    <Link href="#" underline="hover">
      Link
    </Link>
  );
}
```

`underline` prop can have on of the following values: `none`, `hover`, `always`.

`Link` component inherits the style from the parent `Typography` component if exists. So, if you want to change the size of the link, you can do it by changing the size of the parent `Typography` component.

```tsx
<Typography variant="h1">
  <Link href="#" underline="hover">
    Link
  </Link>
</Typography>
```

or directly

```tsx
<Link href="#" variant="h1" underline="hover">
  Link
</Link>
```

# Drawer

The `Drawer` component is used to create a sidebar.

```tsx
import { useState } from "react";

import {
  Drawer,
  List,
  ListItemIcon,
  ListItemText,
  IconButton,
  ListItemButton,
} from "@mui/material";

import MenuIcon from "@mui/icons-material/Menu";

import MailIcon from "@mui/icons-material/Mail";

export default function BasicDrawer() {
  const [open, setOpen] = useState(false);
  const toggleDrawer = () => {
    setOpen(!open);
  };
  return (
    <>
      <IconButton
        size="large"
        edge="start"
        color="inherit"
        aria-label="menu"
        sx={{ mr: 2, ml: 2 }}
        onClick={toggleDrawer}
      >
        <MenuIcon />
      </IconButton>
      <Drawer anchor="left" open={open} onClose={toggleDrawer}>
        <List>
          {["Inbox", "Starred", "Send email", "Drafts"].map((text, index) => (
            <ListItemButton key={text}>
              <ListItemIcon>
                <MailIcon />
              </ListItemIcon>
              <ListItemText primary={text} />
            </ListItemButton>
          ))}
        </List>
      </Drawer>
    </>
  );
}
```

You can change the `anchor` prop to `right`, `left`, `top` or `bottom` to change the position of the sidebar.

# Customizing Theme

To see the default theme, click [here](https://mui.com/material-ui/customization/default-theme/).

To customize the theme, you can use the `createTheme` function.

```tsx
import { createTheme } from "@mui/material/styles";

// create theme input must match the original theme type
const theme = createTheme({
  palette: {
    primary: {
      main: "#ff0000",
    },
  },
});
```

To use the custom theme, you can use the `ThemeProvider` component and wrap the entire app with it.

```tsx
import { ThemeProvider } from "@mui/material/styles";

// ...
<ThemeProvider theme={theme}>
  <App />
</ThemeProvider>;
```

Now open your browser to see the app running with the new red theme.

# Customizing Components

To customize a component, you can use the `styled` function.

```tsx
import { styled } from "@mui/material/styles";

const MyButton = styled(Button)(({ theme }) => ({
  color: theme.palette.secondary.main,
  backgroundColor: theme.palette.primary.main,
  "&:hover": {
    backgroundColor: theme.palette.primary.dark,
  },
  borderRadius: 0,
}));

export default function BasicStyled() {
  return <MyButton>My Button</MyButton>;
}
```
