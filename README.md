anymatch
======
Javascript module to match a string against a regular expression, glob, string,
or function that takes the string as an argument and returns a truthy or falsy
value. The matcher can also be an array of any or all of these. Useful for
allowing a very flexible user-defined config to define things like file paths.

Usage
-----
`npm install anymatch --save`

#### anymatch (matchers, testString, [returnIndex], [startIndex], [endIndex])
* __matchers__: (_Array|String|RegExp|Function_)
String to be directly matched, string with glob patterns, regular expression
test, function that takes the testString as an argument and returns a truthy
value if it should be matched, or an array of any number and mix of these types.
* __testString__: (_String_) The string to test against the matchers.
* __returnIndex__: (_Boolean [optional]_) If true, return the array index of
the first matcher that that testString matched, or -1 if no match, instead of a
boolean result.
* __startIndex, endIndex__: (_Integer [optional]_) Can be used to define a
subset out of the array of provided matchers to test against. Can be useful
with bound matcher functions (see below). When used with `returnIndex = true`
preserves original indexing. Behaves the same as `Array.prototype.slice` (i.e.
includes array members up to, but not including endIndex).

```js
var anymatch = require('anymatch');

var matchers = [
	'path/to/file.js',
	'path/anyjs/**/*.js',
	/foo.js$/,
	function (string) {
		return string.indexOf('bar') !== -1 && string.length > 10
	}
];

anymatch(matchers, 'path/to/file.js'); // true
anymatch(matchers, 'path/anyjs/baz.js'); // true
anymatch(matchers, 'path/to/foo.js'); // true
anymatch(matchers, 'path/to/bar.js'); // true
anymatch(matchers, 'bar.js'); // false

// returnIndex = true
anymatch(matchers, 'foo.js', true); // 2
anymatch(matchers, 'path/anyjs/foo.js', true); // 1

// skip matchers
anymatch(matchers, 'path/to/file.js', false, 1); // false
anymatch(matchers, 'path/anyjs/foo.js', true, 2, 3); // 2
anymatch(matchers, 'path/to/bar.js', true, 0, 3); // -1
```

#### anymatch.matcher (matchers)
You can also use the `matcher` method to get a function that has already been
bound to your matcher(s). This can be used as an `Array.prototype.filter`
callback.

```js
var matcher = anymatch.matcher(matchers);

matcher('path/to/file.js'); // true
matcher('path/anyjs/baz.js', true); // 1
matcher('path/anyjs/baz.js', true, 2); // -1

['foo.js', 'bar.js'].filter(matcher); // ['foo.js']
```

License
-------
[MIT](https://raw.github.com/es128/anymatch/master/LICENSE)
