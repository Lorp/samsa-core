[_Samsa docs_](index.md)

## Documentation

### Fundamental objects

The Samsa Core library offers four main objects to work with:

* [**SamsaFont**](SamsaFont.md)  
a font object, created from a TrueType font stored in an ArrayBuffer

* [**SamsaInstance**](SamsaInstance.md)  
an instance object, created from a SamsaFont object, that is used to render text

* [**SamsaGlyph**](SamsaGlyph.md)  
a glyph object, used internally and also available to clients

* [**SamsaBuffer**](SamsaBuffer.md)  
a memory buffer object with methods for encoding and decoding OpenType structures, used internally and also available to clients


### Quick start

The essential steps are:
* load a font file into an _ArrayBuffer_
* create a _SamsaBuffer_ object from the _ArrayBuffer_
* create a _SamsaFont_ object from the _SamsaBuffer_
* create a _SamsaInstance_ object from the _SamsaFont_
* call `renderText()` on the _SamsaInstance_ to obtain SVG strings

Here is sample code for Node.js that loads `filename` from disk, creates a SamsaFont object, creates a SamsaInstance object with variation axes set to certain locations, renders the string `hello, world!` as SVG, then saves the SVG to the file `render.svg`.

```javascript
const nodeBuffer = fs.readFileSync(filename);
const arrayBuffer = nodeBuffer.buffer;
const samsaBuffer = new SamsaBuffer(arrayBuffer);
const font = new SamsaFont(samsaBuffer);
const instance = font.instance({ wght: 900, wdth: 200 });
const svg = instance.renderText({ text: "hello, world!", fontSize: 72 });
fs.writeFileSync("render.svg", svg);
```

In a browser, you obtain an ArrayBuffer and process it similarly. The resulting SVG can be inserted into the DOM.

```javascript
const samsaBuffer = new SamsaBuffer(arrayBuffer);
const font = new SamsaFont(samsaBuffer);
const instance = font.instance({ wght: 900, wdth: 200 });
const svg = instance.renderText({ text: "hello, world!", fontSize: 72 });
document.getElementById("myDiv").innerHTML = svg;
```

If you’re loading a font file from a remote URL, you’ll probably use `fetch()` then `response.arrayBuffer()` to obtain the `ArrayBuffer`. If a font file is dragged onto the browser, the `ArrayBuffer` is obtained by enumerating the `e.dataTransfer.items` array, where `e` is the drop event. For each `item` in the array (there may be multiple items), if its `kind` property is equal to `file`, set `file = item.getAsFile()` and the promise `file.arrayBuffer()` will yield the `ArrayBuffer`.

### Flow diagram
The diagram illustrates how Samsa creates a SamsaFont object from an ArrayBuffer, creates a SamsaInstance from the SamsaFont, then renders text as SVG.

```mermaid
flowchart TD
    F1[TTF] --> A[ArrayBuffer]
    F2[WOFF2] --> A[ArrayBuffer]
    A[ArrayBuffer] --> B[SamsaBuffer]
    B --> C[SamsaFont]
    B[SamsaBuffer] --> |"SamsaBuffer.decodeWOFF2()"| C[SamsaFont]
    C --> |"SamsaFont.instance(axisSettings)"| D(SamsaInstance)
    D --> |"SamsaInstance.renderText({text: myText, fontSize: mySize, color: myColor})"| E(SVG)
```


