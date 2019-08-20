# Game Camera

The game camera controls the player's view into the [game world](game-world.md). By default, the game camera is set to the same size as your [game display](game-display.md).

You can change the camera's position within the game world, or you can set the camera to automatically follow a specific sprite \(such as the player's character\). You can also set certain objects \(images, text, etc.\) to stay "fixed" within the camera view, as part of the game's user interface.

### Phaser coding references in this section:

* Make Camera Follow Specific Sprite
* Focus Camera on Specific Object or Location
* Make Object Stay Fixed to Camera View
* Make Camera Shake
* Make Camera Flash
* Make Camera Fade

### Phaser API Documentation:

[Properties and Methods for Game Camera](https://photonstorm.github.io/phaser-ce/Phaser.Camera.html)

## Make Camera Follow Specific Sprite

If your game world is larger than your game display, you can set the game camera to automatically follow a specific sprite \(typically the player's character\) as it moves through the game world.

```javascript
game.camera.follow(player);
```

In the code above, `player` represents the name of the sprite to follow. If necessary, change this to match the variable name of your sprite.

You would typically add this command in your `create()` function after adding the sprite to the game.

### Follow Style

You also have the option of selecting a follow style with a specific type of "deadzone" — an invisible rectangular area in the middle of the game display where the camera won't move \(until and unless the sprite moves outside the rectangle\).

* `Phaser.Camera.FOLLOW_LOCKON` is the **default** style used if you don't select a style. It has no deadzone and will closely track the movement of the sprite.
* `Phaser.Camera.FOLLOW_PLATFORMER` is a style that has a tall, narrow deadzone.
* `Phaser.Camera.FOLLOW_TOPDOWN` is a style that has a square deadzone.
* `Phaser.Camera.FOLLOW_TOPDOWN_TIGHT` is a style that has a small square deadzone.

To use a different style, modify the command so it lists the follow style after the sprite's name:

```javascript
game.camera.follow(player, Phaser.Camera.FOLLOW_PLATFORMER);
```

### Unfollow Sprite

You can turn off the following of a sprite using this command:

```javascript
game.camera.unfollow();
```

This would be helpful if you want the camera to follow a different sprite or to focus on a specific object or location.

## Focus Camera on Specific Object or Location

You can make the camera move to focus on a specific display object \(image, sprite, etc.\) or location in the game world. This is a one-time camera movement \(not a continuous following behavior\). This might be helpful as visual feedback to draw the player's attention to an object or event, etc.

To focus the camera on the current location of a specific display object, use this command:

```javascript
game.camera.focusOn(explosion);
```

In the code above, `explosion` represents the name of the display object \(sprite, etc.\) to focus on. Change this to match the variable name of your object.

To focus the camera on a specific location using `x` and `y` coordinates, use this command:

```javascript
game.camera.focusOnXY(600, 300);
```

In the code above, `600` is the `x` coordinate, and `300` is the `y` coordinate. Changes these to match your desired coordinates.

## Make Object Stay Fixed to Camera View

You can make specific objects \(images, text, etc.\) stay fixed within the camera view, so the objects won't move when the game world scrolls. For example, a game's user interface elements \(score, health bar, etc.\) typically stay "fixed" to the camera view.

Phaser display objects \(such as images, text, sprites, etc.\) have a property called `fixedToCamera`. By default, this property is set to `false` — changing this value to `true` will make the object stay fixed within the camera view.

In order to change this property, the display object needs to be assigned to a variable name. For example, if the player's score were displayed using a text object named `scoreText`, you would use this command:

```javascript
scoreText.fixedToCamera = true;
```

Change `scoreText` to the variable name of your display object. Add the command in your `create()` function after adding the display object. \(The `x, y` position where the object is added will be the position where it will stay fixed, relative to the top-left corner of the game camera.\)

## Make Camera Shake

As a visual effect, you can make the game camera shake back and forth briefly \(and then return to normal\). This can be helpful as visual feedback for a collision, explosion, etc.

```javascript
game.camera.shake(0.02, 250);
```

* `0.02` represents the intensity of the shaking \(as a percentage of the camera size\). Change this value as needed, but you'll probably want to use a low number \(less than 0.1\).
* `250` represents the duration of the shaking in milliseconds \(1000 ms = 1 second\). Change this value to whatever duration you need.

### Shake Direction

You also have the option of selecting which direction the camera should shake:

* `Phaser.Camera.SHAKE_BOTH` is the **default** direction used if you don't select a direction. It will shake the camera both horizontally and vertically.
* `Phaser.Camera.SHAKE_HORIZONTAL` will only shake the camera from left to right.
* `Phaser.Camera.SHAKE_VERTICAL` will only shake the camera up and down.

To use a specific direction, modify the command so it also lists `true` plus the shake direction:

```javascript
game.camera.shake(0.02, 250, true, Phaser.Camera.SHAKE_VERTICAL);
```

## Make Camera Flash

As a visual effect, you can make the game camera briefly flash a specific color \(and then fade back to normal\). This can be helpful as visual feedback for a reward, punishment, etc.

```javascript
game.camera.flash(0x00ff00, 500);
```

* `0x00ff00` is a code for the color \(green in this example\). Change this to your own color by listing `0x` followed by a [CSS hex color code](https://www.w3schools.com/colors/colors_hexadecimal.asp).  For example, white would be `0xffffff`, red would be `0xff0000`, etc.
* `500` represents the duration of the flash in milliseconds \(1000 ms = 1 second\). Change this value to whatever duration you need.

## Make Camera Fade

As a visual effect, you can make the game camera fade to a specific color \(black, red, white, etc.\). It is basically the opposite of a camera flash \(except the game camera will remain that color until you reset it\). This can be helpful as visual feedback to indicate a character dying, reaching the end of a level, etc.

```javascript
game.camera.fade(0x000000, 500);
```

* `0x000000` is a code for the color \(black in this example\). Change this to your own color by listing `0x` followed by a [CSS hex color code](https://www.w3schools.com/colors/colors_hexadecimal.asp).  For example, white would be `0xffffff`, red would be `0xff0000`, etc.
* `500` represents the duration of the fade in milliseconds \(1000 ms = 1 second\). Change this value to whatever duration you need.

### Reset Camera to Normal

At the end of the camera fade, the game display will remain that color \(black, red, white, etc.\) until you reset it back to normal using this command:

```javascript
game.camera.resetFX();
```

