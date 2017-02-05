# pixels
> Simple browser-based pixel manipulation

```javascript
var imageData = context.getImageData(0, 0, width, height)

var pixels = Pixels(imageData)
pixels.replace('magenta', 'transparent')
imageData = pixels.compile()

context.putImageData(imageData, 0, 0)
```

# Documentation

## Factory
```javascript
// Create from `ImageData` to retain pixel information...
var pixels = Pixels(imageData)

// ...or from dimensions to create a blank slate.
var pixels = Pixels(width, height)
```
Creates and returns a new `Pixels` object from the specified `ImageData` object. Alternatively, you can create a new `Pixels` instance from a desired `width` and `height`.

## Methods

### `get`
```javascript
var color = pixels.get(x, y) // -> pixels
```
Gets the pixel color at `(x, y)` in the format `'rgba(red, green, blue, alpha)'`.

### `set`
```javascript
pixels.set(color)(x, y) // -> pixels.set
```
Sets the pixel color at `(x, y)` to `color`. Usage is identical to [`Grid.set`](https://github.com/semibran/grid#set), except with colors instead of integers.

`color` can be any valid HTML color, including the following:
- **Color names**: `'blue'`, `'dimgray'`, `'transparent'`...
- **Hex colors**: `'#dc143c'`, `'#DAB'`...
- **RGB and RGBA values**: `'rgb(204, 51, 51)'`, `'rgba(64, 120, 193, 0.5)'`.

### `each`
```javascript
pixels.each(callback) // -> pixels
```
Executes `callback` on each pixel in the pixel data.

```javascript
pixels.each((color, x, y) => x < pixels.width && pixels.set('gray')(x, y))
```

### `replace`
```javascript
pixels.replace(oldColor, newColor) // -> pixels
```

Replaces all appearances of `oldColor` in the pixel data with `newColor`.

This operation turns out to be really speedy since the colors are indexed in [`pixels.colors`](https://github.com/semibran/pixels#colors) instead of interspersed throughout the `ImageData` object.

### `compile`
```javascript
var imageData = pixels.compile() // -> ImageData
```
Alters [`pixels.data`](https://github.com/semibran/pixels#data) based on the changes made to [`pixels.grid`](https://github.com/semibran/pixels#grid) since instantiation. This is what actually converts indices back into pixel colors for you.

After receiving the `ImageData` object, paste it back on the context it came from to view the changes.
```javascript
context.putImageData(imageData, 0, 0) // Magic!
```

## Properties

### `width`
The width of the current image.

### `height`
The height of the current image.

### `colors`
An `Array` of the known and indexed colors for the current image.

### `grid`
A [`Grid`](https://github.com/semibran/grid) object in which each item in `grid.data` corresponds to a color index.

### `data`
The `ImageData` object that was passed into the factory (or created based on `width` and `height`).

# License
MIT
