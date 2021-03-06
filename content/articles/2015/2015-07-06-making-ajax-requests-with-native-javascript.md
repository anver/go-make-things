---
categories:
- Code
- JavaScript
date: '2015-07-06'
url: /making-ajax-requests-with-native-javascript/
title: Making AJAX requests with native JavaScript
---

Get the contents of another HTML document, or from a specific element in another document, without using jQuery. This only works for documents on the same domain. Supported back to IE8.

<!--more-->

```javascript
/**
 * Get data from a URL
 * @param  {String} url       The URL to get
 * @param  {Function} success Callback to run on success
 * @param  {Function} error   Callback to run on error
 */
var getURL = function ( url, success, error ) {

	// Feature detection
	if ( !window.XMLHttpRequest ) return;

	// Create new request
	var request = new XMLHttpRequest();

	// Setup callbacks
	request.onreadystatechange = function () {

		// If the request is complete
		if ( request.readyState === 4 ) {

			// If the request failed
			if ( request.status !== 200 ) {
				if ( error && typeof error === 'function' ) {
					error( request.responseText, request );
				}
				return;
			}

			// If the request succeeded
			if ( success && typeof success === 'function' ) {
				success( request.responseText, request );
			}
		}

	};

	// Get the HTML
	request.open( 'GET', url );
	request.send();

};

// Example 1: Generic Example
getURL(
	'/about',
	function (data) {
		// On success...
		var div = document.createElement(div);

	},
	function (data) {
		// On failure...
	}
);

// Example 2: Find a specific element in the HTML and inject it into the current page
getURL(
	'/about',
	function (data) {

		// Create a div and inject the HTML into it
		var div = document.createElement(div);
		div.innerHTML = data;

		// Find the element you're looking for in the div
		var elem = div.querySelector( '#some-element' );
		var location = document.querySelector( '#another-element' );

		// Quit if the element or the place where you want to inject it don't exist
		if ( !elem || !location ) return;

		// Inject the element into the DOM
		location.innerHTML = elem.innerHTML;

	}
);
```