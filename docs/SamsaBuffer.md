[_Samsa docs_](index.md)

## SamsaBuffer object

### Description

The `SamsaBuffer` object is a subclass of the standard JavaScript [`DataView`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView). Unlike `DataView` it has a pointer property, `p`, that is updated when binary structures are read or written. 


#### Note
For highly optimized operations, it may be better to use a [`Uint8Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) view onto the underlying buffer of the `SamsaBuffer`. In this case, the client must handle the pointer during the optimized operation and update the pointer of the `SamsaBuffer` afterwards. But be careful of using similar multi-byte arrays such as [`Uint16Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint16Array) and [`Uint32Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint32Array): their byte order is “in the platform byte order”, which with x86 and Apple M-series chips is little-endian, unlike TrueType which is big-endian throughout.

### Constructor

`SamsaBuffer()`

### Inherited properties

* `SamsaBuffer.buffer`  
The underlying `ArrayBuffer` which stores the data of the `SamsaBuffer`.

* `SamsaBuffer.offset`  
The offset from the start of the underlying `ArrayBuffer` to the start of the `SamsaBuffer`. Note that `SamsaBuffer.offset` + `SamsaBuffer.byteOffset` is always <= `SamsaBuffer.buffer.byteLength`.

* `SamsaBuffer.byteLength`  
The length of the `SamsaBuffer` buffer in bytes. This may be less than the length of the underlying `ArrayBuffer`.

### Instance properties

* `SamsaBuffer.p`  
The offset from the start of the `SamsaBuffer`, where reading and writing will next take place. Attempting to read or write when `p` is not in the range [0, `SamsaBuffer.byteLength` - 1] is an error. Clients may update it directly, but there are also tell(), seek() and seekr() methods.

### Instance methods

#### Getting and setting the buffer pointer
* `SamsaBuffer.tell()`  
Returns the current value of the pointer `p`.

* `SamsaBuffer.seek(pos)`  
Sets the buffer pointer `SamsaBuffer.p` to `pos`. No return value.

* `SamsaBuffer.seekr(rpos)`  
Increments the buffer pointer `SamsaBuffer.p` by `rpos`. Equivalent `SamsaBuffer.p` += `rpos`. No return value.

#### Reading and writing simple data types

* `SamsaBuffer.u8`  
Getter and setter for an unsigned 8-bit integer.

* `SamsaBuffer.u16`  
Getter and setter for an unsigned 16-bit integer.

* `SamsaBuffer.u24`  
Getter and setter for an unsigned 24-bit integer.

* `SamsaBuffer.u32`  
Getter and setter for an unsigned 32-bit integer.

* `SamsaBuffer.i8`  
Getter and setter for a signed 8-bit integer.

* `SamsaBuffer.i16`  
Getter and setter for a signed 16-bit integer.

* `SamsaBuffer.i24`  
Getter and setter for a signed 24-bit integer.

* `SamsaBuffer.i32`  
Getter and setter for a signed 32-bit integer.

* `SamsaBuffer.tag`  
Getter and setter for an ASCII string of length 4.

#### Encoding and decoding complex data structures

* `SamsaBuffer.decodeGlyph()`  
Decodes a glyph from the buffer. Returns a `SamsaGlyph` object.

* `SamsaBuffer.encodeGlyph()`  
Encodes a glyph into the buffer. Returns the number of bytes written.

* [`SamsaBuffer.encodeInstance()`](SamsaBuffer.encodeInstance.md)  
Encodes an instance, being an entire static TrueType font, into the buffer. Returns the number of bytes written.

### Examples

