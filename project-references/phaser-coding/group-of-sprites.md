# Group of Sprites

You can create a **group** of sprites. You can have different groups for different types of sprites. The members of a particular group should be related in some way, such as a group containing enemies, a group containing platforms, etc. The members of a group typically have many similarities — but they can also have differences.

The main advantage of using a group is that is makes it easier to perform checks or actions on each member of the group.

Another advantage is that your code can recycle "dead" members of a group \(which helps save memory and make your game run faster\).

### Phaser coding references in this section:

* Add New Group
* Create New Member in Group
* Set Property for Specific Member of Group
* Set Property for All Members of Group
* Create Multiple Members Using For Loop
  * Position Members at Regular Intervals
  * Position Members at Random Locations
  * Position Members at Specific Locations
* Call Function for Each Member of Group
* Call Function for Each Alive \(or Dead\) Member of Group
* Count Members of Group
* Get Specific Member of Group
* Recycle Dead Members of Group
* Freeze or Hide Group \(and Undo\)
* Kill All Members of Group
* Created Nested Group Containing Other Groups
* Sort Members of Group for Pseudo Depth Effect

### Phaser API Documentation:

[Properties and Methods for Group](https://photonstorm.github.io/phaser-ce/Phaser.Group.html)

## Add New Group

You can add a new group to your game by assigning it to a variable. Use a global variable if you will need to refer to the group or its members in your `update()` function or in a custom function.

For example, to add a group to a global variable named `platformGroup`:

```javascript
platformGroup = game.add.group();
```

This command would be placed in your `create()` function.

## Create New Member in Group

Once a group has been added, you can then create a new member in the group, and add the member to the game world at a specific position.

The members will be created as [sprites](sprites-animations-and-health.md), so be sure to load the image or spritesheet asset for the members in the `preload()` function. Each member of a group can use the same asset, or you can use different assets for different members in a group.

Adding new members to a group is typically done in the game's `create()` function. However, it can also be done in the `update()` function if you need to add new members as the game proceeds.

For example, to create a new member in a group named `platformGroup`:

```javascript
platformGroup.create(200, 500, 'platform-100');
```

* `200, 500` represent the `x` and `y` coordinates where the member will be positioned in the game world \(based on the member's anchor, which is its top-left corner by default\). Change these values to the position you need.
* `'platform-100'` is the asset key of the image or spritesheet to use for this member. Change this to the asset you want to use.

If you need to create more members in the group, just repeat the command, and change the `x, y` position and/or asset key for each new member:

```javascript
// create more platforms in group
platformGroup.create(400, 425, 'platform-100');
platformGroup.create(600, 350, 'platform-100');
platformGroup.create(50, 100, 'platform-50');
platformGroup.create(250, 175, 'platform-50');
platformGroup.create(450, 260, 'platform-50');
platformGroup.create(900, 275, 'platform-200');
platformGroup.create(1150, 475, 'platform-50');
platformGroup.create(1350, 500, 'platform-50');
```

## Set Property for Specific Member of Group

The members of a group are sprites, so each member of a group can have its sprite properties set, can have physics properties enabled and set, and can have animations added.

If you need to change the value of a property for a specific member of the group \(but not the other members\), first create the new member by assigning it to a local variable. Then change the property for that specific member by referring to its variable name.

```javascript
var bottomPlatform = platformGroup.create(0, 575, 'platform-500');
bottomPlatform.scale.setTo(10, 1);
```

* `bottomPlatform` represents a local variable name for the new member. Change this to a variable name that makes sense to you.

The second line of code in the example changes a property \(the `scale` property\) only for this specific member of the group. Change this to list the property and value that you want to set. If necessary, you can modify multiple properties for this specific member of the group.

## Set Property for All Members of Group

You can use the `setAll()` method to change the value of a property for all members of a group. Inside the parentheses, you first list the name of the property \(in quotes\) and then list the value that you want to set for the property.

For example, this command will set a property for all members of a group named `platformGroup`:

```javascript
platformGroup.setAll('body.immovable', true);
```

* `'body.immovable'` represents the name of the property to change. The name should be listed within quotes. Group members are sprites, so list the name of the specific [sprite property](https://photonstorm.github.io/phaser-ce/Phaser.Sprite.html) you want to change.
* `true` represents the value for the property. Some properties use `true` or `false` for their value. Some properties use a number for their value. Some properties use a string \(text\) for their value \(if so, list the string within quotes\). List the value you want to use for your property.

## Create Multiple Members using For Loop

You can use a `for` loop to create multiple new members in a group. If you do this, you have several options for positioning the new members:

* position the members at regular intervals \(such as: every 200 pixels, etc.\)
* position the members at random locations
* position the members at specific locations using JSON data array

### Position Members at Regular Intervals

In order to position the new members at regular intervals in the game world, you need to create a calculation that will space the members horizontally \(in the `x` direction\) and/or vertically \(in the `y` direction\). Typically, these calculations use the `i` variable that the `for` loop uses to count iterations.

For example, this code would create 10 new members in the `coinGroup` located at regular intervals in the game world:

```javascript
for (var i = 0; i < 10; i++) {
    var coin = coinGroup.create(i * 200 + 100, 0, 'coin');
    // add code to set other properties of each member

}
```

The `for` loop shown above will create 10 new members in the group.

* `var i = 0` creates a local variable named `i` that starts at zero and will be used as the "counter" for the number of times to iterate \(repeat\) the loop.
* `i < 10` sets a condition to decide when the loop should stop iterating \(repeating\). In this case, the number `10` controls the number of members that will be created. Change this to the number that you need to create.
* `i++` changes the local variable at the end of each iteration by increasing its value by one \(`i++` is shorthand for `i = i + 1`\). 

The `x` position of each new member will be calculated using `i * 200 + 100`.

* `200` represents how far apart \(in pixels\) the members will be positioned.
* `100` represents the position of the first member.
* As the value of 

  The first new member \(when `i = 0`\) will have an `x` position of 100 \(0  _200 + 100 = 100\). The next member \(when_ `i = 1`_\) will have an_ `x` _position of 300 \(1_  200 + 100 = 300\). The next member \(when `i = 2`\) will have an `x` position of 500 \(2 \* 200 + 100 = 500\) - and so on.

The `y` position of each new member will simply be set as `0` \(top of game world\). Presumably, each `coin` will have a gravity value set that will make it fall down.

Modify the code to use your own calculations \(and/or numbers\) for the `x` and `y` positions.

### Position Members at Random Locations

You can also just position each new member at a random location.

For example, this code would create 10 new members in the `coinGroup` at random locations in the game world:

```javascript
for (var i = 0; i < 10; i++) {
    var coin = coinGroup.create(game.world.randomX, game.world.randomY, 'coin');
    // add code to set other properties of each member

}
```

### Position Members at Specific Locations \(JSON Data\)

You can also create a JSON data array that contains specific `x` and `y` positions for the new members to be created. Then you would use a `for` loop to iterate through the JSON data array to create the new members, and add them into the game world at their specific positions.

For example, here is a JSON data array that list positions that will be used to create new members in a group:

```javascript
// JSON array listing positions
var coinData = [
    { x:75, y:50 },
    { x:150, y:0 },
    { x:250, y:250 },
    { x:275, y:0 },
    { x:350, y:100 },
    { x:450, y:300 },
    { x:475, y:0 },
    { x:525, y:75 },
    { x:650, y:0 },
    { x:700, y:400 }
    // no comma after last item in array
];
```

This code creates a local variable named `coinData` which is an array containing [JSON data](https://www.w3schools.com/js/js_json_syntax.asp) for the members of a group.

Square brackets `[ ]` are used to contain the items in an array. Within the array, the JSON data for each item is listed inside curly braces `{ }` using **name-value pairs**: the name of a variable followed by its value. A colon is used to separate the variable name and its value. If you were using variables that have strings \(text\) for their values, then list the values within quotes.

Within the JSON data for an item, a comma is used to separate different name-value pairs \(so you can list multiple variables for each item\). As you can see, the JSON data list an `x` and `y` variable for each coin.

Commas are also used to separate the different items within the array \(i.e., the comma that appears after the right-hand curly brace\). Notice that you don't include a comma after the last item in the array.

Each item in an array is identified using an index number, which is based on the order in which the items are listed. The first item in an array is always numbered as index `0`. Since there are 10 items listed in the `coinData` array, they are numbered in order as 0-9. For example, `coinData[1]` actually refers to the 2nd item in the array, which is the data: `{ x:150, y:0 }`

The variable values for an item in an array can be referenced by using the item's array index number followed by a period and then the variable name. For example, `coinData[1].x` is `150` and `coinData[1].y` is `0`.

Now you're going to loop through this array to use the JSON data to create the new members in `coinGroup`.

```javascript
for (var i = 0; i < coinData.length; i++) {
    var coin = coinGroup.create(coinData[i].x, coinData[i].y, 'coin');
    // add code to set other properties for each member

}
```

This `for` loop will iterate through the entire `coinData` array, using `i` to represent the index number of the current item: `i` will start at `0` and increase by one after each loop \(`i++`\) until it reaches the end of the array \(which is represented by `coinData.length`\).

Inside the `for` loop a local variable named `coin` is created as a new member in the `coinGroup` using the `x` and `y` values stored in `coinData[i]`. You can add optional code to set other properties for each member \(e.g., set its anchor, add animations, etc.\).

## Call Function for Each Member of Group

You can use the `forEach()` method to call a function for each member of a group. Inside the function, you can list whatever commands you need to perform for each member. The `forEach()` method will iterate through each member of the group, performing the commands you listed.

For example, this would call an inline function for each member of a group named `enemyGroup`:

```javascript
enemyGroup.forEach(function (enemy) {
    // add code to perform for each member
    enemy.anchor.setTo(0.5, 0.5);
    enemy.animations.add('moving', [0, 1, 2], 10, true);
    enemy.animations.play('moving');
    enemy.body.velocity.y = 50;
});
```

* `enemy` represents a local variable name that will be used within the function to refer to a member of the group. Change this to a variable name that makes sense for the group.
* Within the curly braces `{ }`, list one or more commands to be performed for the member. In this example, the code sets the anchor for the member, adds an animation to the member, and starts playing the member's animation. Change these to whatever commands you need.

The `forEach()` method can be used in your `create()` method to set properties of each member before the game starts.

You can also use the `forEach()` method in your `update()` function \(or in a custom function\) to perform commands on each member during gameplay.

## Call Function For Each Alive \(or Dead\) Member of Group

You can use the `forEachAlive()` or `forEachDead()` methods to call a function only for the "alive" \(or "dead"\) members of the group. Inside the function, you can list whatever commands you need to perform for each alive \(or dead\) member.

For example, this would call an inline function only for each "alive" member of a group named `enemyGroup`:

```javascript
enemyGroup.forEachAlive(function (enemy) {
    // add code to perform for each alive member

});
```

For example, this would call an inline function only for each "dead" member of a group named `enemyGroup`:

```javascript
enemyGroup.forEachDead(function (enemy) {
    // add code to perform for each dead member

});
```

These methods would typically be used during gameplay in your `update()` function \(or in a custom function\).

## Count Members of Group

If your game needs to know how many members are in a specific group, there are several options available to get a count.

For example, if the name of your group were `enemyGroup`:

* `enemyGroup.total` would be the total number of members in the group, including both "alive" and "dead" members
* `enemyGroup.countLiving()` will return the number of "alive" members in the group
* `enemyGroup.countDead()` will return the number of "dead" members in the group

## Get Specific Member of Group

If your game needs to get a specific member of a group \(in order to do something with that member\), there are several methods available to select a member.

For example, if the name of your group were `enemyGroup`:

* `enemyGroup.getFirstAlive()` will return the first "alive" member of the group that it finds.
* `enemyGroup.getFirstDead()` will return the first "dead" member of the group that it finds.
* `enemyGroup.getRandomExists()` will return a random existing member of the group.
* `enemyGroup.getClosestTo(player)` will return the member of the group that is closest in distance to the `player` sprite. Change `player` to whichever sprite or display object that you want.
* `enemyGroup.getFurthestFrom(player)` return the member of the group that is farthest in distance from the `player` sprite. Change `player` to whichever sprite or display object that you want.

In order to use of the methods above, you will want to create a local variable to store the result \(the member\) returned by the function. Then you will be able to add commands to do something with that specific member.

For example, the following code will get a random member of the `enemyGroup`, assign the member to a local variable named `enemy`, and the make that `enemy` move towards the `player`:

```javascript
var enemy = enemyGroup.getRandomExists();
if (enemy) {
    // add code to do something with member
    enemy.animations.play('attack');
    game.physics.arcade.moveToObject(enemy, player, 150);
}
```

## Recycle Dead Members of Group

You can recycle "dead" members of a group by adding them back to the game \(making them "alive" again\). By recycling dead members \(instead of constantly creating new members\), your game will use less memory and run faster.

For example, your game might be designed to add back members of an enemy group that have been removed using the `kill()` method. Since the members are sprites, you can add a "dead" member back into the game using the `revive()` or `reset()` method.

To do this, you would need to find the first dead member of the group, and assign this member to a local variable. Then you can revive or reset the member and change any of its properties \(position, velocity, etc.\).

For example, add this code to your `update()` function to **immediately** recycle a dead member of a group named `enemyGroup`:

```javascript
var enemy = enemyGroup.getFirstDead();
if (enemy) {
    // make alive using: enemy.revive() or enemy.reset()
    // OPTIONAL: change other properties of member

}
```

This code tries to find the first dead member of the group, and assigns this member to a local variable called `enemy` \(change to a name that makes sense for your group\).

* If a dead member was found, then it will run the code inside the `if` statement — this is where you would either revive or reset the member to make it alive again. You can add other code to change the member's properties or perform other actions.
* If there weren't any dead members \(i.e., every member of the group is already alive\), then the local variable will be assigned a value of `null` and the code inside the `if` statement will not be performed.

### Check for Dead Member at Random Interval

If you want to add a brief **random interval** before recycling a dead member of the group, use this slightly modified code:

```javascript
if (Math.random() < 0.01) {
    var enemy = enemyGroup.getFirstDead();
    if (enemy) {
        // make alive using: enemy.revive() or enemy.reset()
        // OPTIONAL: change other properties of member

    }
}
```

This modified code will only check for a dead member to recycle if it generates a random number below a certain value \(`0.01` in this case\). Increasing this value will make it more likely that recycling will occur during any specific game loop — the intervals between recycling will still be random, but recycling will occur more frequently on average. \(Decreasing the value has the opposite effect.\)

### Check for Dead Member at Timed Interval

If you want to check for dead members to recycle based on a **timed interval** \(such as every 10 seconds\), then [add a timer event](timers.md) to your game to do this.

## Freeze or Hide Group \(and Undo\)

If you game needs to freeze or hide all members of a group at once, you can use change the `exists` property or the `visible` property.

**NOTE:** The way that the `exists` property affects a group is different from how it works for an individual sprite.

### Freeze Group Using Exists Property

You can change a group's `exists` property prevent its member sprites from being affected by any physics interactions. However, the member sprites will **still be visible** in the game. Basically, it will cause the member sprites to become "frozen" at their current position and become non-interactive.

For example, to "freeze" a group named `enemyGroup`:

```javascript
enemyGroup.exists = false;
```

To unfreeze the group \(and make it interactive again\):

```javascript
enemyGroup.exists = true;
```

### Hide Group using Visible Property

You can change a group's `visible` property to make all of its member sprites invisible. However, the member sprites will still be involved in certain physics interactions \(moving, falling, colliding, etc.\) because the sprites still exist \(they're just invisible\).

For certain games, invisible sprites could be interesting \(e.g., creating invisible enemies that the player must avoid, etc.\).

For example, to make a group named `enemyGroup` invisible:

```javascript
enemyGroup.visible = false;
```

To show the group again:

```javascript
enemyGroup.visible = true;
```

### Freeze and Hide Group at Same Time

You can also change both properties at the same time.

For example, to freeze and hide a group named `enemyGroup`:

```javascript
enemyGroup.exists = false;
enemyGroup.visible = false;
```

To unfreeze and show the group again:

```javascript
enemyGroup.exists = true;
enemyGroup.visible = true;
```

## Kill All Members of Group

If your game needs to kill all members of a group at once, there is a method specifically to do this:

```javascript
enemyGroup.killAll();
```

## Create Nested Group Containing Other Groups

If necessary, you can create a group that contains other groups \(as well as individual sprites\) nested within it. There may be certain situations where using a nested group is helpful.

You can simply use the `add()` method to add another group \(or a sprite\) into a group.

For example:

```javascript
nestedGroup = game.add.group;

// add other groups and sprites
nestedGroup.add(player);
nestedGroup.add(bossEnemy);
nestedGroup.add(enemyGroup);
nestedGroup.add(treeGroup);
```

## Sort Members of Group for Pseudo Depth Effect

Some 2D games attempt to create a "pseudo-3D" visual effect by using **side-view sprites** combined with **top-down movement** \(meaning sprites can move up, down, left, and right — and have no gravity applied\) plus **depth sorting** \(based on `y` position\).

Sprites that are towards the top of the game display \(lower `y` values\) are supposed to be farther back while sprites that are towards the bottom \(higher `y` values\) are supposed to be closer. The game sorts the visual layering of the sprites based on their `y` positions to make "closer" sprites appear in front of "farther" sprites.

Here's an [example of depth sorting](http://phaser.io/examples/v2/groups/depth-sort). In the example, as you move the player \(using the arrow keys\) through the trees, the player's "depth" relative to specific trees will change.

To be clear, using this "depth" effect will not make your game look like a true 3D game. However, the sprites will have a psuedo-depth effect relative to each other, which can add a slight bit of a "third dimension" to your game.

### Sort Individual Group by Depth

For example, to do a depth sort for the members of a group named 'treeGroup\`:

```javascript
treeGroup.sort('y', Phaser.Group.SORT_ASCENDING);
```

If the members of the group do not move, then you could just perform the sort one time by placing it in the `create()` function.

However, if you the members of the group are moving and changing their `y` positions, then you would want to perform the sort every game loop by placing it at the end of your `update()` function. For example, you might need to do this for a group of enemies.

### Sort Multiple Sprites and Groups by Depth

If you want apply a depth sort to the player's sprite plus other sprites and groups, then you should create a nested group that contains all the sprites and groups that should be sorted by depth.

For example, add a new group named `depthGroup` in your `create()` function, and then add the other sprites and groups into the `depthGroup`:

```javascript
depthGroup = game.add.group;

// add any sprites and groups to sort by depth
depthGroup.add(player);
depthGroup.add(bossEnemy);
depthGroup.add(enemyGroup);
depthGroup.add(treeGroup);
```

Then at the end of your `update()` function \(after any sprites or group members may have changed position during a game loop\), do a depth sort on the `depthGroup`:

```javascript
depthGroup.sort('y', Phaser.Group.SORT_ASCENDING);
```

