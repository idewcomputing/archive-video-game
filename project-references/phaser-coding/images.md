# Images

You can add images to help create your game world \(backgrounds, etc.\) or as part of your game's user interface \(icons, etc.\). Phaser handles each image as a display object with properties and methods that allow you to modify the image in various ways.

### Phaser coding references in this section:

* Load Image
* Add Image
* Change Anchor for Image Position
* Change Image Position
* Make Image Stay Fixed to Camera View
* Hide Image \(and Unhide\)
* Rotate Image to Angle
* Scale Image \(or Flip Image\)
* Crop Image Using Rectangle
* Tint Image Using Color

### Phaser API Documentation:

[Properties and Methods for Image](https://photonstorm.github.io/phaser-ce/Phaser.Image.html)

## Load Image

Each image file \(also referred to as an asset\) must be loaded into the game's memory before you can add it to your game world.

```javascript
game.load.image('logo', 'assets/images/title-logo.png');
```

The command to load an asset is always placed in your `preload()` function.

* `'logo'` represents an asset key — a key is sort of like a variable name. You decide what string to use for the key, as long as each image asset has a unique key.
* `'assets/images/game-logo.png'` represents the folder path and filename of the image to load. Change this to match your specific folder path and filename.

## Add Image

Once an image asset has been loaded into memory, you can add the image to your game world.

If you have an image that simply needs to placed at a specific position and won't be modified in any way, you can add the image directly \(without assigning it to a variable\):

The command to add an image is typically placed in the `create()` function:

```javascript
game.add.image(0, 0, 'background');
```

* `0, 0` represent the `x` and `y` coordinates where the image will be positioned in the game world \(based on the image's anchor, which is its top-left corner by default\). Change these values to the position you need.
* `'background'` represents the asset key of the image to use. Replace this with your asset key.

However, if you will need to modify the image \(change image property, rotate image, etc.\), then you need to declare a variable and assign the image to the variable:

```javascript
gameTitle = game.load.image(400, 300, 'logo');
```

* `gameTitle` represents a variable name. Change this to the name of your variable.

If you only need to modify the image within the `create()` function, you can assign the text object to a local variable.

Otherwise, if you need to modify the image in the `update()` function or a custom function, then assign the image to a global variable.

## Change Anchor for Image Position

The `x` and `y` coordinates of a display object \(text, image, sprite, etc.\) represent the position of the object's **anchor**.

By default, an object's anchor is its top-left corner. However, you can change the anchor for an object \(e.g., to make the object's center become its anchor for positioning\).

For example, to change the anchor of a image named `gameTitle`:

```javascript
gameTitle.anchor.set(0.5, 0.5);
```

* The first number represents the horizontal position of the anchor, as a number between 0-1 \(0 = left edge, 0.5 = horizontal center, 1 = right edge\).
* The second number represents the vertical position of the anchor, as a number between 0-1 \(0 = top edge, 0.5 = vertical center, 1 = bottom edge\).

For most images, it will be fine to leave the anchor as the top-left corner \(the default\).

However, for some images, it may make more sense to set its anchor to its center `(0.5, 0.5)` — or possibly to its top-middle `(0.5, 0)` or bottom-middle `(0.5, 1)`.

## Change Image Position

If you need to change the position of an image, just change its `x` and/or `y` coordinates.

For example, you could change the position of a image named `gameTitle`:

```javascript
gameTitle.x = 50;
gameTitle.y = 20;
```

## Make Image Stay Fixed To Camera View

You can make an image stay fixed within the game camera view, so the image won't move when the game world scrolls.

Phaser display objects \(such as images, text, sprites, etc.\) have a property called `fixedToCamera`. By default, this property is set to `false` — changing this value to `true` will make the object stay fixed to the camera view.

For example, you can make an image named `gameTitle` stay fixed to the camera view:

```javascript
gameTitle.fixedToCamera = true;
```

## Hide Image \(and Unhide\)

You can hide \(or unhide\) an image by changing its `visible` property.

For example, to hide an image named `gameOverPic`:

```javascript
gameOverPic.visible = false;
```

To show the image, just unhide it:

```javascript
gameOverPic.visible = true;
```

## Rotate Image to Angle

You can rotate an image \(around its anchor point\) by changing the value of its `angle` property.

For example, you could rotate an image named `gameTitle`:

```javascript
gameTitle.angle = -10;
```

* `-10` represents the angle in degrees. Change this to any value from -180 to 180. Negative values rotate the image counterclockwise. Positive values rotate the image clockwise.

Phaser also has a property called `rotation` that can be used to get or set the angle of rotation of an image. The only difference is that the `rotation` property uses radians for its units, instead of degrees \(2π radians = 360°\).

## Scale Image \(or Flip Image\)

You can make an image larger or smaller by changing its `scale` property. You can also use this property to flip the image \(forming a mirror image\).

For example, you could scale an image named `gameTitle`:

```javascript
gameTitle.scale.setTo(0.5, 0.5);
```

* The first number represents the **horizontal scale**, which affects the image's width. Change this to the value you need for scaling.
* The second number represents the **vertical scale**, which affects the image's height. Change this to the value you need for scaling.

### Scale Values

* Each direction \(horizontal or vertical\) can be scaled using the same value \(which keeps the same image proportions\) — or can be scaled using different values \(which changes the proportions\).
* A scale value **less than 1** will make the image smaller in that direction. For example, a value of 0.5 would make the image half of its original size.
* A scale value of **exactly 1** will keep the image its original size in that direction.
* A scale value **greater than 1** will make the image larger in that direction. For example, a value of 2 would make the image twice its original size.
* Using a **negative** scale value will flip the image in that direction \(forming mirror image\). For example, a value of -1 will keep the image its original size but flip it.

## Crop Image Using Rectangle

You can apply a rectangular crop to an image. Any portion of the image **outside** the crop rectangle will be hidden from view. The crop is a reversible visual effect — you can change the crop or removed it if desired.

```javascript
livesCrop = new Phaser.Rectangle(0, 0, 75, 25);
livesBar.crop(livesCrop);
```

* The first line of code creates a rectangle and assigns it to a variable named `livesCrop`. Change the variable name for your rectangle.
  * `0, 0` represent the x and y coordinates of the top-left corner of the crop rectangle \(relative to the top-left corner of the image that it will crop\). Change these values based on where  you need to position your crop rectangle on the image.
  * `75, 25` represent the width and height \(in pixels\) of the crop rectangle. Change these values based on how much of the image you need to show.
* The second line of code applies the crop rectangle to an image named `livesBar`. Change the names to match the your image and crop rectangle.

### Change Crop Rectangle

If needed, you can change the size and/or position of your crop rectangle and then update to show the new cropped image.

If you need to change the size of the crop rectangle, change the width and/or height of the rectangle.

If you need to change the position of the top-left corner of the crop rectangle, change the `x` and/or `y` position of the rectangle. \(**NOTE:** This changes what part of the original image is shown, but the cropped image will still be positioned at the same location in the game world.\)

Once you've changed the value\(s\) for your crop rectangle, update the crop.

```javascript
// change whichever properties you need and then update
livesCrop.x = 25;
livesCrop.y = 0;
livesCrop.width = 100;
livesCrop.height = 25;
livesBar.updateCrop();
```

### Remove Crop Entirely

Unfortunately, there isn't a built-in method to directly "uncrop" an image once it has been cropped. Instead, the solution is to make the crop rectangle the same size as the original image and then apply this full-sized crop \(which will show the entire image\).

```javascript
// create rectangle to show full width and height of image
livesCrop = new Phaser.Rectangle(0, 0, 125, 25);
livesBar.crop(livesCrop);
```

## Tint Image Using Color

You can tint an image using a specific color. For example, if you have a background of a light blue sky \("daytime"\), you could tint it a reddish color to represent sunrise or sunset — or tint it a dark color to represent nighttime.

For example, to tint an image named `sky`:

```javascript
sky.tint = 0xff66cc;
```

* `0xff66cc` is a code for the color. Change this to your own color by listing `0x` followed by a [CSS hex color code](https://www.w3schools.com/colors/colors_hexadecimal.asp).

### How Tint Works

The tint color is added as a semi-transparent layer on top of the image, so the tint color will visually mix with the colors in the image.

For example, adding a yellow tint on top of a blue image will actually make the image look green \(yellow + blue = green\).

So you may need to try different tint colors to achieve your desired result \(and it may not be possible to get the exact result you want, depending on the original colors of the image\).

### Remove Tint

To remove the tint color and restore the image to its original appearance, change the tint color to `0xffffff` \(white represents no tint\):

```javascript
sky.tint = 0xffffff; // remove tint
```

