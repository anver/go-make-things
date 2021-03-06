---
title: "Getting formatted months with vanilla JS"
date: 2019-10-07T10:30:00-04:00
draft: false
categories:
- Code
- Design and UX
- JavaScript
---

With vanilla JS, you can use the `getMonth()` to get back the month for any Date object.

```js
var date = new Date();
var month = date.getMonth();
```

But, it returns an integer for the month, starting at `0`. That means January is `0` instead of `1`, and December is `11` instead of `12`.

Today, I wanted to show you a trick for getting back a formatted month name (like `January` or `Jan`).

*The technique we're using today comes courtesy of [Andrew Borstein](https://andrewborstein.com/). Thanks Andrew!*

## Creating a helper function

We're going to create a little helper function, `getMonths()`, to help us out.

We'll accept two arguments. The first will be the `month` ID, starting with `1` for January. The second will be an optional boolean, `short`. If `true`, we'll use short names for our months, like `Jan` instead of `January`.

```js
var getMonths = function (month, short) {
	// Do stuff...
};
```

## Getting an array of month names

First, let's get a list of months.

We'll create a `format` variable. If `short` is defined and `true`, we'll use a `short` month format. Otherwise, we'll use a `long` one.

```js
var getMonths = function (month, short) {

	// Create month names
	var format = short ? 'short' : 'long';

};
```

Next, we'll create an array of 12 entries, and use [the `Array.map()` method](https://vanillajstoolkit.com/reference/arrays/array-map/) to create a new array with our months names.

Inside the `Array.map()` callback, we'll `return` a `new Date()` object, passing in a random year and our month. Then, we'll call the `toLocalString()` method on it to get back a month name. We'll specify that we want the `month` in our `format`.

```js
var getMonths = function (month, short) {

	// Create month names
	var format = short ? 'short' : 'long';
	var monthNames = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11].map(function (mon) {
		return new Date(2000, mon).toLocaleString({}, {month: format});
	});

};
```

At this point, we'll now have a `monthNames` array, containing either long names (`["January", "February", ...]`) or short ones (`["Jan", "Feb"]`).

Because we used the `toLocalString()` method, these will be translated into local month name conventions.

## Getting back a month

Next, we need to get back our months.

Let's provide a way to get back all months as an array. If no `month` argument is provided, we'll return the entire `monthNames` array.

```js
var getMonths = function (month, short) {

	// Create month names
	var format = short ? 'short' : 'long';
	var monthNames = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11].map(function (mon) {
		return new Date(2000, mon).toLocaleString({}, {month: format});
	});

	// Return month name (or all of them)
	return monthNames;

};
```

If a `month` is passed in, we'll subtract `1` from it (so that `1`, January, maps to `0` in the array, and so on). Then, we'll get that item from the `getMonths` array and return it.

```js
var getMonths = function (month, short) {

	// Create month names
	var format = short ? 'short' : 'long';
	var monthNames = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11].map(function (mon) {
		return new Date(2000, mon).toLocaleString({}, {month: format});
	});

	// Return month name (or all of them)
	if (month) {
		return monthNames[(month - 1)];
	}
	return monthNames;

};
```

[Here's a demo on CodePen.](https://codepen.io/cferdinandi/pen/mddyEqm) You can also find this on [the Vanilla JS Toolkit](https://vanillajstoolkit.com/).

## Browser Compatibility

The limiting feature for this helper function is the `toLocaleString()` method.

This will work in all modern browsers, and IE11 and up.