# Game World

The game world contains all of your game objects. Your game world can extend beyond the [game display](game-display.md), and the [game camera](game-camera.md) can follow the player's sprite as it moves through your game world.

### Phaser coding references in this section:

* Set Size of Game World
* Positions of Objects in Game World
* Change Anchor for Object's Position
* Get Current Position of Object
* Change Position of Object
* Layering of Objects in Game World
* Change Order of Object's Layer
* Get Center Position of Game World
* Get Random Position in Game World
* Get Boundaries of Game World
* Make Sprite Wrap Around World

### Phaser API Documentation:

[Properties and Methods for Game World](https://photonstorm.github.io/phaser-ce/Phaser.World.html)

## Set Size of Game World

By default, Phaser will set your game world to be the same size as your game display. However, you can set the boundaries of your game world to be larger than your game display.

```javascript
game.world.setBounds(0, 0, 3000, 600);
```

This Phaser command should be placed near the beginning of your `create()` function.

* The first two numbers `0, 0` represent \(in order\) the `x` and `y` coordinates of the top-left corner of your game world. Typically, you'll use `0, 0` for your top-left corner.
* The next two numbers `3000, 600` represent \(in order\) the width and height \(in pixels\) of your game world. Change these to any values that you want \(as long as they are equal to or greater than the size of your game display\).

**Side-Scrolling Game:** Make the width of the game world larger than the width of the game display. Keep the height of the game world the same as the height of the game display.

**Vertically-Scrolling Game:** Keep the width of the game world the same as the width of the game display. Make the height of the game world larger than the height of the game display.

**Top-Down Scrolling Game:** Make both the width and height of the game world larger than the width and height of the game display.

## Positions of Objects in Game World

Every display object \(images, sprites, text, etc.\) in the game world has a position defined by its `x` and `y` coordinates: `x, y`

The `x` and `y` coordinates represent pixel positions, relative to the top-left corner of the game world \(which is `0, 0`\).

* The `x` coordinates start at `0` \(left boundary\) and increase as you move from left to right.
* The `y` coordinates start at `0` \(top boundary\) and increase as you move from top to bottom.

For example, an object placed at coordinates `250, 100` will be positioned 250 pixels to the right and 100 pixels down, relative to the top-left corner.

![](../../.gitbook/assets/game-world-coordinates.png)

## Change Anchor for Object's Position

The `x` and `y` coordinates of a display object \(image, sprite, text, etc.\) represent the position of the object's **anchor**.

By default, an object's anchor is its top-left corner. However, you can change the anchor for an object \(e.g., to make the object's center become its anchor for positioning\).

For example, to change the anchor of a sprite named `player`:

```javascript
player.anchor.set(0.5, 0.5);
```

* The first number represents the horizontal position of the anchor, as a number between 0-1 \(0 = left edge, 0.5 = horizontal center, 1 = right edge\).
* The second number represents the vertical position of the anchor, as a number between 0-1 \(0 = top edge, 0.5 = vertical center, 1 = bottom edge\).

For some objects, it will make sense to leave the anchor as the object's top-left corner \(the default\).

For other objects \(especially sprites\), it will make more sense to set an object's anchor to its center `(0.5, 0.5)`.

For certain objects, it might even make sense to set the object's anchor to its top-middle `(0.5, 0)` or its bottom-middle `(0.5, 1)`.

## Get Current Position of Object

The current `x` and `y` coordinates of an object's position \(based on its anchor\) are stored as properties of the object.

For example, to get the current position of a sprite named `player`:

* `player.x` represents its current `x` coordinate
* `player.y` represents its current `y` coordinate

## Change Position of Object

You can also set or modify the `x` and/or `y` coordinates of an object to change its position in the game world.

For example, you could change the position of a sprite named `enemy` by assigning it specific `x` and `y` coordinates:

```javascript
enemy.x = 300;
enemy.y = 500;
```

As another example, you could move a sprite named `player` relative to its current position:

```javascript
player.x = player.x + 5;
player.y = player.y - 5;
```

In the example above, the `player` sprite would be moved 5 pixels to the right and 5 pixels up from its current position.

* Increasing the `x` coordinate moves an object to the right, while decreasing `x` moves the object to the left.
* Increasing the `y` coordinate moves an object down, while decreasing `y` moves the object up.

## Layering of Objects in Game World

Every display object \(images, sprites, text, etc.\) is added to the game world as a layer \(using the [CSS z-index](https://www.w3schools.com/cssref/pr_pos_z-index.asp) property\). These object layers are stacked on top of each other, meaning they can overlap \(and hide\) other objects behind them.

**IMPORTANT:** The order in which game objects are added in the `create()` function determines the stacking of these layers in the game world.

So if you have an image that is supposed to be a background for your game, that image should be added first in the `create()` function, so it will be the farthest back layer. Each new display object \(image, sprite, text, etc.\) that is added in the code will appear in front of the previous layers.

For example, if you added the player's sprite first and then added the background image, the player's sprite would be hidden behind the background. To fix this, change the order of the code, so the background image is added first and then the player's sprite is added.

**TIP:** You can use the layering of the game objects to help create a sense of depth within your otherwise flat 2D game world. For example, if your game takes place in a forest, you could add images of trees in the background, then add the player's sprite, and then add more trees in the foreground. As the player's sprite moves through the game world, it will pass in front of the background trees but behind the foreground trees — giving the illusion of depth within your virtual forest.

## Change Order of Object's Layer

Your Phaser game maintains a display list of the stacking order of every object in your game world. If necessary, you can modify the relative order of an object's layer within this list.

For example, the following could be used to modify the stacking order of a sprite named `enemy`:

* `enemy.moveDown();` would move this sprite farther back by one layer
* `enemy.moveUp();` would move this sprite closer to the front by one layer
* `enemy.sendToBack();` would make this sprite the farthest back layer
* `enemy.bringToTop();` would make this sprite the closest layer

## Get Center Position of Game World

* `game.world.centerX` is the center `x` coordinate
* `game.world.centerY` is the center `y` coordinate

## Get Random Position in Game World

* `game.world.randomX` is a random `x` coordinate \(will be different every time you use it\)
* `game.world.randomY` is a random `y` coordinate \(will be different every time you use it\)

## Get Boundaries of Game World

* `game.world.left` is the left `x` boundary \(typically equals `0`\)
* `game.world.right` is the right `x` boundary \(typically equals width of game world\)
* `game.world.top` is the top `y` boundary \(typically equals `0`\)
* `game.world.bottom` is the bottom `y` boundary \(typically equals height of game world\)

## Make Sprite Wrap Around World

You can make a sprite \(or a member of a sprite group\) wrap around to the opposite side of world if it moves outside of the game world boundaries.

For example, if the sprite moves out of the world across the right boundary, the sprite will reappear at the left boundary of the world \(and vice versa\). The same is true for the top and bottom boundaries.

This feature is typically used for single-screen games. For example, in _Asteroids_, the asteroids and the player's ship wrap around the world. However, this feature can be used for any size of game world.

To make an individual sprite wrap around the world:

```javascript
game.world.wrap(player, 32);
```

The code example above must be placed in the `update()` function \(otherwise, it will not work\).

* `player` represents the sprite's variable name. Change this to the name of your sprite.
* `32` represents the amount of padding \(in pixels\) to allow before wrapping the sprite. In this case, the sprite can move 32 pixels outside of the world before the sprite will be wrapped around to the other side. Change this to any value you want \(including 0 for no padding\).

### Use Padding to Prevent Visual Splitting of Sprite

The advantage of using padding is that it can prevent the sprite from appearing to be visually split — i.e., part of the sprite shown on one side of the world and the rest of the sprite shown on the opposite side of the world.

To prevent this visual split, use a padding value that is at least half the width or height of the sprite \(whichever is greater\).

### Make Each Member of Group Wrap Around World

To make each member of a sprite group wrap around the world:

```javascript
asteroidGroup.forEach(function (asteroid) {
    game.world.wrap(asteroid, 20);
});
```

Again, the code example above must be placed in the `update()` function \(otherwise, it will not work\).

* `asteroidGroup` represents the group's variable name. Change this to the name of your sprite group.
* `asteroid` is a local variable used in the function to represent an individual member of the group. Change this to any local variable name that makes sense.
* `20` represents the amount of padding to allow before wrapping the sprite. Change this to any value you need.

### Make Sprite Only Wrap Horizontally

If you only want the sprite to wrap around the world horizontally between left and right \(but not between top and bottom\):

```javascript
game.world.wrap(player, 32, true, false);
```

### Make Sprite Only Wrap Vertically

If you only want the sprite to wrap around the world vertically between top and bottom \(but not between left and right\):

```javascript
game.world.wrap(player, 32, false, true);
```

