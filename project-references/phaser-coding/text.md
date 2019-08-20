# Text

You can add text to your game to display feedback or information as part of your game's user interface. Phaser handles text as a display object with properties and methods that allow you to modify the text in various ways.

### Phaser coding references in this section:

* Add Text \(and Set Style\)
* Add Text Shadow
* Change Anchor for Text Position
* Change Text Position
* Rotate Text to Angle
* Make Text Stay Fixed to Camera View
* Hide Text \(and Unhide\)
* Change Text Content
* Clear Out Text Content
* Add Line Breaks in Text Content
* Change Text Style

### Phaser API Documentation:

[Properties and Methods for Text](https://photonstorm.github.io/phaser-ce/Phaser.Text.html)

## Add Text \(and Set Style\)

If you have **static** text \(i.e., won't change during game\) that just needs to be positioned and have basic style properties set, you can add the text object directly \(without assigning it to a variable\):

```javascript
game.add.text(350, 20, 'Health', { fontSize: '20px', fill: '#ffffff' });
```

The command to add text would be placed in your `create()` function \(probably towards the end, so that it will be displayed in front of other objects\).

* `350, 20` represent the `x` and `y` coordinates where the text will be positioned \(based on the text's anchor, which is its top-left corner by default\). Change these values to the position you need.
* `'Health'` represents the text string to display as the text. Replace this with your text.
* **OPTIONAL:** You can style the text by providing a comma-separated list of style properties and values inside curly braces `{ }` \(i.e., a JSON object\).

However, if the text will be **dynamic** \(i.e., will change during the game\) or needs to be modified beyond basic style properties \(such as adding a text shadow, hiding the text, etc.\), then you need to declare a variable and assign the text object to the variable:

```javascript
scoreText = game.add.text(50, 20, 'Score: 0', { fontSize: '20px', fill: '#ffffff' });
```

* `scoreText` represents a variable name. Change this to the name of your variable.

If you only need to change the text properties within the `create()` function, you can assign the text object to a local variable.

Otherwise, if you need to change the text in the `update()` function or a custom function, then assign the text object to a global variable.

### Style Properties for Text

The default style for text will be Arial 20pt bold in black — but you can modify any of these default properties.

The [Phaser API documentation for Text](https://photonstorm.github.io/phaser-ce/Phaser.Text.html) lists all the style properties that can be set for text. Here's a list of the most common style properties that you might use:

* `font` sets the font-family. By default, it will use `'Arial'`. The font must be already be installed on the user's computer, so stick with a [web-safe font family](https://www.w3schools.com/cssref/css_websafe_fonts.asp). If you're interested in using a special font, here's an [example of how to load a webfont from Google in Phaser](http://phaser.io/examples/v2/text/google-webfonts).
* `fontSize` sets the size of the font. By default, it will use `'20pt'` \(20pt is approximately 27px\). For more accurate layout, you may want to use px \(pixels\) to set the size, such as: `'30px'`
* `fontWeight` sets the weight of the font, such as `'normal'` or `'bold'`. By default, it will use `'bold'`.
* `fill` sets the color of the text. By default, it will use `'black'`. Set the value \(as a string\) using either a [HTML color name](https://www.w3schools.com/colors/colors_names.asp) or a [CSS hex code](https://www.w3schools.com/colors/colors_hexadecimal.asp).  

## Add Text Shadow

If you've assigned the text object to a variable, you can add a shadow behind the text:

```javascript
scoreText.setShadow(2, 2, '#000000', 4);
```

* The first value is the **horizontal offset** \(in pixels\) of the shadow.  Positive values shift the shadow to the right, while negative values shift it to the left. Use a value of `0` for no horizontal offset. \(A small value between 0-5 usually works well for offset.\)
* The second value is the **vertical offset** \(in pixels\) of the shadow. Positive values shift the shadow down, while negative values shift it up. Use a value of `0` for no vertical offset. \(A small value between 0-5 usually works well for offset.\)
* The third value is the **color** of the shadow. The color can listed either as a [CSS hex string](https://www.w3schools.com/colors/colors_hexadecimal.asp) or as a [RGBA string](https://www.w3schools.com/css/css3_colors.asp).  Colors that often work well for shadows are either `'#000000'` \(black\) or `'rgba(0, 0, 0, 0.7)'` \(slightly transparent black\) — but it depends on the color of the text and the color of the background.
* The fourth value is the **blur** \(in pixels\) of the shadow. Use a value of `0` for a sharp shadow. Use a value up to about `10` for a softer shadow.  \(A small value between 0-5 usually works well for blur.\)

## Change Anchor for Text Position

The `x` and `y` coordinates of a display object \(text, image, sprite, etc.\) represent the position of the object's **anchor**.

By default, an object's anchor is its top-left corner. However, you can change the anchor for an object \(e.g., to make the object's center become its anchor for positioning\).

For example, to change the anchor of a text object named `messageText`:

```javascript
messageText.anchor.set(0.5, 0.5);
```

* The first number represents the horizontal position of the anchor, as a number between 0-1 \(0 = left edge, 0.5 = horizontal center, 1 = right edge\).
* The second number represents the vertical position of the anchor, as a number between 0-1 \(0 = top edge, 0.5 = vertical center, 1 = bottom edge\).

For most text, it will be fine to leave the anchor as the top-left corner \(the default\).

However, for some text, it may make more sense to set its anchor to its center `(0.5, 0.5)` — or possibly to its top-middle `(0.5, 0)` or bottom-middle `(0.5, 1)`.

## Change Text Position

If you need to change the position of the text, just change its `x` and/or `y` coordinates.

For example, you could change the position of a text object named `messageText`:

```javascript
messageText.x = 400;
messageText.y = 150;
```

## Rotate Text to Angle

You can rotate text \(around its anchor point\) by changing the value of its `angle` property.

For example, you could rotate a text object named `gameTitle`:

```javascript
gameTitle.angle = -10;
```

* `-10` represents the angle in degrees. Change this to any value from -180 to 180. Negative values rotate the image counterclockwise. Positive values rotate the image clockwise.

Phaser also has a property called `rotation` that can be used to get or set the angle of rotation of an text. The only difference is that the `rotation` property uses radians for its units, instead of degrees \(2π radians = 360°\).

## Make Text Stay Fixed To Camera View

You can make an image stay fixed within the game camera view, so the image won't move when the game world scrolls.

Phaser display objects \(such as images, text, sprites, etc.\) have a property called `fixedToCamera`. By default, this property is set to `false` — changing this value to `true` will make the object stay fixed to the camera view.

For example, you can make a text object named `scoreText` stay fixed to the camera view:

```javascript
scoreText.fixedToCamera = true;
```

## Hide Text \(and Unhide\)

If you've assigned the text object to a variable, you can hide \(or unhide\) the text by changing its `visible` property.

For example, to hide the text:

```javascript
messageText.visible = false;
```

To show the text again, just unhide it:

```javascript
messageText.visible = true;
```

## Change Text Content

You can change the content of the text that is displayed by assigning a new text string to its `text` property:

```javascript
messageText.text = 'Game Over';
```

You can concatenate \(combine\) text strings and/or variable values using the `+` sign. JavaScript will treat the result as one continuous text string.

For example, if your game has a numerical variable named `score`, you could display a text label plus the value of `score`:

```javascript
scoreText.text = 'Score ' + score;
```

Notice that a blank space was included in the text string \(inside the quotes\). Otherwise, there would be no space between the word "Score" and the number when the text is displayed.

## Clear Out Text Content

You can clear out the text content \(make it blank\) by simply assigning an empty string to the `text` property:

```javascript
messageText.text = '';
```

## Add Line Breaks in Text Content

If necessary, you can add line breaks to split up your text into multiple lines.

To add a line break, insert: `\n`

For example, the following text will be split into 3 lines:

```javascript
messageText.text = 'Game Over\nYou Lost\nTry Again';
```

The text would be displayed as:

Game Over  
You Lost  
Try Again

### Change Text Alignment

If you do use multi-line text, you may want to change the text so each line will be center-aligned:

```javascript
messageText.align = 'center';
```

The options for `align` are `'left'`, `'center'`, or `'right'`. By default, the alignment will use `'left'`. This can also be set as one of the style properties when you add the text object.

## Change Text Style

If you've assigned the text object to a variable, you can later modify any of its style properties. Here's a few examples:

```javascript
messageText.fontSize = '40px';
messageText.fontWeight = 'normal';
messageText.fill = '#ff0000';
```

