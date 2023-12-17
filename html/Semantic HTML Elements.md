# Semantic HTML Elements

Why use semantic HTML elements?

- Easier to read and understand
- Better for SEO
- Better for accessibility (screen readers, etc.)

Examples of semantic HTML elements:

- `<header>`
- `<nav>`
- `<main>`
- `<section>`
- `<article>`
- `<aside>`
- `<footer>`
- `<figure>` and `<figcaption>`
- `<details>`
- `<summary>`
- `<h1>` to `<h6>`

> There are something special about `<article>` that you should know. It is a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable. Examples include: a forum post, a magazine or newspaper article, or a blog entry.

and much more, there are about [100 semantic HTML elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

Use these elements whenever possible instead of `<div>`s and `<span>`s.

When to use `<div>`s and `<span>`s?

- When there is no other semantic element that fits the content
- When you need to group elements together for styling purposes

## Semantics are not for html only

The same concept can be for both css and javascript

consider a function that takes a string parameter, and returns an `<li>` element with that string as its textContent. Would you need to look at the code to understand what the function did if it was called `build('Peach')`, or `createLiWithContent('Peach')`?

consider styling a list with li elements representing different types of fruits. Would you know what part of the DOM is being selected with `div > ul > li`, or `.fruits__item`?
