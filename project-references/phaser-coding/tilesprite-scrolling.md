# Tilesprite Scrolling

You can add one or more background images to your game, and if your game world is larger than the game display, you can also make your game world scroll automatically to follow the player. This can be done without using a tilesprite — you can just position whatever image\(s\) you want throughout your game world, and make the game camera follow the player.

### What's a Tilesprite?

A [tilesprite](http://phaser.io/examples/v2/tile-sprites/tiling-sprite) is used to create a **single background image that looks like it is scrolling as the player moves**. A tilesprite can be used for a single-screen game or for an extended game world.

However, not just any image will work as a tilesprite. There are two main criteria: pattern and size.

In order to create a smooth scrolling effect, the tilesprite image **must have a pattern that matches seamlessly across opposite edges** — which could be its horizontal edges \(left and right\), its vertical edges \(top and bottom\), or both — depending on which direction\(s\) your game world is supposed to scroll.

![](../../.gitbook/assets/sky-clouds.jpg)

In the image above, notice that the pattern of the sky and clouds along the left edge matches perfectly with the right edge. If you were to place multiple copies of this image side-by-side, the pattern would repeat seamlessly, so this image would be perfect for a horizontally scrolling game.

The other criteria for a tilesprite background image is that it **should be the exact size of your game display**. If necessary, use an image editor to resize your tilesprite image to match the width and height of your game display. \(Alternatively, you could change the size of your game display to match the size of the image.\)

### Phaser coding references in this section:

* Load Image for Tilesprite
* Add Tilesprite
* Make Tilesprite Stay Fixed to Camera View
* Scroll Tilesprite When Player Moves
* Create Parallax Effect to Simulate Depth

### Phaser API Documentation:

[Properties and Methods for Tilesprite](https://photonstorm.github.io/phaser-ce/Phaser.TileSprite.html)

## Load Image for Tilesprite

Just like other images, the image file for your tilesprite must be loaded into the game's memory before you can add it to your game world.

```javascript
game.load.image('sky', 'assets/images/sky-clouds.jpg');
```

The command to load an asset is always placed in your `preload()` function.

* `'sky'` represents an asset key — a key is sort of like a variable name. You decide what string to use for the key, as long as each image asset has a unique key.
* `'assets/images/sky-clouds.jpg'` represents the folder path and filename of the image to load. Change this to match your specific folder path and filename.

## Add Tilesprite

Once the image asset has been loaded into memory, you can add the tilesprite to your game world.

You will need to declare a global variable for the tilesprite and assign the tilesprite to the variable in your `create()` function.

For example, to add a tilesprite to the variable named `skyTile`:

```javascript
skyTile = game.add.tilesprite(0, 0, 800, 600, 'sky');
```

* `0, 0` represent the `x` and `y` position for the top-left corner of the tilesprite image. This is normally where you want to position tilesprite images.
* `800, 600` represent the width and height of the tilesprite image. Be sure the size of your tilesprite matches the size of your game display. Change these numbers to match your tilesprite and game display.
* `'sky'` is the asset key for the tilesprite image.

Remember that the code to add background images, such as tilesprites, should be listed in your `create()` function before the code to add other sprites, images, text, etc.

### Layer Multiple Tilesprites

You can also layer multiple images to create a more complex background. This only works if the layer\(s\) in front have some transparent areas, so you can see part of the other layer\(s\) behind.

For example, imagine the background of your game will consist of these three tilesprites:

* `skyTile` will be the farthest back tilesprite layer — an image showing sky and clouds
* `mountainTile` will be the middle tilesprite layer — an image showing a mountain ridge but with a transparent sky \(so you can see part of the `skyTile` behind it\)
* `cityTile` will be the closest tilesprite layer — an image showing a silhouette of city buildings but with a transparent sky \(so you can see part of the `mountainTile` and `skyTile` behind it\)

Add the tilesprites in the order of their "depth" from back to front:

```javascript
skyTile = game.add.tilesprite(0, 0, 800, 600, 'sky');
mountainTile = game.add.tilesprite(0, 0, 800, 600, 'mountains');
cityTile = game.add.tilesprite(0, 0, 800, 600, 'city');
```

## Make Tilesprite Stay Fixed to Camera View

If your game world is larger than your game display, then you must make the tilesprite stay fixed to the game camera's view.

For example, to make a tilesprite named `skyTile` stay fixed to the camera view:

```javascript
skyTile.fixedToCamera = true;
```

If you have layered multiple tilesprite images to create your background, then be sure to fix each of them to the camera.

## Scroll Tilesprite When Player Moves

In order to create a realistic scrolling effect, the tilesprite should scroll in the **opposite** direction that the player is moving. For example, if the player's sprite is moving to the right, then the tilesprite should scroll to the left.

You can scroll the tilesprite by changing its `tilePosition.x` and/or `tilePosition.y` values — depending on whether the tilesprite should scroll horizontally, vertically, or both:

* Change the tilesprite's `tilePosition.x` to simulate horizontal scrolling:
  * If the player is moving to the right, make the tilesprite scroll left by decreasing its `tilePosition.x`.
  * If the player is moving to the left, make the tilesprite scroll right by increasing its `tilePosition.x`.
* Change the tilesprite's `tilePosition.y` to simulate vertical scrolling:
  * If the player is moving down, make the tilesprite scroll up by decreasing its `tilePosition.y`.
  * If the player is moving up, make the tilesprite scroll down by increasing its `tilePosition.y`.

The code to scroll the tilesprite should be included in your `update()` function \(after any player movement has occurred\).

If the tilesprite scrolling in your game seems too fast or too slow \(relative to the player's movement\), adjust the value used to scroll the tilesprite:

* If the scrolling seems too fast, use a smaller value.
* If the scrolling seems too slow, use a larger value.

There are several options for how to scroll the tilesprite, depending on how your game is set up:

1. If you have a single-screen game that moves the player by directly changing the player's position \(`x` and/or `y` coordinates\), use **Option 1** below.
2. If you have a single-screen game that moves the player by changing its velocity or acceleration, use **Option 2** below.
3. If you have an extended game world with a game camera set to follow the player, use **Option 3** below. 

### Option 1: Scroll Tilesprite When Changing Player's Position

If you have a **single-screen game** \(game world is same size as game display\) that moves the player's sprite by directly increasing or decreasing its `x` or `y` coordinates, then move the tilesprite at the same time \(but in the opposite direction\).

For example, this code checks the `arrowKey` inputs used to move the `player` sprite and **horizontally** scrolls a tilesprite named `skyTile`:

```javascript
if (arrowKey.right.isDown) {
    // move player to right, and scroll tilesprite to left
    player.x += 5;
    skyTile.tilePosition.x -= 5;
}
else if (arrowKey.left.isDown) {
    // move player to left, and scroll tilesprite to right
    player.x -= 5;
    skyTile.tilePosition.x += 5;
}
```

The example above scrolls the tilesprite by the same amount that the player's position is moved. As necessary, adjust the scrolling value to a number that feels right for your game.

If your game needs to scroll the tilesprite **vertically**, use similar code to change the `y` positions of the player and tilesprite.

**NOTE:** `+=` is the **addition assignment operator**. It is a shortcut for adding a value to a variable's current value.

* `player.x += 5` is the same as `player.x = player.x + 5`

`-=` is the **subtraction assignment operator**. It is a shortcut for subtracting a value from a variable's current value.

* `player.x -= 5` is the same as `player.x = player.x - 5`

[W3Schools Tutorial on JavaScript Assignment Operators](https://www.w3schools.com/js/js_assignment.asp)

### Option 2: Scroll Tilesprite Using Player's Velocity

If you have a **single-screen game** \(game world is same size as game display\) that moves the player's sprite by changing its velocity \(or acceleration\), then move the tilesprite in the opposite direction by subtracting the player's velocity \(reduced by a scaling factor\).

For example, to **horizontally** scroll a tilesprite named `skyTile`:

```javascript
skyTile.tilePosition.x -= player.body.velocity.x / 50;
```

* Dividing the player velocity by `50` represents a scaling factor, so the scrolling is not too fast. As necessary, adjust this scaling factor to a number that feels right for your game.

If your game needs to scroll the tilesprite **vertically**, use a similar command:

```javascript
skyTile.tilePosition.y -= player.body.velocity.y / 50;
```

### Option 3: Scroll Tilesprite Using Camera's Position

If your game has an **extended game world** \(larger than the game display\) with a game camera set to follow the player, then you can scroll the tilesprite relative to the game camera's position.

For example, to **horizontally** scroll a tilesprite named `skyTile`:

```javascript
skyTile.tilePosition.x = game.camera.x * -0.2;
```

* Multiplying the camera position by `-0.2` represents a scaling factor, so the scrolling is not too fast. Using a negative value makes the tilesprite scroll in the opposite direction of the camera movement.  As necessary, adjust this scaling factor to a number that feels right for your game.

If your game needs to scroll the tilesprite **vertically**, use a similar command:

```javascript
skyTile.tilePosition.y = game.camera.y * -0.2;
```

## Create Parallax Effect to Simulate Depth

In the real world, we experience a visual phenomenon called parallax — as we move, objects in the distance seem to shift more slowly compared to objects that are closer to us. This is one of the ways that our vision system detects depth and distance.

You can simulate parallax in your game by creating a background consisting of multiple tilesprite layers and then scrolling these tilesprites at different rates based on their depth — the farthest back tilesprite layer should scroll the slowest, while the tilesprite layers that are closer should scroll faster.

### 1. Create Layered Background using Multiple Tilesprites

In your `create()` function, create a layered background by adding multiple tilesprites in the order of their "depth" from back to front. Two or three tilesprite layers will be sufficient for a parallax effect.

For an extended game world, be sure to make each tilesprite stay fixed to the camera.

For example, imagine the background of your game consists of these three tilesprites:

* `skyTile` is the farthest back tilesprite layer — an image showing sky and clouds
* `mountainTile` is the middle tilesprite layer — an image showing a mountain ridge but with a transparent sky \(so you can see part of the `skyTile` behind it\)
* `cityTile` is the closest tilesprite layer — an image showing a silhouette of city buildings but with a transparent sky \(so you can see part of the `mountainTile` and `skyTile` behind it\)

### 2. Scroll Tilesprites at Different Rates Based on Depth

Create the parallax effect by scrolling the tilesprites at different rates \(by using different scaling factors\) based on their relative depth.

The furthest back tilesprite layer should scroll the slowest, while the other tilesprite layers should scroll faster as they get closer to the front.

Scroll the tilesprites using one of the three options shown below.

### Option 1: Single-screen game that moves player by changing its position

```javascript
if (arrowKey.right.isDown) {
    // move player to right, and scroll tilesprites to left
    player.x += 5;
    skyTile.tilePosition.x -= 2;
    mountainTile.tilePosition.x -= 3;
    cityTile.tilePosition.x -= 4;
}
else if (arrowKey.left.isDown) {
    // move player to left, and scroll tilesprites to right
    player.x -= 5;
    skyTile.tilePosition.x += 2;
    mountainTile.tilePosition.x += 3;
    cityTile.tilePosition.x += 4;
}
```

As necessary, adjust the scrolling rates to numbers that feel right for your game.

### Option 2: Single-screen game that moves player by changing its velocity or acceleration

```javascript
skyTile.tilePosition.x -= player.body.velocity.x / 50;
mountainTile.tilePosition.x -= player.body.velocity.x / 40;
cityTile.tilePosition.x -= player.body.velocity.x / 30;
```

As necessary, adjust the scaling factors to numbers that feel right for your game.

### Option 3: Extended game world with game camera that follows player

```javascript
skyTile.tilePosition.x = game.camera.x * -0.2;
mountainTile.tilePosition.x = game.camera.x * -0.3;
cityTile.tilePosition.x = game.camera.x * -0.4;
```

As necessary, adjust the scaling factors to numbers that feel right for your game.

