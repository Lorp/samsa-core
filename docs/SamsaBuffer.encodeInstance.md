[_Samsa docs_](index.md)

## [SamsaBuffer](SamsaBuffer.md).encodeInstance() method

This method is used to generate a binary TrueType font in memory. The resulting font is a static font, not a variable font. Clients may use the font in memory in all the ways they might use any other TrueType font, for example as as a webfont, to offer it for download, or to save it to disk.

### Syntax

```javascript
encodeInstance(instance, options);
```

### Parameters

* `instance` (required)  
A `SamsaInstance` object representing the instance to be encoded in the `SamsaBuffer`.

* `options` (default `{}`)  

  * `options.checkSums` (default `false`)  
	Controls whether you valid checksums will be calculated. Set it to  `true` for valid checksums. In most applications, correct checksums are not necessary. Setting this option to `false` sets checksum fields to 0 and speeds up encoding slightly.
	
  * `options.glyphCompression` (default `true`)  
	Controls whether the glyph data will be compressed with standard TrueType encoding or not. Typically, TrueType uses 0, 1 or 2 bytes per coordinate as necessary. However it’s valid to use 2 bytes per coordinate throughout, which allows a simpler encoding routine. Set this to `false` to use 2 bytes consistently, triggering a significantly faster code path at the expense of significantly larger font files. Avoiding glyph compression is recommended when the font will immediately be compressed with WOFF2 encoding, because WOFF2 transforms coordinate data using its custom compressed scheme.
  
### Return value
The size of the resulting font in bytes, which is the number of bytes written to the `SamsaBuffer`.

If an `ArrayBuffer` or `Buffer` is needed that contains the font and is precisely the size of the font, the client must create a new buffer of the appropriate size and copy the data from the `SamsaBuffer` to the new buffer.


### Example

```javascript
// samsa-instantiate.js
// This reads a font file and instantiates it at a certain designspace location.
// It then writes the resulting font to disk.
// Example usage:
// node samsa-instantiate.js Gingham.ttf '{"wght": 850, "wdth": 70}' 

import fs from "fs";
import { SamsaFont, SamsaBuffer } from "./samsa-core.js";

// load the font into a SamsaBuffer and create a SamsaFont object
const filename = process.argv[2];
const fontVariationSettings = process.argv[3] ? JSON.parse(process.argv[3]) : {};
const nBuffer0 = fs.readFileSync(filename);
const sBuffer0 = new SamsaBuffer(nBuffer0.buffer);
const font = new SamsaFont(sBuffer0);

// create a SamsaInstance at a certain designspace location
const instance = font.instance(fontVariationSettings);

// create a new SamsaBuffer large enough to contain the instantiated font
const sBuffer1 = new SamsaBuffer(new ArrayBuffer(sBuffer0.byteLength * 4));

// encode the SamsaInstance as a TrueType font in the buffer
const fontLength = sBuffer1.encodeInstance(instance);

// create a Uint8Array that is a view onto the font data in the buffer
const u8View = new Uint8Array(sBuffer1.buffer, 0, fontLength);

// convert the Uint8Array to a Node buffer
const nBuffer1 = Buffer.from(u8View);

// write the buffer to disk
fs.writeFileSync("static-font.ttf", nBuffer1);
```

### Limitations

There is currently no easy way to specify the safe size of the buffer needed to contain the font (hence the `*4` in the example). The length of the variable font will usually be safe. However, if `options.glyphCompression` is set to `false`, then a significantly more memory might be needed.

Any variations handled by `MVAR`, `cvar`, `GSUB`, `GPOS` tables (and others) are not saved in the generated font. The to do list includes:
* modifying the `OS/2` table according to variations defined in the `MVAR` table
* modifying the `cvt` table according to variations defined in the `cvar` table
* modifying kerning according to variations defined in the `GPOS` and `GDEF` tables
* substituting glyphs according to Feature Variations defined in the `GSUB` table
