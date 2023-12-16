# flex

- `display: flex;` : to make a flex container
- `display: inline-flex;` : to make a flex container inline
- `flex-direction: row | row-reverse | column | column-reverse;` : to set the direction of the main axis

> Note: if you changed `direction: rtl`, the main axis will be from right to left (the flex box will correspond to the direction of the language)

- `flex-wrap: nowrap | wrap | wrap-reverse;` : to set the wrapping of the flex items, is set to `nowrap` by default
- `flex-flow: <flex-direction> || <flex-wrap>;` : to set the direction and the wrapping of the flex items in one property
- `justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;` : to set the alignment of the flex items along the main axis
- `align-items: stretch | flex-start | flex-end | center | baseline;` : to set the alignment of the flex items along the cross axis (the perpendicular axis to the main axis) (the default value is `stretch`)
- `align-content: stretch | flex-start | flex-end | center | space-between | space-around;` : to set the alignment of the flex lines along the cross axis (the perpendicular axis to the main axis) (the default value is `stretch`)
- `align-self: auto | stretch | flex-start | flex-end | center | baseline;` : to set the alignment of a flex item along the cross axis (the perpendicular axis to the main axis) (the default value is `auto`)
- `flex-grow: <number>;` : to set the flex grow factor of a flex item (the default value is `0`)
- `flex-shrink: <number>;` : to set the flex shrink factor of a flex item (the default value is `1`)
- `order: <number>;` : to set the order of a flex item (the default value is `0`)
- `flex-basis: <length> | auto;` : to set the initial main size of a flex item (the default value is `auto`)
- `flex: <flex-grow> <flex-shrink> <flex-basis>;` : to set the flex grow factor, the flex shrink factor and the initial main size of a flex item in one property (the default value is `0 1 auto`)
