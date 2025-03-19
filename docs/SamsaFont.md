[_Samsa docs_](index.md)

## SamsaFont object

### Description

The `SamsaFont` object is the fundamental object in the Samsa Core library. It is created from a binary TrueType font stored in a [`SamsaBuffer`](SamsaBuffer.md). Note that `SamsaBuffer` is a subclass of the standard JavaScript [`DataView`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView). The `SamsaFont` object is used to create [`SamsaInstance`](SamsaInstance.md) objects, which are then used to render text using the `SamsaInstance.renderText()` method.

### Constructor

`SamsaFont()`

### Instance properties

### Instance methods

* `SamsaFont.glyphIdFromUnicode()`  
Takes an integer argument that is the Unicode code point of a character, and returns the id of the glyph that, by default, represents the character. If the font does not contain a glyph for the character, return value is 0.

* `SamsaFont.loadGlyphById()`  
Takes an integer argument that is the id of the glyph to be loaded, and returns a new `SamsaGlyph` object. The returned `SamsaGlyph` may be simple or composite; if composite, it can be converted to a simple glyph using `SamsaGlyph.decompose()`. It is valid to load glyph 0, which, according to the TrueType specification, represents the missing glyph and is typically an empty square or a square with a cross inside it. Note that this method only returns the default form of the glyph. For variations, it is necessary to  `SamsaInstance` object. When using this method directly, clients should keep track of those glyphs they have already loaded, thus not load the same glyph multiple times.

### Examples

The essential steps are to obtain a `SamsaBuffer` that contains a TrueType font, and make a `SamsaFont` from it.

```javascript
const buf = new SamsaBuffer(arrayBuffer); // arrayBuffer contains a TrueType font
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