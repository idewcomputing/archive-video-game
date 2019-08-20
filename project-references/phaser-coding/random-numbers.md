# Random Numbers

Phaser has built-in methods to generate different types of random numbers \(and to select things at random\). Using these methods is easier than writing your own code to generate specific types of random numbers based on the JavaScript `Math.random()` function.

Random numbers are commonly used in video games to introduce variation that makes the game less predictable. Random numbers are also used to make decisions based on probabilities of certain events happening. Applying random numbers in the right ways can increase the challenge in the game and make repeated playing more enjoyable.

For example, some games place certain resources or obstacles in random or varied locations. Many games use random numbers to create variation in how enemies behave.

Your game doesn't have to use random numbers to be enjoyable, but you should try to think about ways that variation or randomness could improve the gameplay experience.

### Phaser coding references in this section:

* Get Random Position in Game World
* Get Random Member of Group
* Get Random Whole Number Within Range
* Get Random Decimal Number Within Range
* Get Random Angle in Degrees
* Get Random Number Between 0 and 1
* Get Random Number Between -1 and 1
* Get Random Sign \(Positive or Negative\)

### Phaser API Documentation:

[Properties and Methods for Random Data Generator](https://photonstorm.github.io/phaser-ce/Phaser.RandomDataGenerator.html)

## Get Random Position in Game World

You can get a random `x` position and/or random `y` position within your game world boundaries. For example, this could be used to place resources, obstacles, or enemies within your game.

```javascript
var rndX = game.world.randomX;
var rndY = game.world.randomY;
```

For example, to add a `zombie` to a random position within the game world:

```javascript
var zombie = zombieGroup.create(game.world.randomX, game.world.randomY, 'zombie');
```

## Get Random Member of Group

You can get a random existing member of a group of sprites. For example, this could be used to add unpredictability to enemy behavior.

```javascript
var rndSprite = group.getRandomExists();
```

For example, to select a random member of `zombieGroup` to move towards the `player`:

```javascript
var zombie = zombieGroup.getRandomExists();
game.physics.arcade.moveToObject(zombie, player, 150);
```

## Get Random Whole Number Within Range

You can get a random integer \(whole number\) within a range by specifying the minimum and maximum numbers for the range. \(The minimum and maximum values will be included as part of the possible range to randomly select from.\)

```javascript
var rndInteger = game.rnd.integerInRange(min, max);
```

For example, to give each enemy a random velocity between 125 and 175 pixels per second:

```javascript
enemyGroup.forEach(function (enemy) {
    enemy.body.velocity.x = game.rnd.integerInRange(125, 175);
});
```

## Get Random Decimal Number Within Range

You can get a random real number \(decimal number\) within a range by specifying the minimum and maximum numbers for the range. \(The minimum and maximum values will be included as part of the possible range to randomly select from.\)

```javascript
var rndDecimal = game.rnd.realInRange(min, max);
```

For example, to give each `asteroid` a different size by scaling it to a decimal value between 0.5 and 2:

```javascript
asteroidGroup.forEach(function (asteroid) {
    asteroid.scale.setTo(game.rnd.realInRange(0.5, 2));
});
```

## Get Random Angle in Degrees

You can get a random angle between -180 degrees and 180 degrees.

```javascript
var rndAngle = game.rnd.angle();
```

As a reference for angle direction, here are a few examples:

* `0` represents pointing to the right
* `-90` represents pointing up
* `-180` and `180` both represent pointing to the left
* `90` represents pointing down

For example, in a top-down game, this could be used to give each enemy in a group a random angle for its movement.

```javascript
enemyGroup.forEach(function (enemy) {
    enemy.angle = game.rnd.angle();
    game.physics.arcade.velocityFromAngle(enemy.angle, 150, enemy.body.velocity);
});
```

## Get Random Number Between 0 and 1

You can get a random decimal number between 0 and 1 \(which is like getting a random fraction, proportion, or chance\).

```javascript
var rndChance = game.rnd.frac();
```

This is equivalent to using the regular JavaScript random function:

```javascript
var rndChance = Math.random();
```

For example, you could use this to make decisions in the game by calculating the chances of something happening. Imagine that your game is designed so that an enemy's bullet has a 25% chance of damaging the player by 10 points — but will otherwise cause only 5 points of damage.

```javascript
function playerHit(player, bullet) {
    bullet.kill();
    if (game.rnd.frac() < 0.25) player.damage(10);
    else player.damage(5);
    hurtSound.play();
    healthBar.scale.setTo(player.health / player.maxHealth, 1);
}
```

## Get Random Number Between -1 and 1

You can get a random decimal number between -1 and 1.

```javascript
var rndNormal = game.rnd.normal();
```

For example, imagine that you want to introduce a small margin of error in the fire angle of the enemy weapon \(so the enemy is not a perfect shot every time it aims and fires at the player\). Let's say you wanted to add a small random margin of error between -5 and 5 degrees to the enemy's aim.

```javascript
enemy.rotation = game.physics.arcade.angleBetween(enemy, player);
enemyWeapon.fireAngle = enemy.angle + game.rnd.normal() * 5;
enemyWeapon.fire();
```

## Get Random Sign \(Positive or Negative\)

You can get a random sign \(positive or negative\). It returns a result of either -1 or 1. Think of it like a coin flip: heads or tails.

```javascript
var rndSign = game.rnd.sign();
```

For example, in a side-view game, this could be used to give each enemy in a group a random direction for its horizontal velocity: about half of the enemies will randomly get a positive velocity \(moving to right\), and the rest will get a negative velocity \(moving to left\).

```javascript
enemyGroup.forEach(function (enemy) {
    enemy.body.velocity.y = 150 * game.rnd.sign();
});
```

