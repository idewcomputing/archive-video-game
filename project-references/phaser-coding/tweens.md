# Tweens

You can add certain types of animation effects using a **tween**. A tween changes a game object's property from one value to another value over a specified amount of time. Phaser figures out all the "in between" values to make the change occur smoothly over the time duration. Tweens can be used for objects such as text, images, sprites, etc.

For example, a tween could be as simple as making the game's title fade in \(or fade out\). However, a tween can also be more complex. You can use a tween to change multiple properties of an object at the same time, such as moving a sprite while also making it smaller and making it fade out. You can also chain multiple tweens together \(for the same object or multiple objects\) to have them play in sequence.

Here's an example of a [tween animation effect](http://phaser.io/examples/v2/tweens/tween-several-properties) that changes the values of the `y` and `angle` properties for a set of sprites \(each sprite is an image of a letter\). The tweens use delays to start playing in a staggered fashion from left to right.

### Phaser coding references in this section:

* Object Properties Used in Tweens
* Add Tween to Change Object Property
  * To Tween \(Current Value → Different Value\)
  * From Tween \(Different Value → Current Value\)
  * Deciding Which Type of Tween to Use
* Easing Patterns for Tween Effects
* Examples of Tweens
  * Tween Alpha \(Make Object Fade In or Fade Out\)
  * Tween Angle \(Make Object Rotate\)
  * Tween Scale \(Make Object Larger or Smaller\)
  * Tween Position \(Move Object Horizontally or Vertically\)
  * Tween Multiple Properties of Same Object at Same Time
* Chain Tweens Together in Sequence
* Control Playing of Tween \(Start, Pause, Resume, Stop\)
* Detect When Tween Completed

### Phaser API Documentation:

[Properties and Methods for Tween](https://photonstorm.github.io/phaser-ce/Phaser.Tween.html) [Classes and Methods for Phaser Easing Patterns](https://photonstorm.github.io/phaser-ce/Phaser.Easing.html)

## Object Properties Used in Tweens

The most commonly-used object properties used in tweens include:

* `alpha` \(make object fade in or fade out using transparency\)
* `angle` \(make object rotate\)
* `scale` \(make object larger or smaller\)
* `x` position \(make object move left or right\)
* `y` position \(make object move up or down\)

However, any object property can used in tweens, as long as the property can have a range of numeric values.

## Add Tween to Change Object Property

You can add a tween in your `create()` function, `update()` function, or in a custom function. Tweens can be set to start automatically or can be started manually \(which requires assigning them to a variable\).

There are two types of tweens:

* To Tween — changes property from its current value to a new value
* From Tween — changes property from a specified value to its current value

In order to apply a tween to a game object \(such as: text, image, sprite, etc.\), the object **must** be assigned to a variable \(so you can reference the object's variable name in the tween\).

If you want to automatically play the tween, you can add the tween directly \(without assigning the tween to a variable\).

However, if you want to manually control the playing of the tween \(start, pause, etc.\), you must add the tween by assigning it to a variable.

### To Tween \(Current Value → Different Value\)

A "to" tween changes an object's property \(or set of properties\) from its current value **to a different value** over a designated amount of time.

Here is the generic format for a "to" tween:

```javascript
game.add.tween(target).to({ property: value }, duration, easing,
    autostart, delay, repeat, yoyo);
```

* `target` represents the variable name of the object whose property will be changed.
* `{ property: value }` represents the name of the object's property and its new value.
  * Any property that can have a range of numeric values can be tweened.
  * The value can be absolute \(i.e., a specific number such as `300`\) or relative \(i.e., a change such as `'+300'` or `'-300'`\). Relative values need to be listed inside quotes.
  * If needed, you can change multiple properties by separating property-value pairs with a comma: `{ property: value, property: value }`
* `duration` represents the amount of time \(in milliseconds\) over which the property's value should change from its current value to the new value. If no value is listed, it will default to `0` \(no delay\).
* `easing` represents the name of a mathematical pattern that determines how the property's value will be changed over time. If no value is listed, it will default to linear easing \(steady change\).
* `autostart` determines whether the tween will automatically start. List `true` to automatically start the tween, or list `false` to manually start the tween. Tweens that will be manually started must be assigned to a variable. If no value is listed, it will default to `false` \(manual start\).
* `delay` represents the delay \(in milliseconds\) before the tween will start. If no value is listed, it will default to `0` \(no delay\).
* `repeat` represents the number of times that the tween should repeat itself. List the number of times to repeat, or list `-1` to make tween repeat itself continuously. If no value is listed, the default is `0` \(play tween once but don't repeat\). 
* `yoyo` determines whether the tween will reverse itself. List `true` to make the tween play and then reverse itself \(play backwards — returning to the starting value\). If no value is listed, the default is `false` \(play tween once but don't reverse\).

Only the `target` name and the `property: value` pair are required parameters. The other parameters are optional and will use their defaults if omitted.

However, it is recommended to list values for `duration`, `easing`, `autostart`, and `delay`. This will make it easier for you to fine-tune the tween animation effect.

### From Tween \(Different Value → Current Value\)

A "from" tween changes an object's property \(or set of properties\) **from a different value** to its current value over designated amount of time.

Here is the generic format for a "from" tween:

```javascript
game.add.tween(target).from({ property: value }, duration, easing,
    autostart, delay, repeat, yoyo);
```

The parameters are the same as a "to" tween \(see above for descriptions\).

### Deciding Which Type of Tween to Use

How do you decide whether to use a "to" tween versus a "from" tween?

**The short answer:** It may not matter. You can most likely use either type of tween to achieve the same effect.

**The longer answer:** It depends. It might be slightly easier to use one versus the other. Just use whichever one makes more sense to you \(or for your game\).

Possible reasons to use a "to" tween:

* You want the current value of the object's property to be different when the tween effect is completed. \(The tween will start from the current value and change to a different value.\)
* You have a specific ending value in mind for the property.

Possible reasons to use a "from" tween:

* You want the current value of the object's property to be the same when the tween effect is completed. \(The tween will start from a different value and change to the current value.\)
* You have a specific starting value in mind for the property.

Again, in most cases, you could probably use either type of tween. Your code will just be slightly different.

## Easing Patterns for Tween Effects

Phaser has different "easing patterns" that determine how the values of the property are mathematically changed from its starting value to its ending value.

The default easing pattern is to change the value in a linear pattern \(i.e., steady change over time\). In many cases, a linear pattern will work just fine for a tween effect.

However, in other cases, you may want to use a different easing pattern to achieve a more realistic \(or interesting\) animation effect for the tween. For example, a cubic pattern is often used for changes that naturally speed up or slow down.

Here are some of the more commonly-used Phaser easing patterns \(not a complete list\):

* `Phaser.Easing.Default` — the same as `Phaser.Easing.Linear.None`
* `Phaser.Easing.Linear.None` — value changes at steady rate
* `Phaser.Easing.Cubic.In` — value changes slowly at first and then speeds up
* `Phaser.Easing.Cubic.Out` — value changes rapidly at first and then slows down
* `Phaser.Easing.Cubic.InOut` — value changes slowly at first, then speeds up, and then slows down again

Some of the other [classes of Phaser easing patterns](https://photonstorm.github.io/phaser-ce/Phaser.Easing.html) include: back easing, bounce easing, circular easing, elastic easing, sinusoidal easing, etc.

**VISUAL REFERENCE:** Here are graphs that visually represent the different [Phaser Easing patterns](http://lets-gamedev.de/phasereasings/).

* The black line in each graph shows how the property's value will change over time during the tween. \(Time progresses from left to right across the graph.\)
* The red lines represent the starting and ending values for the property. \(The bottom red line represents the starting value. The top red line represents the ending value. The graph shows the value increasing over time, but the pattern still applies if the value is decreasing over time — just imagine flipping the graph vertically.\)
* PowerX easing patterns are no longer included in Phaser.
* Exponential easing patterns are not shown, but they would look similar to the quintic easing patterns.

## Examples of Tweens

### Tween Alpha \(Make Object Fade In or Fade Out\)

For example, to **fade in** the `gameTitle` from a value of `0` \(transparent\) to its current value \(which is `1` unless you previously set it to something else\):

```javascript
game.add.tween(gameTitle).from({alpha: 0}, 2000, Phaser.Easing.Cubic.Out, true, 0);
```

As another example, to **fade out** the `gameTitle` from its current value \(assume `1`\) to a new value of `0` \(completely transparent\):

```javascript
game.add.tween(gameTitle).to({alpha: 0}, 2000, Phaser.Easing.Cubic.Out, true, 0);
```

### Tween Angle \(Make Object Rotate\)

For example, to make the `gameTitle` rotate from a value of `360` degrees to its current value \(which is `0` unless you previously set it to something else\):

```javascript
game.add.tween(gameTitle).from({angle: 360}, 2000, Phaser.Easing.Cubic.Out, true, 0);
```

This will rotate the `gameTitle` 360 degrees counter-clockwise. To make it rotate clockwise, use `-360` as the "from" value.

### Tween Scale \(Make Object Larger or Smaller\)

Scale is a property that has multiple components:

* `scale.x` \(horizontal scale\)
* `scale.y` \(vertical scale\)

Because this property has multiple components, you must use `object.scale` as the target and `{ x: value, y: value }` for the property-value pairs.

This also means that you cannot combine scale and another property into the **same** tween. However, if you want to tween an object's scale and another property at the same time, just create **separate** tweens that play at the same time.

For example, to scale `gameTitle` from a value of `4` to its current value \(which is `1` unless you previously set it to something else\):

```javascript
game.add.tween(gameTitle.scale).from({x: 4, y: 4}, 2000, Phaser.Easing.Cubic.Out, true, 0);
```

To scale `gameTitle` while also making it fade in at the same time, just add separate tweens \(one directly after the other\):

```javascript
game.add.tween(gameTitle.scale).from({x: 4, y: 4}, 2000, Phaser.Easing.Cubic.Out, true, 0);
game.add.tween(gameTitle).from({alpha: 0}, 2000, Phaser.Easing.Cubic.Out, true, 0);
```

As another example, to scale the `gameTitle` from its current value **to a new value** of `0` \(i.e., make it shrink until it disappears\):

```javascript
game.add.tween(gameTitle.scale).to({x: 0, y: 0}, 2000, Phaser.Easing.Cubic.Out, true, 0);
```

### Tween Position \(Make Object Move Horizontally or Vertically\)

An object's position is determined its `x` and `y` properties. You can move an object by tweening its `x` and/or `y` positions.

**NOTE:** Since `x` and `y` are actually two separate properties \(rather than components of the same property\), you can change both `x` and `y` positions with the same tween, if needed.

**NOTE:** If the object is a sprite that has gravity \(`gravity.x` or `gravity.y`\), its gravity will also affect the sprite as you move it with a tween.

For example, to move the `gameTitle` to position `300, 20`:

```javascript
game.add.tween(gameTitle).to({x: 300, y: 20}, 2000, Phaser.Easing.Cubic.InOut, true, 0);
```

If desired, you can use negative values for position in order to move an object off-screen \(or move the object from off-screen to on-screen\).

For example, to move the `gameTitle` vertically from a position off-screen \(`y: -100`\) down to its current `y` position:

```javascript
game.add.tween(gameTitle).from({y: -100}, 2000, Phaser.Easing.Default, true, 0);
```

Remember that values can be absolute \(i.e., a specific number such as `300`\) or relative \(i.e., a change such as `'+300'` or `'-300'`\). Relative values need to be listed inside quotes.

For example, to move the `bossEnemy` horizontally from its current position to a new position 300 pixels to the right:

```javascript
game.add.tween(bossEnemy).to({x: '+300'}, 2000, Phaser.Easing.Cubic.InOut, true, 0);
```

To move the `bossEnemy` horizontally to the left use a negative relative value \(such as: `'-300'`\).

To make the `bossEnemy` repeatedly move horizontally to the left by 300 pixels and back again to its original position, set the tween to repeat continuously and to reverse itself in a yoyo:

```javascript
game.add.tween(bossEnemy).to({x: '-300'}, 2000, Phaser.Easing.Cubic.InOut, true, 0, -1, true);
```

### Tween Multiple Properties of Same Object at Same Time

You can tween multiple properties of the same object at the same time by adding multiple tweens and/or listing multiple property-value pairs in the same tween.

For example, these two tweens for `gameTitle` are added back to back and thus will start at the same time:

```javascript
game.add.tween(gameTitle.scale).from({x: 0, y: 0}, 1500, Phaser.Easing.Default, true, 0);
game.add.tween(gameTitle).from({alpha: 0, angle: -360, y: '-150'}, 1500, Phaser.Easing.Default, true, 0);
```

Together, these two tweens will make the `gameTitle` become larger \(scale from 0 to 1\), fade in \(from 0 to 1\), rotate clockwise \(from -360 to 0\), and move down \(from 150 pixels above its current `y` position\). All four of these effects will start at the same time and occur over the same duration of time.

## Control Playing of Tween \(Start, Pause, Resume, Stop\)

If you assign a tween to a variable and set the tween's autostart parameter to `false`, then you can control the playing of the tween. You will be able to start, pause, resume, and/or stop the tween whenever you want. You'll also be able to replay \(reuse\) a tween if needed.

```javascript
// add tween by assigning to variable such as: tweenName

tweenName.start();
tweenName.pause();
tweenName.resume();
tweenName.stop();
```

First, add the tween by assigning it to a variable. Use any unique variable name that make sense for your game.

Then when needed in your game, you can use any of the methods to start, pause, resume, and/or stop the tween.

**NOTE:** If the tween is being added in one function \(such as the `create()` function\) but will be controlled in a **different** function \(such as the `update()` function or a custom function\), then the tween should be assigned to a global variable. \(Otherwise, if the tween is added and controlled within the same function, a local variable can be used.\)

## Chain Tweens Together in Sequence

You can chain multiple tweens together, so they play in a sequence — when the first tween ends, the next tween will start, etc.

One of the tweens will represent the start of the chain. The other tweens will represent additional "links" in the chain.

```javascript
// add tween1 by assigning to variable
// add tween2 by assigning to variable
// add tween3 by assigning to variable

tween1.chain(tween2, tween3);
tween1.start(); // play chain in order: tween1, tween2, tween3
```

First, add the individual tweens by assigning them to variables. Use any unique variable names that make sense for your game.

Then use the `chain()` method to "link" the tweens in order \(which represents the order in which they will play\).

**NOTE:** If the chain is being created in one function \(such as the `create()` function\) but will be started in a **different** function \(such as the `update()` function or a custom function\), then the first tween in the chain should be assigned to a global variable \(but the other tweens in the chain can just use local variables if the tweens and chain are created within one function\).

**NOTE:** If you want multiple tweens to play at the same time \(in parallel instead of in sequence\), then do not chain them together \(just add them one after another\).

## Detect When Tween Completed

You can add an `onComplete` signal to call a custom function when a particular tween has completed.

First, add the tween by assigning it to a global variable. Use any unique variable name that make sense for your game. Then add an `onComplete` signal to specify the name of the custom function to call. Use any unique function name that make sense for your game.

```javascript
// add tween by assigning to variable such as: tweenName

tweenName.onComplete.add(tweenComplete, this);
```

Finally, add the custom function after your `update()` function. Be sure to use the same name that you listed in the `onComplete` signal.

```javascript
function tweenComplete(){
    // add code to perform when tween completed

}
```

