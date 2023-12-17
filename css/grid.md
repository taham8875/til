# grid

- `display: grid;` : to make a grid container
- `display: inline-grid;` : to make a grid container inline
- `grid-template-columns: number of columns in : [px, em, rem, %, fr, auto, repeat(number, length)];` : to set the columns of the grid

> `auto` vs `fr`:
>
> - `auto` : the column will take the width of its content
> - `fr` : the column will take the remaining space of the grid container
> `fr` is greedy, `auto` is shy, (try to use `auto` with `fr` and see how columns with `auto` will take the width of their content (just enough / the smallest possible) and columns with `fr` will take the remaining space of the grid container (biggest possible))

- `grid-template-rows: number of rows in : [px, em, rem, %, fr, auto, repeat(number, length)];` : to set the rows of the grid
- `grid-template-areas: "name of the area" "name of the area" "name of the area";` : to set the areas of the grid
- `grid-area: name of the area;` : to set the area of a grid item
- `row-gap: <length>;` : to set the gap between the rows of the grid
- `column-gap: <length>;` : to set the gap between the columns of the grid
- `gap: <row-gap> <column-gap>;` : to set the gap between the rows and the columns of the grid in one property
- `justify-content: start | end | center | stretch | space-around | space-between | space-evenly;` : to set the alignment of the grid items along the main axis (the default value is `stretch`)
- `align-content: start | end | center | stretch | space-around | space-between | space-evenly;` : to set the alignment of the grid lines along the cross axis (the perpendicular axis to the main axis) (the default value is `stretch`)
- `grid-column-start: <number>;` : to set the start column of a grid item
- `grid-column-end: <number>;` : to set the end column of a grid item
- `grid-row-start: <number>;` : to set the start row of a grid item
- `grid-row-end: <number>;` : to set the end row of a grid item
- `grid-column: <grid-column-start> / <grid-column-end>;` : to set the start and the end columns of a grid item in one property
- `grid-row: <grid-row-start> / <grid-row-end>;` : to set the start and the end rows of a grid item in one property

> e.g: - `grid-column: 1 / 3;` : the grid item will start at the first column and end at the third column
>
> - `grid-column: 2 / span 2;` : the grid item will start at the second column and end at the fourth column
> - `grid-column: span 2 ;` : the grid item will start at the first column and end at the third column
> Note that the end is exclusive

- `grid-area: <grid-row-start> / <grid-column-start> / <grid-row-end> / <grid-column-end>;` : to set the start and the end rows and columns of a grid item in one property
- `minmax(min, max);` : to set the minimum and the maximum size of a grid item
- `auto-fit` : to make the grid items fit the grid container
