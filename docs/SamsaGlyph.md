[_Samsa docs_](index.md)

## SamsaGlyph object

### Description

The `SamsaGlyph` object represents a glyph as either:
* a simple glyph (an array of points and an array of end points for each contour)
* a composite glyph (an array of components)

### Constructor

`SamsaGlyph()`

### Instance properties

* `SamsaGlyph.components`  
Array of glyph components. It is empty for simple glyphs. Note that the `points` property still contains the 4 phantom points.
* `SamsaGlyph.endPts`  
Array of indices into the `points` array that indicate the last point of each contour.
* `SamsaGlyph.font`  
The `SamsaFont` object that the glyph belongs to.
* `SamsaGlyph.instructionLength`  
The number of bytes in the glyph’s instructions.
* `SamsaGlyph.instructions`  
If `SamsaGlyph.instructionLength` is non-zero, this is a `SamsaBuffer` object containing the instruction bytes for the glyph; otherwise it is `undefined`.
* `SamsaGlyph.name`  
The name of the glyph (often not used).
* `SamsaGlyph.numberOfContours`  
The number of contours in the glyph.
* `SamsaGlyph.numPoints`  
The number of points in the glyph. It excludes the 4 phantom points.
* `SamsaGlyph.points`  
Array of points (each point is an array of integers of the form `[x, y, onCurve]`, where `onCurve` is 0 for off-curve points and 1 for on-curve points). Note that the length of this array is 4 more than `SamsaGlyph.numPoints`, because 4 [phantom points](https://learn.microsoft.com/en-us/typography/opentype/spec/tt_instructing_glyphs#phantom-points) are appended.
* `SamsaGlyph.tvts`  
Array of tuple variation tables (TVTs), which are used in variations. Each TVT specifies a ‘region’ in a variable font’s designspace in which variation is to be applied, and a set of ‘deltas’, where each delta is a vector representing the maximum displacement of a point in _x_ and _y_.

### Instance methods
* `SamsaGlyph.decompose()`  
Returns a new `SamsaGlyph` object that is a simple glyph, by recursively decomposing the glyph if it is composite. The new glyph is visually identical to the original glyph. The `components` array of the returned glyph will be empty. It is valid to call this method on a simple glyph, in which case the method returns a new glyph that is a copy of the original.
* `SamsaGlyph.instantiate()`  
Returns a new `SamsaGlyph` object, being a new version of this glyph transformed according to the `SamsaInstance` object passed as an argument.
* `SamsaGlyph.svgPath()`  
Returns a path string representing the glyph, suitable for use as the [`d`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d) attribute of an SVG [`<path>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path) element.

### Examples

```javascript
// display the array of points for the glyph for the character "A"
const nodeBuffer = fs.readFileSync(filename);
const arrayBuffer = nodeBuffer.buffer;
const samsaBuffer = new SamsaBuffer(arrayBuffer);
const font = new SamsaFont(samsaBuffer);
const string = "A";
const glyphId = font.glyphIdFromUnicode(string.codePointAt(0));
const glyph = font.loadGlyphById(glyphId);
console.log(glyph.points);
```
