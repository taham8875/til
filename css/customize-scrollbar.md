# Customize Scrollbar With CSS

To customize the scrollbar with CSS, you need to use the `::-webkit-scrollbar` pseudo-element. It is used to style the scrollbar with your own CSS styles.

i.e. to change the width of the scrollbar:

```css
::-webkit-scrollbar {
  width: 10px;
}
```

To change the color of the scrollbar track:

```css
::-webkit-scrollbar-track {
  background: #f1f1f1;
}
```

To change the color of the scrollbar thumb:

```css
::-webkit-scrollbar-thumb {
  background: #888;
}
```

To customize the corners of the scrollbar thumb:

```css
::-webkit-scrollbar-thumb-corner {
  background: red;
}
```

You can use pseudo-classes with the `::-webkit-scrollbar` pseudo-element to style the scrollbar when the user hovers over it:

```css
::-webkit-scrollbar-thumb:hover {
  background: #555;
}
```

You can customize the scrollbar in any element, e.g. some div element:

```css
.some-custom-element::-webkit-scrollbar {
  width: 10px;
}
