# Game Display

The **game display** is the HTML `<canvas>` element that Phaser adds to the webpage to display your game. Phaser also refers to this as the Stage.

The game display shows the player's view into your [game world](game-world.md). The game world can be the same size as the game display \(e.g., a single-screen game like _Space Invaders_\) — or the game world can extend beyond the game display \(e.g., a scrolling game like _Crossy Road_\). For extended game worlds, the position of the [game camera](game-camera.md) determines what is shown in the game display.

### Phaser coding references in this section:

* Set Size of Game Display
* Change Background Color of Game Display
* Change Background to Random Color

### Phaser API Documentation:

* [Properties and Methods for Game](https://photonstorm.github.io/phaser-ce/Phaser.Game.html)
* [Properties and Methods for Stage](https://photonstorm.github.io/phaser-ce/Phaser.Stage.html)

## Set Size of Game Display

You set the width and height of your game display when you first create your Phaser.Game object, which is typically done at the beginning of your JavaScript game code — before the `preload()` function.

```javascript
// create Phaser.Game object named "game"
var game = new Phaser.Game(800, 600, Phaser.AUTO, 'my-game',
    { preload: preload, create: create, update: update });
```

In the code above, the game display will be 800 pixels in width by 600 pixels in height. You can change these numbers for your game. However, you'll want the game display to fit within the available width and height of the player's browser window. For beginning game developers, just choose a width and height that fits your own screen.

**REMINDER:** Remember that `'my-game'` represents the id name of your game container `<div>` in your HTML. If your CSS sets a width or height for `#my-game`, then be sure to update those values to match the size of the game display in your JavaScript.

While we won't cover details here, Phaser has a [Scale Manager](https://photonstorm.github.io/phaser-ce/Phaser.ScaleManager.html) that can be used to scale your game display to fit the size and orientation \(landscape vs. portrait\) of the player's device, so your game is playable on a range of device sizes. This is a feature for advanced game developers.

## Change Background Color of Game Display

By default, Phaser uses black as the background color for your game display. However, you can change this to any color using a CSS hex string.

```javascript
game.stage.backgroundColor = '#ffffff';
```

In the code above, `'#ffffff'` represents white — replace this with your own [CSS hex string](https://www.w3schools.com/colors/colors_hexadecimal.asp).

In most cases, you would add this command in your `create()` function \(probably towards the beginning — just to make it easy to find it\). For example, if you only need to change the background color once, then do this in the `create()` function.

However, you can also use this command in your `update()` function or in a custom function. For example, you might do this if you wanted to change the background color based on certain conditions or events in the gameplay.

## Change Background to Random Color

You can also let Phaser choose a random color for the game display background:

```javascript
game.stage.backgroundColor = Phaser.Color.getRandomColor();
```

