
floating-element
================

A simple javascript that gets a target element and makes sure it will always stay on the same offset in the page, and fixed on scroll.

Api: 
```javascript
	var elemenet = float_element($('.some-element')); //start
	element._stop_float(); //stop
```
Code (outline - maybe buggy): 
```javascript
	var float_element = (function() {
	var cache = [];
	return function(element) {
		
		var offset = element.offset(),
			cached = {
				position : element.css('position') || '',
				top      : element.css('top')      || '',
				left     : element.css('left')     || '',
				right    : element.css('right')    || '',
				bottom   : element.css('bottom')   || '',
				height   : element.css('height')   || '',
				width    : element.css('width')    || '',
				'z-index': element.css('z-index')  || ''
			};

		element._stop_float = function() {
			var css = cache.filter(function(current) { return current[0] == element; }).pop()[1];
			if(css) {
				delete element._stop_float;
		  		element.css(css);
			}
			return element;
		};
		
		cache.push([element, cached]);

		return element.css({
			position  : 'fixed',
			top       : offset.top,
			left      : offset.left,
			height    : element.outerHeight(),
			width     : element.outerWidth(),
			right     : offset.top + element.outerHeight(),
			bottom    : offset.left + element.outerWidth(),
			'z-index' : 9999,
		});
	};
}());
```
