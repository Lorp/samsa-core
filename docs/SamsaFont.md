[_Samsa docs_](index.md)

## SamsaFont object

### Description

The `SamsaFont` object is the fundamental object in the Samsa Core library. It is created from a binary TrueType font stored in a [`SamsaBuffer`](SamsaBuffer.md). Note that `SamsaBuffer` is a subclass of the standard JavaScript [`DataView`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView). The `SamsaFont` object is used to create [`SamsaInstance`](SamsaInstance.md) objects, which are then used to render text using the `SamsaInstance.renderText()` method.

### Constructor

`SamsaFont()`

### Instance properties

### Instance methods

### Examples

```javascript
const buf = new SamsaBuffer(arrayBuffer);
const font = new SamsaFont(buf);
```

There are numerous ways to get a font file into an `ArrayBuffer`. Common ways on the web are [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch) and [File drag and drop](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/File_drag_and_drop). Common ways in Node.js are [`fetch`](https://nodejs.org/api/globals.html#fetch) and [`fs.readFile()`](https://nodejs.org/api/fs.html#fsreadfilepath-options-callback).

Example using `fetch()` (works on the web and Node.js):

```javascript
const fontUrl = "https://example.com/font.ttf";

fetch(fontUrl)
	.then(response => {
		if (!response.ok) {
			throw new Error("Failed to fetch font");
		}

		const arrayBuffer = response.arrayBuffer();
		const buf = new SamsaBuffer(arrayBuffer);
		const font = new SamsaFont(buf);

		// do stuff with the font here

    });
```

Example using `fs.readFileSync()` (works in Node.js):

```javascript
const fs = require("fs");
const fontPath = "path/to/font.ttf";

const data = fs.readFileSync(fontPath);
const buf = new SamsaBuffer(data.buffer);
const font = new SamsaFont(buf);

// do stuff with the font here
```