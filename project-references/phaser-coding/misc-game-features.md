# Misc Game Features

This section explains how to add other miscellaneous game features, such as a start screen, a game over screen, etc.

**IMPORTANT:** You should add your core game mechanics before worrying about adding any of these miscellaneous features.

### Phaser coding references in this section:

* Add Start Screen for Game
* Add Input Signal to Pause Game \(and Resume\)
* Add Game Over Screen
* Add Input Signal to Restart Game

## Add Start Screen for Game

You can add a simple "start screen" to your game. Since your game is a single-state game, this "start screen" will simply display an image and/or text \(such as the game title and maybe some information\). The "start screen" will be hidden once the game is started \(by the player pressing a specific key, etc.\).

**NOTE:** If you create a Phaser game that has multiple states, you would create a separate game state just for the start of the game. Creating a multi-state game is not recommended if you're new to creating games.

### 1. Add Start Screen to Game

You need to decide what to display at the start of the game as your "start screen":

* You could display an image that will cover all \(or some\) of the game screen.
* You could display an image of the game's title \(or you could just display the title as text\).
* You could display some text information \(such as the game's premise, some basic instructions, etc.\).
* You could display a combination of the above — or anything else you might want.

As necessary, find or create any image\(s\) that you need. If you want your "start screen" to completely cover and hide the game \(until the game is actually started\), then create an image that is the same width and height as your game camera view.

In this example, the "start screen" will simply display an image of the game's title and some text explaining how to start the game. The rest of the game world will be visible behind the "start screen." \(Again, if you want to hide the game world at the start, create an image that will completely cover the the game camera view.\)

Before your `preload()` function, declare global variables for any images or text objects that will be displayed as part of your "start screen". Use any unique variable names that make sense for your game.

```javascript
// global variables
var gameTitle, gameStartText;
```

In the example code above, `gameTitle` will be the variable for an image of the game's title, and `gameStartText` will be the variable for some text explaining how the player can start the game.

If you are using any images in your "start screen", be sure to load the images in your `preload()` function.

```javascript
game.load.image('game-title', 'assets/images/game-title.png');
```

Towards the end of your `create()` function \(after adding all the other sprites, images, and text for your game world\), add the image\(s\) and/or text that you want to display as your "start screen".

```javascript
// START SCREEN - add images and/or text to display
gameTitle = game.add.image(400, 300, 'game-title');
gameTitle.anchor.set(0.5, 0.5);

gameStartText = game.add.text(400, 500, 'Click to Start', {fontSize: '30px', fill: '#00ff00'});
gameStartText.anchor.set(0.5, 0.5);
```

As necessary, change the positions to place the images and/or text where you need.

### 2. Prevent Normal Gameplay from Starting

You need to decide how to prevent the normal gameplay from starting \(until the game's start signal has occurred\):

* You could pause the game.
* You could hide the player's character.
* You could prevent the game's inputs from performing normal gameplay actions.
* You could use a combination of the above — or another method that makes sense.

The simplest approach is to just pause the game until the player has started the game. This will "freeze" everything in the game world.

However, for certain games, you could hide the player's character until the game's start signal occurs. However, the other sprites in the game world \(such as enemies, etc.\) can still move around before the game's start signal has occurred. For example, in Practice 2, the _Asteroids 2084_ game simply hides the player's spaceship until the game is started \(which occurs when the player presses the spacebar\).

### Option 1: Pause Game

To pause the game, add this command in your `create()` function \(after the code that adds your "start screen" and the signal that will start the game\):

```javascript
// pause game until start signal occurs
game.paused = true;
```

### Option 2: Hide Player Sprite

If you aren't pausing the game, then you could hide the player's sprite until the game's start signal has occurred.

To hide the player's sprite, add this command in your `create()` function \(after the code that adds the `player` sprite and sets its properties\):

```javascript
// hide player until start signal occurs
player.exists = false;
```

You might also need to modify some of the code in your `update()` function that checks for player input, so the input will only work if `player.exists` is `true`. Otherwise, those inputs might perform actions that should only occur once the game has officially started.

For example, if the code in your `update()` function to check the `fireKey` input looked like this:

```javascript
if (fireKey.isDown) {
    weapon.fire();
}
```

Then you might need to modify that code to include the condition that `player.exists` is `true`, like this:

```javascript
if (player.exists && fireKey.isDown) {
    weapon.fire();
}
```

### 3. Add Signal to Start Game

You need to add a one-time signal that will be used to start the game:

* You could start the game when the player clicks the mouse button.
* You could start the game when the player presses a specific key.
* You could start the game when the player clicks on a specific image or sprite.
* You could set a timer to automatically start the game after a certain amount of time.
* You could use another signal that makes sense for your game.

When the signal occurs, a custom function will be called that will hide the "start screen" and allow the normal gameplay to begin.

Below are several options for signals. Each option adds a signal that will call a custom function named `startGame` \(which you'll add to your game code in Step 4\). You would add the signal in your `create()` function \(after adding the images and text for your "start screen"\).

### Option 1: Start Game When Mouse Clicked

```javascript
// start game by clicking mouse (or tapping finger)
game.input.onDown.addOnce(startGame, this);
```

### Option 2: Start Game When Specific Key Pressed

You can designate a separate input key that will only be used to start the game — or you can use an existing input key that is normally used during gameplay for another action \(e.g., to fire a weapon, to make the player jump, etc.\).

In either case, before your `preload()` function, be sure to declare a global variable for the input key that will be used to start the game, such as:

```javascript
// global variables
var startKey;
```

**To add a separate input key** that will only be used to start the game:

```javascript
// start game by pressing ENTER key
startKey = game.input.keyboard.addKey(Phaser.Keyboard.ENTER);
startKey.onDown.addOnce(startGame, this);
```

**To re-use an existing input key** normally used for another gameplay action \(such as firing a weapon, etc.\), the code is identical except the input key's variable name is its "normal" name:

```javascript
// spacebar will be normally used to fire weapon
fireKey = game.input.keyboard.addKey(Phaser.Keyboard.SPACEBAR);

// spacebar will be used one-time to start game
fireKey.onDown.addOnce(startGame, this);
```

### Option 3: Start Game When Specific Image or Sprite Clicked

You can add a signal to start the game when a specific image or sprite is clicked using the mouse \(or tapped using a finger\).

Before your `preload()` function, be sure to declare a global variable for the image or sprite that will be used to start the game, such as:

```javascript
// global variables
var startButton;
```

In your `preload()` function, be sure to load the image \(or spritesheet\).

```javascript
game.load.image('start-button', 'assets/images/start-button.png');
```

In your `create()` function, add the image \(or sprite\), and enable it to act as an input:

```javascript
startButton = game.add.image(400, 500, 'start-button');
startButton.inputEnabled = true;
startButton.events.onInputDown.addOnce(startGame, this);
```

### Option 4: Start Game When Timer Ends

To start the game automatically when a timer ends, set the timer for the amount of time that you want your "start screen" to be displayed:

```javascript
// start game when timer ends
game.time.events.add(Phaser.Timer.SECOND * 10, startGame, this);
```

### 4. Add Custom Function to Start Game

After your `update()` function, you'll need to add the custom function named `startGame` that will be called by the game's start signal. This custom function will hide the "start screen" and allow the normal gameplay to begin.

Inside the custom function, you will hide each image or text object displayed in your "start screen" by setting its `visible` property to `false`. Be sure to do this for each image or text object that should be hidden once the game is started.

The custom function will also allow the game to start by either unpausing the game or by unhiding the player sprite \(depending on which option you used in Step 2\).

### Option 1: Unpause Game

If you paused the game in your `create()` function:

```javascript
function startGame() {
    // hide images and text used in start screen
    gameTitle.visible = false;
    gameStartText.visible = false;

    // resume game
    game.paused = false;
}
```

### Option 2: Unhide Player Sprite

If you hid the `player` sprite in your `create()` function:

```javascript
function startGame() {
    // hide each image or text used in start screen
    gameTitle.visible = false;
    gameStartText.visible = false;

    // add player into game world
    player.exists = true;
}
```

## Add Input Signal to Pause Game \(and Resume\)

You can pause a Phaser game by setting the game's `paused` property to `true`. Setting the property back to `false` will resume the game.

When the game is paused, the `update()` function is paused, which causes everything in your game world to be frozen in place. However, you can still detect input and run custom functions, which allows you to designate a key to resume the game again.

**NOTE:** If your game displays a timer using `game.time.totalElapsedSeconds()`, be aware that pausing the game does not pause the game's internal clock — so even though your timer display will stop updating while the game is paused, the game time will keep elapsing in the background. Once you resume the game, the timer display will jump ahead to reflect the amount of time that elapsed while the game was paused. To avoid this issue, you'll need to figure out how to modify your timer.

Let's add a key that can be used to pause the game \(and also resume the game\). Each time the key is pressed, we'll toggle `game.paused` between `true` or `false`.

Select a key that will be used as an input to pause \(and resume\) the game. This example will use the `P` key \(for "pause"\). We'll add a signal that will call a custom function whenever this key is pressed down.

This example will also display text on the screen to indicate when the game is paused. However, adding this text is optional.

Before your `preload()` function, declare global variables for the input key and the text \(if you're using the text\):

```javascript
// global variables
var pauseKey, pauseText;
```

In your `create()` function \(towards the end\), add this code:

```javascript
// pause (and resume) game using P key
pauseKey = game.input.keyboard.addKey(Phaser.Keyboard.P);
pauseKey.onDown.add(togglePause, this);

// OPTIONAL - display text when game paused
pauseText = game.add.text(400, 300, 'PAUSED', {fontSize: '40px', fill: '#ff0000'});
pauseText.anchor.set(0.5, 0.5);
pauseText.fixedToCamera = true;
pauseText.visible = false; // hide until game paused
```

If desired, you can leave out `pauseText` — or change its properties \(such as what the text displays, its position, its fill color, etc.\).

Notice that whenever `pauseKey` is pressed down, Phaser will run a custom function named `togglePause`, which you'll need to add to your game code.

After your `update()` function, add the custom function `togglePause()`:

```javascript
function togglePause() {
    if (game.paused == true) {
        game.paused = false; // resume game
        pauseText.visible = false;
    }
    else {
        game.paused = true;
        pauseText.visible = true;
    }
}
```

If you're not using `pauseText` in your game, then remove those lines of code from the custom function.

## Add Game Over Screen

You can add a simple "game over screen" to your game. Since your game is a single-state game, this "game over screen" will simply display text \(but could also display images or other elements\). The "game over screen" will be hidden at the start of the game and will only be displayed when the game detects specific conditions that indicate it is over.

In general, a game can end in two possible ways — either the player "wins" \(e.g., the player completes the challenge, the player reaches the end of a level, etc.\) or the player "loses" \(e.g., the player's character dies, the player runs out of available time, etc.\).

However, in certain games, there isn't really a way to "win" the game — instead, your game performance is measured in some way \(such as: score points, achievements, progress, etc.\). For example, every player will eventually "lose" _Space Invaders_ by running out of lives; however, your _Space Invaders_ gameplay is measured by how high your score is at the end of the game.

**NOTE:** If you create a Phaser game that has multiple states, you could create a separate game state just for the end of the game \(or a level\). Creating a multi-state game is not recommended if you're new to creating games.

### 1. Add Game Over Screen to Game

You need to decide what to display at the end of the game as your "game over screen" — if necessary, you may need to display different things if the player "wins" versus "loses" the game:

* You could display one or more images \(e.g., the game's logo, different images for "win" versus "lose", etc.\).
* You could display text \(e.g., a message such as "You Win" or "Try Again", instructions for how to play again, etc.\).
* You could display a combination of the above — or anything else you might want.

As necessary, find or create any image\(s\) that you want to use in your screen\(s\).

In this example, the "game over screen" will simply display a different image based on whether the player won or lost, as well as some text indicating whether the player won or lost.

Before your `preload()` function, declare global variables for any images or text objects that will be displayed as part of your "game over screen". Use any unique variable names that make sense for your game.

```javascript
// global variables
var gameWinImage, gameLoseImage, gameOverText;
```

In the example code above, `gameWinImage` and `gameLoseImage` will be the variables for the different images that will be displayed based on how the game ended, and `gameOverText` will be the variable for the text indicating whether the player won or lost the game.

If you are using any images in your "game over screen", be sure to load the images in your `preload()` function.

```javascript
game.load.image('game-win', 'assets/images/gameover-win.png');
game.load.image('game-lose', 'assets/images/gameover-lose.png');
```

Towards the end of your `create()` function \(after adding all the other sprites, images, and text for your game world\), add the image\(s\) and/or text that you want to display as your "game over screen".

```javascript
// GAME OVER SCREEN - add images and/or text to display
gameWinImage = game.add.image(400, 300, 'game-win');
gameWinImage.anchor.set(0.5, 0.5);

gameLoseImage = game.add.image(400, 300, 'game-lose');
gameLoseImage.anchor.set(0.5, 0.5);

gameOverText = game.add.text(400, 500, 'Game Over', {fontSize: '30px', fill: '#00ff00'});
gameOverText.anchor.set(0.5, 0.5);
```

As necessary, change the positions to place the images and/or text where you need.

### 2. Hide Game Over Screen at Start of Game

The "game over screen" needs to be hidden when the game starts.

In your `create()` function \(immediately after adding the elements for your "game over screen"\), hide each element in your "game over screen" by setting its `visible` property to `false`:

```javascript
// hide game over screen at start
gameWinImage.visible = false;
gameLoseImage.visible = false;
gameOverText.visible = false;
```

### 3. Detect Condition\(s\) that Cause Game to End

You need to detect the specific conditions that cause the game to be over. If your game can end in multiple ways \(such as "winning" or "losing"\), then you'll need to detect each of these possible conditions.

For example, a side-scrolling game could be designed to be "won" if the player reaches the end of the level — or "lost" if the player runs out of lives \(or time\).

When a condition occurs that causes the game to end, you will call a custom function named `gameOver()` to display the "game over screen".

Some common examples of "lose" conditions and "win" conditions will be provided here. You can mix and match these with each other — and with your own conditions — based on what's needed for your game design.

### Declare Variable to Track Whether Game Won or Lost

If your game could end with the player either "winning" or "losing" the game, then it will help to have a variable to track the game's final result.

Before your `preload()` function, declare a global variable to represent whether or not the player has won the game. Assign an initial value of `false` to the variable \(because the game hasn't been won yet at the start\).

```javascript
// global variables
var gameWon = false; // set to false for start of game
```

Later, if the player does win the game, the variable will be changed to `true`.

### Lose Condition: Detect When Player Killed

In some games, the player has **only one life**: if the player's character dies, the game is over.

In this example code, when the player is killed, the game will call a custom function named `gameOver()`:

```javascript
player.events.onKilled.add(function() {
    gameOver();
});
```

The example code above would be placed in the `create()` function after the code that adds the `player` sprite.

### Lose Condition: Detect When Player Out of Lives

In some games, the player can have **multiple lives**: when the player's character dies, the game continues if the player has any lives remaining — otherwise, the game is over.

In this example code, when the player is killed, the game will check to see if the player has any lives remaining. If so, the player is reset back into the game world with full health. Otherwise \(when the player is out of lives\), a custom function named `gameOver()` is called:

```javascript
player.events.onKilled.add(function() {
    playerLives--; // subtract one life
    if (playerLives > 0) {
        // add player back into game world
        player.reset(50, 400, player.maxHealth);
        livesText.text = 'Lives ' + playerLives;
        healthText.text = 'Health ' + player.health;
    }
    else gameOver();
});
```

The example code above would be placed in the `create()` function after the code that adds the `player` sprite.

### Lose Condition: Detect When Player Out of Time

In some games, there is a limited amount of time for the player to complete the game's level.

You can [create a countdown timer](timers.md) for your game that displays the amount of time remaining \(based on a time limit that you specify\).

In this example code, if time limit is not over, it displays the time remaining. Otherwise \(when the time runs out\), a custom function named `gameOver()` is called:

```javascript
if (timeOver == false) displayTimeRemaining();
else gameOver();
```

The example code above would be placed in the `update()` function. Be sure to add the other code necessary to [create the countdown timer](timers.md).

### Win Condition: Detect When Player Reaches Specific Location

In some games, the player wins by reaching the end of the level. A simple way to detect this is to compare the position of the player against a specific position \(`x` and/or `y`\) in the game world.

For example, this code will detect when the `player` sprite has reached an `x` position of 4950 pixels \(which might represent the "end" of a game level that is 5000 pixels in width\):

```javascript
if (player.x > 4950) {
    gameWon = true;
    gameOver();
}
```

The example code above would be placed in the `update()` function.

### Win Condition: Detect When Player Reaches Specific Object

In some games, the player wins by reaching a specific object in the game world \(e.g., a treasure, an exit door, another character, etc.\). A simple way to detect this is to check for overlap between the `player` sprite and the object's sprite.

For example, in your `update()` function, check for the overlap of the player and the target object \(the `lostCat` sprite in this example\):

```javascript
game.physics.arcade.overlap(player, lostCat, rescueCat, null, this);
```

Then after your `update()` function, add the custom function called by the overlap method:

```javascript
function rescueCat(player, cat) {
    // can add other code - play sound, increase score etc.
    gameWon = true;
    gameOver();
}
```

### Win Condition: Detect When Player Defeats Specific Enemy

In some games, the player wins by defeating a specific enemy, such as a boss enemy.

In this example code, when the `bossEnemy` is killed, the game will call a custom function named `gameOver()`:

```javascript
bossEnemy.events.onKilled.add(function() {
    // can add other code - play sound, increase score etc.
    gameWon = true;
    gameOver();
});
```

The example code above would be placed in the `create()` function after the code that adds the `bossEnemy` sprite.

### Win Condition: Detect When Player Defeats All Enemies in Group

In some games, the player wins by defeating all the enemies in a group.

In this example code, when there are no more living members left in the `enemyGroup`, the game will call a custom function named `gameOver()`:

```javascript
if (enemyGroup.countLiving() == 0) {
    // can add other code - play sound, increase score etc.
    gameWon = true;
    gameOver();
});
```

The example code above would be placed in the `update()` function.

### Win Condition: Detect When Player Collects All Objects in Group

In some games, the player wins by collecting all the objects in a group \(such as a group of coins, etc.\).

In this example code, when there are no more living members left in the `coinGroup`, the game will call a custom function named `gameOver()`:

```javascript
if (coinGroup.countLiving() == 0) {
    // can add other code - play sound, increase score etc.
    gameWon = true;
    gameOver();
});
```

The example code above would be placed in the `update()` function.

### 4. Add Custom Function to Display Game Over Screen

After your `update()` function, you'll need to add the custom function named `gameOver()` that will be called when a "game over" condition is detected. The custom function will show the "game over screen" \(and might need to cause the normal gameplay to end\).

Inside the custom function, you will show each image or text object displayed in your "game over screen" by setting its `visible` property to `true`. Be sure to do this for each image or text object that should be shown once the game ends. You can also add other code to change the text, play a sound, etc.

```javascript
function gameOver() {
    // show game over screen
    if (gameWon == true) {
        gameWinImage.visible = true;
        gameOverText.text = 'You Win';
        // could add other code - play win sound, etc.
    }
    else {
        gameLoseImage.visible = true;
        gameOverText.text = 'You Lose';
        // could add other code - play lose sound, etc.    
    }
    gameOverText.visible = true;
    // could add other code - set high score, pause game, etc.
    // could add signal to restart new game
}
```

**NOTE:** You may want \(or need\) to add other code to prevent the normal gameplay from continuing once the game is supposed to be over. Here are two simple options \(but there are other possible options\):

* Pause the gameplay by using: `game.paused = true;`
* Remove the player by using: `player.exists = false;`

## Add Input Signal to Restart Game

You can add an input signal to allow the player to restart the game \(i.e., start a new game\) when the current game is over. Often this is used in combination with displaying a "game over screen".

A Phaser game can be restarted by using:

```javascript
// start new game
game.state.restart();
```

The instructions below will show how to embed the "restart" command within a custom function that is called by a signal within your game.

### 1. Add Signal to Restart Game

In your `gameOver()` function, you would add a one-time signal that will restart the game:

* You could restart the game when the player clicks the mouse button.
* You could restart the game when the player presses a specific key.
* You could restart the game when the player clicks on a specific image or sprite.
* You could set a timer to automatically restart the game after a certain amount of time.
* You could use another signal \(or combination of signals\) that makes sense for your game \(e.g., restart when specific enemy defeated, etc.\).

When the signal occurs, a custom function will be called that will restart the game.

Below are several options for signals. Each option adds a signal that will call a custom function named `restartGame` \(which you'll add to your game code in Step 2\).

### Option 1: Restart Game When Mouse Clicked

In your `gameOver()` function, add the signal to restart the game:

```javascript
// restart game by clicking mouse (or tapping finger)
game.input.onDown.addOnce(restartGame, this);
```

### Option 2: Restart Game When Specific Key Pressed

You can designate a separate input key that will only be used to start the game — or you can use an existing input key that is normally used during gameplay for another action \(e.g., to fire a weapon, to make the player jump, etc.\).

In either case, before your `preload()` function, be sure to declare a global variable for the input key that will be used to start the game, such as:

```javascript
// global variables
var startKey;
```

**To add a separate input key** that will only be used to start or restart the game, first add the input key in your `create()` function:

```javascript
// start or restart game by pressing ENTER key
startKey = game.input.keyboard.addKey(Phaser.Keyboard.ENTER);
```

Then add the signal in your `gameOver()` function:

```javascript
startKey.onDown.addOnce(restartGame, this);
```

**To re-use an existing input key** normally used for another gameplay action \(such as firing a weapon, etc.\), first, add the input key in your `create()` function:

```javascript
// spacebar will be normally used to fire weapon
fireKey = game.input.keyboard.addKey(Phaser.Keyboard.SPACEBAR);
```

Then add the signal in your `gameOver()` function:

```javascript
// spacebar will be used one-time to restart game
fireKey.onDown.addOnce(restartGame, this);
```

**To add a dedicated key that can restart the game at any point during gameplay**, use a separate input key that's not used for any other purpose, change the signal from `addOnce()` \(one-time signal\) to `add()` \(repeatable signal\), and add the signal in your `create()` function \(instead of in the `gameOver()` function\):

```javascript
// restart game at any time by pressing R key
restartKey = game.input.keyboard.addKey(Phaser.Keyboard.R);
restartKey.onDown.add(restartGame, this);
```

### Option 3: Restart Game When Specific Image or Sprite Clicked

You can add a signal to restart the game when a specific image or sprite is clicked using the mouse \(or tapped using a finger\).

Before your `preload()` function, be sure to declare a global variable for the image or sprite that will be used to start the game, such as:

```javascript
// global variables
var startButton;
```

In your `preload()` function, be sure to load the image \(or spritesheet\).

```javascript
game.load.image('start-button', 'assets/images/start-button.png');
```

In your `create()` function, add the image \(or sprite, enable it to act as an input, and hide the image \(until the game is over\):

```javascript
startButton = game.add.image(400, 500, 'start-button');
startButton.inputEnabled = true;
startButton.visible = false;
```

In your `gameOver()` function, show the image, and add the signal to it:

```javascript
startButton.visible = true;
startButton.events.onInputDown.addOnce(restartGame, this);
```

### Option 4: Restart Game Automatically Using Timer

To restart the game automatically after a timer delay, set a timer for the amount of time that you want your "game over screen" to be displayed.

In your `gameOver()` function, add the timer:

```javascript
// restart game when timer ends
game.time.events.add(Phaser.Timer.SECOND * 5, restartGame, this);
```

### 2. Add Custom Function to Restart Game

After your `update()` function, you'll need to add the custom function named `restartGame` that will be called by the signal you added. This custom function will use the `game.state.restart()` method to restart the current game.

Restarting the game skips the `preload()` function \(because all the game assets should already be loaded into memory from the previous game\). Instead, it first clears out the current game world. Then it runs the `create()` function again one-time \(to rebuild a fresh game world\). After that, the `update()` function begins to runs in a continuous loop for the new gameplay.

**IMPORTANT:** If you have any custom game variables \(e.g., to store the player's score, etc.\), those variables will keep the same values they had at the end of the previous game. For some variables, that's perfectly fine. However, for other variables \(such as the player's current score, etc.\), you may need to reset the values of those variables for the new game before restarting the game. If desired, you can also change the values of certain variables \(such as updating the game's high score, increasing the enemy count for the next game, etc.\).

Inside the custom function, change or reset the values of certain game variables \(as necessary\) for the start of a new game. Then restart the game:

```javascript
function restartGame() {
    // optional: change or reset values of certain variables for new game
    if (score > highScore) highScore = score;
    score = 0;
    playerLives = 3;

    // start new game
    game.state.restart();
}
```

