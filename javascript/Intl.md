# Intl Cheat Sheet

[Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl#constructor_properties) comprise the ECMAScript Internationalization API, which provides language sensitive string comparison, number formatting, date and time formatting, and more.

# Basic Usage

## `DateTimeFormat`

```js
const formatter = new Intl.DateTimeFormat("en-US");
console.log(formatter.format(new Date())); // 7/15/2023
```

If you want to pass the default locale, you can use `undefined` as the first argument.

```js
const formatter = new Intl.DateTimeFormat(undefined);
console.log(formatter.format(new Date())); // 7/15/2023
```

Customize the format by passing an object as the second argument.

```js
const formatter = new Intl.DateTimeFormat(undefined, {
  weekday: "long",
  year: "2-digit",
  month: "long",
  day: "numeric",
});

console.log(formatter.format(new Date())); // Saturday, July 15, 23
```

## `RelativeTimeFormat`

```js
const formatter = new Intl.RelativeTimeFormat("en-US");
console.log(formatter.format(1, "day")); // in 1 day
console.log(formatter.format(-1, "day")); // 1 day ago
```

# `NumberFormat`

```js
const formatter = new Intl.NumberFormat("en-US");
console.log(formatter.format(1234567.89)); // 1,234,567.89
```

For Currency, pass the currency code as the second argument.

```js
const formatter = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
});

console.log(formatter.format(1234567.89)); // $1,234,567.89
```

For units, pass the unit as the second argument.

```js
const formatter = new Intl.NumberFormat("en-US", {
  style: "unit",
  unit: "mile-per-hour",
});

console.log(formatter.format(1234567.89)); // 1,234,567.89 mph
```

You can change the notation by passing the `notation` option. (useful for social media apps like youtube videos count)

```js
const formatter = new Intl.NumberFormat("en-US", {
  notation: "compact",
});

console.log(formatter.format(1234567.89)); // 1.2M
```

For scientific notation, pass the `notation` option as `scientific`.

```js
const formatter = new Intl.NumberFormat("en-US", {
  notation: "scientific",
});

console.log(formatter.format(1234567.89)); // 1.23456789E6
```

If you have a long fraction part, you can use the `maximumFractionDigits` option to limit the fraction part.

```js
const formatter = new Intl.NumberFormat("en-US", {
  maximumFractionDigits: 2,
});

console.log(formatter.format(12.1546843584864)); // 12.15
```
