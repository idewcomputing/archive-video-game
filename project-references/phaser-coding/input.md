# Input

You can use any keys on the keyboard as inputs for player actions. You can also use mouse or touch input for your game. Phaser even has support for gamepads.

Phaser treats touch input as a pointer — i.e., similar to a mouse \(the position of the touch is treated like the position of a mouse cursor, a tap is treated like a mouse click, a double-tap is treated like a double-click, etc.\).

Phaser does not have built-in properties or methods to directly detect touch gestures such as: swipe left, swipe right, pinch to zoom, rotate, etc. However, Phaser allows you to have up to 10 "pointer" inputs \(i.e., for up to ten fingers\), and Phaser Pointer objects do have certain properties and methods that can be used to **indirectly** detect certain touch gestures \(such as: swipe direction, etc.\).

**NOTE:** This reference section focuses on keyboard and mouse input — and will not cover details for gamepads or complex touch input \(beyond position or taps\).

### Phaser coding references in this section:

* Add Arrow Keys as Inputs
* Add Specific Key as Input
* Check If Key Currently Pressed \(or Not\)
* Check If Key Just Pressed \(or Just Released\)
* Check Multiple Properties for Same Key
* Check If Multiple Keys Pressed at Same Time
* Get Duration of Key Press
* Get Current Position of Pointer \(Mouse or Touch\)
* Check If Pointer Is Down \(Click or Tap\)
* Check Left and Right Mouse Buttons Separately
* Check for Double Click \(or Double Tap\)
* Check Duration Pointer Was Held Down

In addition, the [Physics and Collisions](physics-and-collisions.md) section has coding references for physics methods that involve the Pointer \(mouse or touch\):

* Move Sprite Towards Pointer
* Find Distance from Sprite to Pointer
* Find Angle from Sprite to Pointer

### Phaser API Documentation:

* [Properties and Methods for Input](https://photonstorm.github.io/phaser-ce/Phaser.Input.html)
* [Properties and Methods for Keyboard](https://photonstorm.github.io/phaser-ce/Phaser.Keyboard.html)
* [Properties and Methods for Key](https://photonstorm.github.io/phaser-ce/Phaser.Key.html)
* [List of Phaser Key Codes](https://photonstorm.github.io/phaser-ce/Phaser.KeyCode.html)
* [Properties and Methods for Pointer \(Mouse or Touch\)](https://photonstorm.github.io/phaser-ce/Phaser.Pointer.html)

## Add Arrow Keys as Inputs

Since it is common for games to use the arrow keys for input, Phaser has a command to add all four arrow keys as inputs \(rather than having to add them individually\).

Phaser refers to the arrow keys as cursor keys \(since many computer programs use the arrow keys to move the cursor\).

Add this command in your `create()` function to add the arrow keys as inputs assigned to a global variable named `arrowKey`:

```javascript
arrowKey = game.input.keyboard.createCursorKeys();
```

This command will make all 4 arrow keys \(left, right, up, down\) into inputs assigned to the `arrowKey` variable. The individual keys are identified using the properties `left`, `right`, `up`, and `down` as follows:

* `arrowKey.left`
* `arrowKey.right`
* `arrowKey.up`
* `arrowKey.down`

Even though this command will add all 4 arrow keys as potential inputs, your game doesn't necessarily have to actually use all of them. Your game might only check 2 or 3 of the arrow keys for actual input.

## Add Specific Key as Input

You can add an individual key as input by referencing its Phaser key code.

For example, add this command in your `create()` function to add the spacebar key as an input assigned to a global variable named `fireKey`:

```javascript
fireKey = game.input.keyboard.addKey(Phaser.KeyCode.SPACEBAR);
```

* `SPACEBAR` represents a specific Phaser key code. Change this key code to match the specific key you need.

Phaser key codes are listed in UPPERCASE:

* For letter keys, the keycode is simply the letter \(such as:  A, B, C, etc.\).
* For number keys, the keycode is the name of the number \(such as: ONE, TWO, THREE, etc.\).
* For arrow keys \(if you wanted to add one or more individually\), the keycode is the name of its direction \(such as: LEFT, RIGHT, UP, DOWN\)
* For other keys, the keycode is a name or description \(such as: SPACEBAR, ENTER, SHIFT, TAB, CONTROL, etc.\).

Here is a complete list of [all the available Phaser key codes](https://photonstorm.github.io/phaser-ce/Phaser.KeyCode.html).

## Check If Key Currently Pressed \(or Not\)

Phaser has several properties for detecting input on keys. JavaScript [if-else conditional statements](https://www.w3schools.com/js/js_if_else.asp) are used to check these input properties to see whether they are `true` or `false`.

Keys have two properties that can be used to detect whether or not a specific key is currently being pressed by the player:

* `isDown` will be `true` if the key is currently pressed down — this property will remain true as long as the player continues to hold the key down
* `isUp` will be `true` if the key is currently up \(i.e., not being pressed\) — this property will remain true as long as the player doesn't press the key

At any given moment, a key is either being pressed or it's not. So one of these two properties has to be `true` \(and the other property has to be `false`\). For example, if the key is being pressed, `isDown` is `true` \(which means that `isUp` has to be `false`\).

**TIP:** The `isDown` property is the most commonly-used way to detect input on a key.

For example, here's code that checks for input on global variables named `arrowKey` and `spacebar`:

```javascript
// check arrow keys for input
if (arrowKey.right.isDown) {
    // insert code (such as: move player to right, etc.)

}
else if (arrowKey.left.isDown) {
    // insert code (such as: move player to left, etc.)

}
else {
    // insert code (such as: stop moving player, etc.)
}

// check spacebar for input
if (spacebar.isDown) {
    // insert code (such as: fire player weapon, etc.)

}
```

This code would be placed in the `update()` function \(or in a custom function that is called by the `update()` function every game loop\).

Notice that an if-else statement is used to check input on **both** the right and left arrow keys. This is done because these particular keys are supposed to be mutually exclusive — the game can't move the player both left and right at the exact same time, so `else` is used to help detect three possible conditions: 1. the player is pressing the right arrow key 2. else the player is pressing the left arrow key 3. else the player is not pressing either of them

In the event that the player is actually pressing the right and left arrow keys at the same time \(which will probably happen at some point during gameplay\), the game will act as if the player were only pressing the right arrow key. This is because the if-else statement first checks for input on the right arrow key \(and will skip checking the other conditions once a `true` condition is detected\).

Notice that a separate `if` statement is used to check input on the spacebar. By checking this key with its own `if` statement, the game allows the player to press the spacebar at the same time as one of the arrow keys.

Also notice that we didn't include an `else` statement for the spacebar — though you could if you needed to for your game.

If you have other keys to check, you would add more `if` statements to separately check the input on each key.

If different keys are supposed to be mutually exclusive \(i.e., do opposite things\) in your game, then you would combine their input checks into the same if-else statement.

## Check If Key Just Pressed \(or Just Released\)

In some games, you may want to detect exactly when the player first presses a specific key — or when the player first releases the key that was being pressed.

Remember that your core gameplay occurs in your `update()` function, which loops many times per second. Phaser can detect the exact game loop during which a key was first pressed \(or first released\) by the player.

Keys have two properties that can be used to detect whether the key was just pressed or just released in the current game loop:

* `justDown` will be `true` if the key was just pressed down during the current game loop \(so you can detect exactly when the key is pressed down\) — this property is only true during the specific game loop when the key is first pressed down — after that loop, it becomes false, even if the player keeps pressing the key
* `justUp` will be `true` if the key was just released during the current game loop \(so you can detect exactly when the key is released\) — this property is only true during the specific game loop when the key is first released — after that loop, it becomes false, even if the player still isn't pressing the key

For example, here is code to check input on a global variable named `spacebar`:

```javascript
// check spacebar for input
if (spacebar.justDown) {
    // insert code (such as: fire player weapon, etc.)

}
```

This code would be placed in the `update()` function \(or in a custom function that is called by the `update()` function every game loop\).

### Difference Between isDown and justDown

What would be the difference between checking `spacebar.isDown` versus `spacebar.justDown`?

For example, let's say that the spacebar is used by the player to fire a weapon:

* If your game checks for `spacebar.isDown`, the player can simply hold down the spacebar to keep firing the weapon continuously \(limited only by the weapon's fire rate\).
* If your game checks for `spacebar.justDown`, the weapon will only fire one time if the player holds down the spacebar. In order to fire the weapon again, the player will have to release the spacebar and press it again.

Either input option is perfectly fine — it simply depends on what kind of input style you want your game to use.

## Check Multiple Properties for Same Key

You can also check multiple properties for the same key as part of an if-else statement to do different things based on whether the key was just pressed — versus the key is still being pressed — versus the key was just released — versus the key is still up:

```javascript
// check spacebar for input (all properties)
if (spacebar.justDown) {
    // insert code for when key is first pressed

}
else if (spacebar.isDown) {
    // insert code for when key is still being pressed

}
else if (spacebar.justUp) {
    // insert code for when key is first released

}
else if (spacebar.isUp) {
    // insert code for when key is still not pressed

}
```

This code would be placed in the `update()` function \(or in a custom function that is called by the `update()` function every game loop\).

You don't necessarily have to check all 4 properties for the same key. However, if you are checking multiple properties \(2 or more\) for the same key, then list them in the **order shown above** \(otherwise, your game logic may not work as you expect\) — just remove the property check that you don't need.

## Check If Multiple Keys Pressed at Same Time

If necessary for your game, you can also check whether the player is pressing multiple keys at the same time. For example, maybe your game requires a special combination of keys to be pressed for certain actions to occur.

An if statement can check for multiple keys being pressed together simply by combining multiple conditions using the `&&` logical operator \(which represents AND\).

For example, imagine your game has a power boost feature that allows the player to run at a faster speed when the `boostKey` is pressed at the same time as an `arrowKey`. The code for checking the player input related to movement might look like this:

```javascript
// check input for player movement
if (arrowKey.right.isDown && boostKey.isDown) {
    // insert code (such as: move to right at boost speed, etc.)

}
else if (arrowKey.right.isDown) {
    // insert code (such as: move to right at normal speed, etc.)

}
else if (arrowKey.left.isDown && boostKey.isDown) {
    // insert code (such as: move to left at boost speed, etc.)

}
else if (arrowKey.left.isDown) {
    // insert code (such as: move to left at normal speed, etc.)

}
else {
    // insert code (such as: stop moving, etc.)

}
```

This code would be placed in the `update()` function \(or in a custom function that is called by the `update()` function every game loop\).

## Get Duration of Key Press

Once a key has been released, you can use the `duration` property to get the amount of time \(in milliseconds\) that the key was held down before being released.

This might be useful for a game where the force or magnitude of a player's action depends on how long a key was held down.

If your game needs to check a key's `duration` property, then you would only want to do this after you first detect that the key was just released.

For example, add this code in your `update()` function \(or in a custom function that is called by the `update()` function every game loop\):

```javascript
// check spacebar for input
if (spacebar.justUp) {
    // add code to do something with: spacebar.duration

}
```

Inside the `if` statement, the value of `spacebar.duration` will be the amount of time \(in milliseconds\) that the `spacebar` key was held down before being released. \(Reminder: 1000 ms = 1 second\)

## Get Current Position of Pointer \(Mouse or Touch\)

You can get the current position of the pointer \(mouse or touch\) using:

* `game.input.activePointer.x`
* `game.input.activePointer.y`

Use these positions to do something in your `update()` function or in a custom function.

## Check If Pointer Is Down \(Click or Tap\)

You can check for a mouse click or touch event \(tap\) using the `isDown` property \(similar to checking for a key being pressed\):

For example, add this code in your `update()` function \(or in a custom function that is called by the `update()` function every game loop\):

```javascript
if (game.input.activePointer.isDown) {
    // add code for when click or tap occurs

}
```

## Check Left and Right Mouse Buttons Separately

If your game will only use the left button of the mouse, then just use the code in the previous section to check if the pointer is down.

However, if your game will use both the left and right buttons on the mouse for different actions, then you can check them separately:

```javascript
if (game.input.activePointer.leftButton.isDown) {
    // add code for when left button clicked

}

if (game.input.activePointer.rightButton.isDown) {
    // add code for when right button clicked

}
```

This code would be placed in the `update()` function \(or in a custom function that is called by the `update()` function every game loop\).

## Check for Double Click \(or Double Tap\)

You can check for a double-click \(mouse\) or double-tap \(touch\) by adding an `onTap` signal that will call a custom function whenever a tap \(or click\) occurs. The custom function will be able to detect whether it was a double tap or single tap.

First, add this command to your `create()` function:

```javascript
game.input.onTap.add(checkTap, this);
```

Whenever a tap \(or click\) occurs during the game, a custom function named `checkTap` will be run. \(If desired, you can change the name of the custom function.\)

Next, add the custom function after your `update()` function:

```javascript
function checkTap(pointer, doubleTap) {
    if (doubleTap) {
        // add code for when double click occurs

    }
    else {
        // add code for when single click occurs

    }
}
```

## Check Duration Pointer Was Held Down

You can check the duration that the pointer \(mouse or touch\) was held down by adding an `onUp` signal that will call a custom function whenever the pointer is released. The custom function will be able to use the `duration` property to get the amount of time \(in milliseconds\) that the pointer was held down before being released.

This might be useful for a game where the force or magnitude of a player's action depends on how long the pointer was held down.

First, add this command to your `create()` function:

```javascript
game.input.onUp.add(checkDuration, this);
```

Whenever the pointer is released up, a custom function named `checkDuration` will be run. \(If desired, you can change the name of the custom function.\)

Next, add the custom function after your `update()` function:

```javascript
function checkDuration(pointer) {
    // add code to do something with: pointer.duration

}
```

Inside the custom function, the value of `pointer.duration` will be the amount of time \(in milliseconds\) that the pointer was held down \(clicked or tapped\) before being released. \(Reminder: 1000 ms = 1 second\)

