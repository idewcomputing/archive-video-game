# Enemy Behavior

You can use Phaser code to develop enemy behaviors that will give the enemies "artificial intelligence", so they respond appropriately to certain conditions or events during gameplay. Many games actually use very simple rules to create enemy behaviors that seem "intelligent" to the player.

A very simple \(but still useful\) model of behavior is: **STIMULUS → RESPONSE**

You should decide on a set of a few simple rules that will guide how your enemies should behave. As necessary, restate each rule as a more specific "stimulus-response" statement \(which will act like pseudocode\). What are the specific conditions \(stimulus\) that will lead to a specific enemy behavior \(response\)? The final step is try to translate this into actual code. **Hint:** [Phaser Arcade Physics](physics-and-collisions.md) has properties and methods that are useful when coding enemy behaviors.

Here's an example of how to translate a general rule for enemy behavior into "stimulus-response" pseudocode and then into Phaser code:

**Rule for Enemy Behavior**

* If the player gets close enough to an enemy, the enemy will follow the player.

**Restated as "Stimulus-Response" Pseudocode**

* If the enemy and the player are within a certain distance of each other AND the enemy is moving away from the player \(or the enemy is not currently moving\), make the enemy move towards the player.

**Translated into Possible Phaser Code**

```javascript
// see if enemy and player within 400px of each other
if (game.physics.arcade.distanceBetween(enemy, player) < 400) {

    // if player to left of enemy AND enemy moving to right (or not moving)
    if (player.x < enemy.x && enemy.body.velocity.x >= 0) {
        // move enemy to left
        enemy.body.velocity.x = -150;
    }
    // if player to right of enemy AND enemy moving to left (or not moving)
    else if (player.x > enemy.x && enemy.body.velocity.x <= 0) {
        // move enemy to right
        enemy.body.velocity.x = 150;
    }
}
```

This reference section will supply some examples of common enemy behaviors used in games. However, you may be able to develop other behaviors using your own code \(or by finding other code examples elsewhere online\).

### Phaser coding references in this section:

* Make Enemies Bounce Off Obstacles, Each Other, or the Player
* Make Enemies Patrol Back and Forth on Platforms
* Make Enemies Patrol Back and Forth using Step Counter
* Make Enemies Move Towards Player
* Make Enemies Fire Weapon at Player

## Make Enemies Bounce Off Obstacles, Each Other, or the Player

You can make enemies bounce off obstacles, each other, and/or the player. To do this, you need to set the `bounce` property for the enemies, and then check for collisions \(or overlap\) between the enemies and the other sprites.

### Set Enemy Bounce Property

For example, if the enemies move horizontally \(left or right\), then set their `body.bounce.x` property to a value of `1` when adding each enemy in your `create()` function:

```javascript
enemy.body.bounce.x = 1;
```

Using a value of `1` will ensure the enemy will not lose any velocity when it collides with an object and reverses direction. \(If you want the enemy to slow down after each collision, then use a value less than 1.\)

### Make Enemies Bounce Off Obstacles

In your `create()` function, be sure to set each member of the obstacle group to "immovable":

```javascript
wallGroup.setAll('body.immovable', true);
```

In your `update()` function, add a `collide()` method between the enemy group and the obstacle group:

```javascript
game.physics.arcade.collide(enemyGroup, wallGroup);
```

### Make Enemies Bounce Off Each Other

In your `update()` function, add a `collide()` method between the members of the enemy group:

```javascript
game.physics.arcade.collide(enemyGroup);
```

### Make Enemies Bounce Off Player

In your `update()` function, add an `overlap()` method between the player and the enemy group to run a custom function \(called `touchEnemy` in this example\):

```javascript
game.physics.arcade.overlap(player, enemyGroup, touchEnemy, null, this);
```

Then add the custom function after your `update()` function. Inside the custom function, reverse the enemy's direction by multiplying its velocity by `-1`.

```javascript
function touchEnemy(player, enemy) {
enemy.body.velocity.x *= -1;
// can add other code - damage player, etc.
}
```

You could add other code inside the custom function, such as damaging the player's health, etc.

## Make Enemies Patrol Back and Forth on Platforms

You can make a group of enemies patrol back and forth on platforms. If an enemy is about to move off the edge of a platform, then the enemy should reverse its direction. When the enemy reaches the opposite platform edge, the enemy will reverse again — patrolling back and forth on the platform without falling off.

In order to accomplish this, you need to have your `collide()` method between the enemies and platforms run a custom function \(which will check whether or not each enemy needs to reverse its direction\).

In your `update()` function, your `collide()` method might look like this:

```javascript
game.physics.arcade.collide(enemyGroup, platformGroup, patrolPlatform, null, this);
```

Then you'll need to add the custom function `patrolPlatform()` after your `update()` function.

The custom function will check the enemy's current direction of movement \(right vs. left\) and also check whether the enemy has started to move over the edge of the platform \(by comparing the edge positions of the enemy and platform\). If the enemy is moving off the edge of the platform, the enemy's direction will be reversed \(by multiplying its velocity by -1\).

```javascript
function patrolPlatform(enemy, platform) {
    // if enemy moving to right and has started to move over right edge of platform
    if (enemy.body.velocity.x > 0 && enemy.right > platform.right) {
        enemy.body.velocity.x *= -1; // reverse direction
    }
    // else if enemy moving to left and has started to move over left edge of platform
    else if (enemy.body.velocity.x < 0 && enemy.left < platform.left) {
        enemy.body.velocity.x *= -1; // reverse direction
    }
}
```

## Make Enemies Patrol Back and Forth using Step Counter

You can make a group of enemies patrol back and forth \(whether they are on a platform or not\) simply by counting the number of "steps" they take and reversing each enemy when they reach a certain number \(the step limit\).

Before your `preload()` function, declare a global variable named `stepLimit`, and assign it a value representing the number of "steps" that each enemy can take before reversing direction. Each step actually represents one game loop \(as a general rule of thumb, about 50-60 game loops occur in every 1 second of game time\).

```javascript
// global variables
stepLimit = 100; // number of steps before reversing enemies
```

When adding each enemy in your `create()` function, give the enemy a starting velocity. You can either give each enemy the same velocity or give each enemy a random velocity within a specified range.

For example, this command would give each enemy a random horizontal velocity between 125 and 175 and also randomly select either a positive velocity \(moving to right\) or negative velocity \(moving to left\):

```javascript
enemy.body.velocity.x = game.rnd.integerInRange(125, 175) * game.rnd.sign();
```

When adding each enemy, you also need to give them a new property which we'll call `currentStep`. This is not a built-in Phaser property. However, you can add new properties to an existing JavaScript object \(including a Phaser object\) just by assigning a value to the property. If the object doesn't already have a property with this name, then JavaScript will automatically add the property to the object.

Rather than starting each enemy at a step count of zero, let's make each enemy's starting `stepCount` somewhere between 0 and the `stepLimit`:

```javascript
// start enemy at random step in count
enemy.stepCount = game.rnd.integerInRange(0, stepLimit);
```

Giving each enemy a random starting point for its `stepCount` will help prevent the enemies from all reversing direction at the exact same moment in the game. \(If you do want the enemies to move in unison with each other, instead just set them to the same `stepCount` at the start.\)

Then in your `update()` function, you need to increase each enemy's `stepCount` with each game loop \(i.e., each time the `update()` function runs\). If an enemy reaches the `stepLimit`, then reverse the enemy's direction by multiplying its velocity by `-1` and reset its step counter:

```javascript
enemyGroup.forEachAlive(function (enemy) {
    // increase enemy's step counter
    enemy.stepCount++;
    // check if enemy's step counter has reach limit
    if (enemy.stepCount > stepLimit) {
        // reverse enemy direction
        enemy.body.velocity.x *= -1;
        // reset enemy's step counter
        enemy.stepCount = 0;
        // can add other code - change enemy animation, etc.
    }
});
```

## Make Enemies Move Towards Player

You can make the enemies in your game move towards the player by detecting where the player is located relative to an enemy and then changing the direction of the enemy's movement, so the enemy will move towards the player.

Provided below are examples for a side-view game and for a top-down game.

### SIDE-VIEW GAME

Let's assume you have a side-view game with platforms where the enemies can move either left or right.

Let's also assume that an enemy will only change direction to move towards the player if the player is within a certain distance AND the player and enemy are possibly on the same platform \(i.e., their "bottom" positions are the same\).

For each enemy, you would first need to check whether its bottom position matches the player AND the player is within a specified distance. If that's true, then next you need to detect whether the player is currently located to the left or the right of the enemy \(by comparing their `x` positions\) AND whether the enemy is moving away from the player \(by checking whether its velocity is positive or negative, which tells you direction\). Then when necessary, you'd change the enemy's direction to move towards the player.

Here is example code that could be included in your `update()` function:

```javascript
enemyGroup.forEachAlive(function (enemy) {
    // if bottom positions equal (could be on same platform) AND player within 400px
    if (player.bottom == enemy.bottom && game.physics.arcade.distanceBetween(enemy, player) < 400) {    
        // if player to left of enemy AND enemy moving to right
        if (player.x < enemy.x && enemy.body.velocity.x > 0) {
            // move enemy to left            
            enemy.body.velocity.x *= -1; // reverse direction
            // or could set directly: enemy.body.velocity.x = -150;        
            // could add other code - change enemy animation, make enemy fire weapon, etc.
        }
        // if player to right of enemy AND enemy moving to left
        else if (player.x > enemy.x && enemy.body.velocity.x < 0) {
            // move enemy to right
            enemy.body.velocity.x *= -1; // reverse direction
            // or could set directly: enemy.body.velocity.x = 150;
            // could add other code - change enemy animation, make enemy fire weapon, etc.
        }
    }
});
```

You could revise the code above however you need for your game. For example:

* You could change the distance from 400 pixels to something else.
* You could add other code to change the enemy animation, make the enemy fire a weapon, etc.
* If you don't want to worry about the player and enemy being on the same platform \(or your game doesn't have platforms\), you could remove the condition that compares the `bottom` positions.
* If you don't want to worry about a minimum distance between the player and enemy, you could remove the condition that checks the `distanceBetween()` — or you could even remove the entire outer `if` statement.
* If your enemies are stationary and should only start to move when the player gets close, then remove the condition that checks whether the `enemy.body.velocity.x` is greater than or less than zero. \(Then be sure to directly set the enemy velocity instead of multiplying it by -1.\)
* If your game allows enemies to move up and down, then compare the `y` positions of the player and enemy, in order to decide whether the enemy should move up or down.
* etc.

### TOP-DOWN GAME

Let's assume you have a top-down game where the player and enemy can rotate and move in any direction \(i.e., at any angle\).

Let's also assume that an enemy will only change direction to move towards the player if the player is within a certain distance.

For each enemy, you would first need to check whether the player is within a specified distance. If that's true, then next you can rotate the enemy to face towards the player \(but that's optional, depending on your game\). Then you'd move the enemy towards the player.

Here is example code that could be included in your `update()` function:

```javascript
enemyGroup.forEachAlive(function (enemy) {
    // if player within 400px
    if (game.physics.arcade.distanceBetween(enemy, player) < 400) {
        // rotate enemy to face towards player
        enemy.rotation = game.physics.arcade.angleBetween(enemy, player);
        // move enemy towards player at 150px per second
        game.physics.arcade.velocityFromRotation(enemy.rotation, 150, enemy.body.velocity);
        // could add other code - make enemy fire weapon, etc.
    }
});
```

You could revise the code above however you need for your game. For example:

* You could change the distance from 400 pixels to something else.
* You could change the enemy velocity from 150 pixels/second to something else.
* You could add other code to make the enemy fire a weapon, etc.

## Make Enemies Fire Weapon at Player

The enemies in a group can share a single enemy weapon, rather than needing to create a separate weapon for each enemy.

In your `create()` function, add a weapon for the enemies to share, set any necessary weapon properties \(such as: `fireRate`, `bulletSpeed`, etc.\), and add a function to play a sound effect when the weapon fires:

```javascript
// modify enemy weapon properties as necessary
enemyWeapon = game.add.weapon(5, 'enemy-bullet');
enemyWeapon.fireRate = 250;
enemyWeapon.bulletSpeed = 400;
enemyWeapon.bulletKillType = Phaser.Weapon.KILL_CAMERA_BOUNDS;

enemyWeapon.onFire.add(function() {
    enemyFireSound.play();
});
```

In your `update()` function, you need to select a specific enemy to use the weapon, move the weapon to that enemy's position, adjust the weapon's fire angle \(if necessary\), and then fire the weapon:

```javascript
// add missing code to select specific enemy from group
enemyWeapon.trackSprite(enemy); // give weapon to this enemy
enemyWeapon.fireAngle = 0; // if necessary, change fire angle
enemyWeapon.fire();
```

The code shown above is "generic" code. See below for more details on how to select an enemy, set the fire angle, etc.

### 1. Select Enemy and Give Weapon to Enemy

You need to decide on a rule for which enemy will fire the weapon: a random enemy, the closest enemy, all enemies within a certain distance, etc. Once the specific enemy has been selected, you give the weapon to that enemy using the `trackSprite()` method.

To select a random enemy from the group, and give it the weapon:

```javascript
var enemy = enemyGroup.getRandomExists();
enemyWeapon.trackSprite(enemy); // give weapon to this enemy
```

To select the closest enemy to the player, and give it the weapon:

```javascript
var enemy = enemyGroup.getClosestTo(player);
enemyWeapon.trackSprite(enemy); // give weapon to this enemy
```

To select an enemy within a certain distance of the player, and give it the weapon:

```javascript
enemyGroup.forEachAlive(function (enemy) {
    // if player within 400px
    if (game.physics.arcade.distanceBetween(enemy, player) < 400) {
        enemyWeapon.trackSprite(enemy); // give weapon to this enemy  

    }
});
```

**NOTE:** If you use the `forEachAlive()` function to select nearby enemies to fire the weapon, be aware that the enemy weapon's `fireRate` \(as well as the number of bullets in the enemy's weapon\) will limit how many frequently the weapon is actually fired. The result is that even though multiple enemies might be close to the player, typically just one enemy at a time will fire the weapon \(and that same enemy tends to keep firing until it is no longer close to the player\).

### 2. Adjust Fire Angle and Fire Weapon

For certain games, the enemies always fire in the same direction. For example, in _Space Invaders_, the aliens always fire straight down. If this is how your game works, then you can set the enemy weapon fire angle in the `create()` function — and you won't need to change it during gameplay.

However, in other games, the enemies need to adjust the enemy weapon fire angle during gameplay, in order to fire at the player.

### SIDE-VIEW GAME

Let's assume you have a side-view game where the enemies can fire the weapon either left or right in the direction of the player. You would need to determine whether the player is currently located to the left or the right of the enemy. You could do this simply by comparing their `x` positions. Then you would set the enemy weapon fire angle to the point in the direction of the player.

To make the enemy weapon fire to the left:

```javascript
enemyWeapon.fireAngle = -180;
enemyWeapon.fire();
```

To make the enemy weapon fire to the right:

```javascript
enemyWeapon.fireAngle = 0;
enemyWeapon.fire();
```

If needed, you can set the enemy weapon to other angles \(e.g., to fire up or down, to fire at a non-perpendicular angle, etc.\)

### TOP-DOWN GAME

If the enemy has been rotated to face towards the player, then make the enemy weapon fire in the same direction of the enemy's angle:

```javascript
enemyWeapon.fireAngle = enemy.angle;
enemyWeapon.fire();
```

Otherwise if your game does not rotate the enemy to face the player, then get the angle between the enemy and player, and make the enemy weapon fire at this angle:

```javascript
var angleRad = game.physics.arcade.angleBetween(enemy, player);
enemyWeapon.fireAngle = game.math.radToDeg(angleRad);
enemyWeapon.fire();
```

### Optional: Add Margin of Error to Fire Angle

If you wanted to add small margin of error to the angle \(so the enemy doesn't always have perfect aim\), add this extra line of code after setting the "perfect" angle \(but before firing the weapon\):

```javascript
enemyWeapon.fireAngle += game.rnd.normal() * 5;
```

* `5` represents the maximum amount of error \(in degrees\). In this case, it would add a random error between -5 to +5 degrees \(including the possibility of 0 degrees — i.e., no error\). Change this number to the maximum amount of error you want.

