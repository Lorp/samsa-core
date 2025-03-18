## SamsaGlyph object

### Description

The `SamsaGlyph` object represents a glyph as either:
* a simple glyph (an array of points and an array of end points for each contour)
* a composite glyph (an array of components)

### Constructor

`SamsaGlyph()`

### Instance properties

* `SamsaGlyph.font`  
the `SamsaFont` object that the glyph belongs to
* `SamsaGlyph.numPoints`  
the number of points in the glyph (excludes the 4 phantom points)
* `SamsaGlyph.points`  
array of points (each point is an array of integers of the form `[x, y, onCurve]`, where `onCurve` is 0 for off-curve points and 1 for on-curve points)
* `SamsaGlyph.numberOfContours`  
the number of contours in the glyph
* `SamsaGlyph.endPts`  
array of indices into the `points` array that indicate the last point of each contour
* `SamsaGlyph.instructionLength`  
the number of bytes in the glyph’s instructions
* `SamsaGlyph.name`  
the name of the glyph (often not used)
* `SamsaGlyph.components`  
array of glyph components (the `points` property still contains the 4 phantom points)
* `SamsaGlyph.tvts`  
array of tuple variation tables, which are used in variations

### Instance methods
* `SamsaGlyph.instantiate()`  
returns a new `SamsaGlyph` object, being a new version of this glyph transformed according to the `SamsaInstance` object passed as an argument
* `SamsaGlyph.svgPath()`  
returns a path string representing the glyph, suitable for use as the [`d`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d) attribute of an SVG [`<path>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path) element

### Examples


