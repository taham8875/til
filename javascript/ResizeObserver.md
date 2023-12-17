# `ResizeObserver`

It is easy in css to add styles to elements based on the size screen, but if you want to do actions depending on the size of specified elements you can use `ResizeObserver` API.

Resize Observer is the only way to detect element size changes and makes changing content/styles on a page based on resizing.

For example, if we want to change the color of a box based on its width

```js
const box = document.querySelector('.box');

const observer = new ResizeObserver(entries => {
    console.log(entries)
});

observer.observe(box);
```

`entries` holds the elements that the observer observes in an array, if you have multiple observed elements (e.g. `observer.observe(rectangle)` `observer.observe(card)`, the all will be in the array)

Let's complete our task

```js
const observer = new ResizeObserver(entries => {
    const boxElements = entries[0]
    const isSmall = boxElements.contentRect.width < 400;
    box.target.style.background = isSmall ? 'red' : 'blue';
});
```

> By default the `content-box` is used, but you can also use the `border-box`, and the `device-pixel-content-box`.
> `content-box`: the actual content area of the element
> `border-box`: takes into account the border and padding changes
> `device-pixel-content-box`: is similar to the `content-box` option but it takes into account the actual pixel size of the device it is rendering too

The most important two properties on any entry are the `contentRect` and the `target`.

> There are also `borderBoxSize`, `contentBoxSize` and `devicePixelContentBoxSize` properties on the entry object, which align with the box-sizing options.
