# MakeCode Arcade / API Reference Guide / Images

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Images** module.

This module creates images, fill, draw, and set pixels.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Creating

### create

Create a pixel image of a certain size.

```js
function image.create(width: number, height: number): Image;
```

An empty rectangular image is created for the number of pixels wide and high you ask for. Empty means that the image contains all transparent pixels. Pixels colors are set in the image using the image functions.

You can create an zero size image (width = 0 and height = 0) but the function will actually create and image of a default size.

**_Parameters_**

- **width:** the number of pixels wide (x dimension) for the image.
- **height:** the number of pixels high (y dimension) for the image.

**_Returns_**

an empty (transparent) image of the desired size.

**_Examples_**

**1. Create a 32 x 32 image and draw an orange border around it.**

```js
let orangeBox = image.create(32, 32)
for (let i = 0; i < 32; i++) {
    orangeBox.setPixel(0, i, 13)
    orangeBox.setPixel(i, 0, 13)
    orangeBox.setPixel(i, 31, 13)
    orangeBox.setPixel(31, i, 13)
}
let boxSprite = sprites.create(orangeBox)
```

### clone

Copy an image to make another just like it.

```js
(method) Image.clone(): Image;
```

A new image is created that is a copy of the original. The image layout and pixel colors are the same.

**_Returns_**

an image that is an exact copy of the original image.

**_Examples_**

**1. Create and clone image**

Make a image layout for a stick figure person. Clone the image and display it below the original image.

```js
let stickPerson1 = img`
. . . a a . . .
. . a . . a . .
. . . a a . . .
. . . a . . . .
a a a a a a a .
. . . a . . . .
. . . a . . . .
. . . a . . . .
. . . a . . . .
. . a   a . . .
. a . . . a . .
. a a . . a a .
`
let stickPerson2 = stickPerson1.clone()

let showPerson1 = sprites.create(stickPerson1)
let showPerson2 = sprites.create(stickPerson2)
showPerson2.y = showPerson1.y + 16
```

### screenImage

Get an image of everything displayed on the screen.

```js
function image.screenImage(): Image;
```

**_Returns_**

an image of everything displayed on the screen.


## Drawing

### drawLine

Draw a line from one point in an image to another point.

```js
(method) Image.drawLine(x0: number, y0: number, x1: number, y1: number, c: number): void;
```

The pixels are located at points in the image. The point is a coordinate which is two values that are a horizontal position and a vertical position. A line is drawn by setting the color of the pixels directly between two coordinates. The line has a width of one pixel.

**_Parameters_**

- **x0:** a number that’s the horizontal pixel location of the first coordinate.
- **y0:** a number that’s the vertical pixel location of the first coordinate.
- **x1:** a number that’s the horizontal pixel location of the second coordinate.
- **y1:** a number that’s the vertical pixel location of the second coordinate.
- **c:** the number of the color of the pixels in the line. Color numbers are value between 0 and 15 which select a color from the current palette of colors.

**_Examples_**

**1. Draw a big X in image by making two diagonal lines.**

```js
let showBigX: Sprite = null
let drawBigX: Image = null
drawBigX = image.create(32, 32)
drawBigX.fill(1)
drawBigX.drawLine(0, 0, 31, 31, 10)
drawBigX.drawLine(0, 31, 31, 0, 10)
showBigX = sprites.create(drawBigX)
```

### drawRect

Draw an ouline around a rectangle with a pixel color.

```js
(method) Image.drawRect(x: number, y: number, w: number, h: number, c: number): void;
```

A rectangular outline is drawn in an image. The line width of the outline is one pixel wide drawn with the color you choose.

**_Parameters_**

- **x:** a number that’s the horizontal pixel location of the upper-left corner of the rectangle to draw.
- **y:** a number that’s the vertical pixel location of the upper-left corner of the rectangle to draw.
- **w:** a number that’s the width in pixels of the rectangle.
- **h:** a number that’s the height in pixels of the rectangle.
- **c:** the number of the color to draw the rectangular outline with. Color numbers are value between 0 and 15 which select a color from the current palette of colors.

### setPixel

Set the color of a pixel location in an image.

```js
(method) Image.setPixel(x: number, y: number, c: number): void;
```

Pixels are an individual point of color in an image. Pixels are arraged in columns (its x value) and rows (its y value). All of the pixels together make the rectangle of the image. Some pixels might have no color (transparent) and other pixels might have a color value set for them.

**_Parameters_**

- **x:** the column number of the pixel location.
- **y:** the row number of the pixel location.
- **c:** the number of the new color to set for the pixel location of x and y. Color numbers are value between 0 and 15 which select a color from the current palette of colors.

### getPixel

Get the color number of a pixel at a location in an image.

```js
(method) Image.getPixel(x: number, y: number): number;
```

**_Parameters_**

- **x:** a number that’s the horziontal position of the pixel.
- **y:** a number that’s the vertical position of the pixel.

**_Returns_**

a number that’s the color of the pixel at location x and y in the image. The color number is a value between 0 and 15.

### fill

Set all the pixels of an image to one color.

```js
(method) Image.fill(c: number): void;
```

**_Parameters_**

- **c:** the number of the color to set all the pixels in image to. Color numbers are value between 0 and 15 which select a color from the current palette of colors.

### fillRect

Set all the pixels within a rectangular area of an image to one color.

```js
(method) Image.fillRect(x: number, y: number, w: number, h: number, c: number): void;
```

You can fill all of pixels in a rectanluar area at once with a color. The fill rectangle can be either the entire image area or some smaller part of it.

**_Parameters_**

- **x:** a number that’s the horizontal pixel location of the upper-left corner of the fill rectangle.
- **y:** a number that’s the vertical pixel location of the upper-left corner of the fill rectangle.
- **w:** a number that’s the width in pixels of the fill rectangle.
- **h:** a number that’s the height in pixels of the fill rectangle.
- **c:** the number of the color to set all the pixels in the rectangle to. Color numbers are value between 0 and 15 which select a color from the current palette of colors.

### flipX

Flip the pixels horizontally from one side to the other side.

```js
(method) Image.flipX(): void;
```

The pixels in each half of the image are moved across to the other half. This happens in the horizontal direction. They are “flipped” across an imaginary line that goes down the middle of the image. So, in an image with a size of 10 x 10, a pixel at a location of (3, 4) will move to (6, 4) and the pixel that was originally at (6, 4) will move to (3, 4).

If an image has on odd number of columns, then the “center line” goes through a column of pixels. When those pixels are “flipped” they keep their same location and don’t move.

### flipY

Flip the pixels vertically from one side to the other side.

```js
(method) Image.flipY(): void;
```

The pixels in each half of the image are moved across to the other half. This happens in the vertical direction. They are “flipped” across an imaginary line that goes across the middle of the image. So, in an image with a size of 10 x 10, a pixel at a location of (3, 2) will move to (3, 8) and the pixel that was originally at (3, 8) will move to (3, 2).

If an image has on odd number of rows, then the “center line” goes through a row of pixels. When those pixels are “flipped” they keep their same location and don’t move.

### replace

Replace all the pixels of one color with pixels of another color.

```js
(method) Image.replace(from: number, to: number): void;
```

**_Parameters_**

- **from:** the color number of the pixels to change.
- **to:** the color number of the replacement pixels.
