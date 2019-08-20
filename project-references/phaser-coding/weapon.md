# Weapon

You can use Arcade Physics to add a weapon to your game that fires bullets. The weapon is not represented visually by an actual weapon \(such as a gun, etc.\). The "weapon" is really just a group of "bullets" ready to be fired.

The "bullets" can be any visual object you want — bullets, laser beams, arrows, fireballs, snowballs, hearts, etc. It simply depends on the image or spritesheet that you use for the bullets.

When you fire a weapon, a bullet appears at the weapon's position and travels outward at an angle and speed determined by the weapon's properties \(which you can set\). When the bullet hits another object, you can perform specific actions \(such as damaging or killing the object, etc.\).

**RECOMMENDATION:** If you only think of a "weapon" as a literal weapon with bullets \(such as a gun\), you'll be overlooking interesting ways to use a Phaser weapon in your game. Think more broadly about objects that can be thrown, shot, or directed at a target.

* In the game Angry Birds, the "bullets" are birds that you aim and shoot using a slingshot.
* In a basketball game, the basketball is a "bullet" that is shot towards the goal. You can add gravity and rotation to make your basketball "bullet" act realistically.

### Phaser coding references in this section:

* Load Image or Spritesheet for Bullets
* Add Weapon
* Set Weapon Properties
  * Auto Fire
  * Fire Angle
  * Fire Rate
  * Fire Limit
  * Bullet Speed
  * Bullet Kill Type \(Remove Bullet\)
  * Bullet Kill Distance
  * Bullet Kill Lifespan
  * Bullet Gravity
  * Bullet Rotate to Match Direction
* Make Weapon Track Sprite
* Fire Weapon
* Call Function When Weapon Fired Successfully
* Check for Bullets Hitting Objects
* Change Collision Area of Bullets
* Display Visual Weapon

### Phaser API Documentation:

[Properties and Methods for Arcade Physics Weapon](https://photonstorm.github.io/phaser-ce/Phaser.Weapon.html)

## Load Image or Spritesheet for Bullets

The bullets for a weapon can use an image or a spritesheet \(e.g., for animated bullets\).

The image or spritesheet file for your weapon's bullets must be loaded into the game's memory before you can add the weapon to your game.

```javascript
game.load.image('laser', 'assets/images/laser.png');
```

The command to load an asset is always placed in your `preload()` function.

* `'laser'` represents an asset key — a key is sort of like a variable name. You decide what string to use for the key, as long as each image asset has a unique key.
* `'assets/images/laser.png'` represents the folder path and filename of the image to load. Change this to match your specific folder path and filename.

## Add Weapon

Once the asset for the bullets has been loaded into memory, you can add the weapon to your game.

You will need to declare a global variable for the weapon and assign the weapon to the variable in your `create()` function. Typically you will want to add the weapon before adding the sprite or group that will use the weapon \(so that the weapon bullets will appear from behind the sprite using the weapon\).

For example, to add a weapon to the variable named `laser`:

```javascript
laser = game.add.weapon(5, 'laser');
```

* `5` represents the number of bullets to create for this weapon. Bullets are automatically recycled as they are fired and removed, so this represents the maximum number that could be on-screen in the game at any one time. Change this number to what you need.
* `'laser'` is the asset key for the bullets. Change this to match your asset key name.

**NOTE:** There is no visual indication that a weapon has been added to the game. When the weapon is fired, a bullet will appear and begin to travel — but otherwise, there is no actual "weapon" displayed.

## Set Weapon Properties

This sub-section describes the most common weapon properties that you might use in your game.

These weapon properties are typically set in your `create()` function after adding the weapon. However, sometimes certain properties are adjusted during gameplay \(such as fire angle, etc.\).

### Auto Fire

If you want a weapon to automatically fire itself \(limited only by its `fireRate` and `fireLimit`\), then set its `autofire` property to `true`.

For example, to set the `laser` weapon to autofire:

```javascript
laser.autofire = true;
```

If you don't set a weapon to autofire, then be sure to add an input that the player can use to manually fire the weapon.

### Fire Angle

You can use the `fireAngle` property to set the angle \(in degrees\) at which bullets will be fired.

For example, to set the fire angle for the `laser` weapon:

```javascript
laser.fireAngle = 0;
```

Change the angle to any value from -180 to 180. Negative values rotate the fire angle counterclockwise. Positive values rotate the fire angle clockwise.

Here are some commonly used values for the fire angle:

* `0` represents pointing right
* `-90` represents pointing up
* `180` represents pointing left
* `90` represents pointing down

If the sprite using the weapon will rotate during gameplay, you use the `trackSprite()` method to automatically rotate the weapon's fire angle to match the sprite's rotation.

### Fire Rate

You can use the `fireRate` property to set a limit on how frequently the weapon can be fired. The rate represents the minimum amount of time \(in milliseconds\) between fires.

For example, to set a fire rate for the `laser` weapon:

```javascript
laser.fireRate = 250;
```

In this example, the `laser` can only be fired once every 250 milliseconds, even if the player is pressing the "fire key" faster than that.

### Fire Limit

You can use the `fireLimit` property to set a limit on the total number of bullets that can be fired during the game. Once the limit is reached, the weapon won't fire any more \(unless you reset it\).

By default, a weapon has no fire limit \(i.e., has unlimited bullets\), unless you specifically change it.

For example, to set a fire limit for the `laser` weapon:

```javascript
laser.fireLimit = 10;
```

### Get Count of Bullets Fired

The `shots` property keeps track of how many total bullets have been fired from the weapon. When the value of `shots` reaches the `fireLimit`, the weapon cannot be fired anymore.

For example, this code would calculate the number of shots remaining by subtracting the number of shots fired from the fire limit:

```javascript
var shotsRemain = laser.fireLimit - laser.shots;
shotRemainText.text = 'Shots Left ' + shotsRemain;
```

### Reset Bullet Count

Once the fire limit has been reached, you have to reset the `shots` counter back to zero to allow the weapon to be fired again. Once reset, the fire limit is still in effect \(so the weapon will eventually "run out" of bullets again\).

You can add a function that will be called whenever a weapon's fire limit has been reached. The most common use for this is to reset the bullet count after subtracting from the player's inventory of saved bullets \("ammo packs"\).

For example, this code will check to see if the player has any more bullets in inventory to "load" into the weapon. If so, it will reset the weapon to fire again.

```javascript
laser.onFireLimit.add(function() {
    if (laserAmmo > 0) {
        laserAmmo--; // subtract ammo pack
        laser.resetShots();
        ammoText.text = 'Laser Packs ' + laserAmmo;
    }
});
```

### Bullet Speed

You can use the `bulletSpeed` property to set the speed at which the bullets travel \(in pixels per second\).

For example, to set the bullet speed for the `laser` weapon:

```javascript
laser.bulletSpeed = 500;
```

### Bullet Kill Type \(Remove Bullet\)

You can set the `bulletKillType` property to automatically remove bullets under certain conditions. Here are the most commonly-used values for this property:

* `Phaser.Weapon.KILL_WORLD_BOUNDS` is the **default value** unless you change it. It automatically removes a bullet if it leaves the game world boundaries.
* `Phaser.Weapon.KILL_CAMERA_BOUNDS` automatically removes a bullet if it leaves the game camera boundaries.
* `Phaser.Weapon.KILL_DISTANCE` automatically removes a bullet after it has traveled a certain distance, which is determined by the `bulletDistance` property.
* `Phaser.Weapon.KILL_LIFESPAN` automatically removes a bullet after it has traveled for a certain amount of time, which is determined by the `bulletLifespan` property.

For example, to automatically remove the `laser` weapon bullets if they leave the game camera:

```javascript
laser.bulletKillType = Phaser.Weapon.KILL_CAMERA_BOUNDS;
```

**NOTE:** In addition to setting the `bulletKillType` property, you will still need to manually remove bullets \(using the `kill()` method\) when they hit other objects.

### Bullet Kill Distance

If you set the `bulletKillType` property to `Phaser.Weapon.KILL_DISTANCE`, then you need to set a distance \(in pixels\) for the `bulletKillDistance` property.

For example, to automatically remove the `laser` weapon bullets after they have traveled 500 pixels:

```javascript
laser.bulletKillDistance = 500;
```

### Bullet Lifespan

If you set the `bulletKillType` property to `Phaser.Weapon.KILL_LIFESPAN`, then you need to set an amount of time \(in milliseconds\) for the `bulletLifespan` property \(1000 ms = 1 second\).

For example, to automatically remove the `laser` weapon bullets after they have traveled for 2 seconds:

```javascript
laser.bulletLifespan = 2000;
```

### Bullet Gravity

You can set the `bulletGravity` property to make the bullets respond to gravity.

By default, weapon bullets will not be affected by gravity and will travel in a straight line \(based on its fire angle\).

You can set the bullets' gravity value for the horizontal direction \(`x`\) and/or the vertical direction \(`y`\). The value represents the acceleration due to gravity measured in pixels per second squared. By default, a bullet's gravity is set to zero in each direction, unless you specifically change it.

* `bulletGravity.x` — change this value to add gravity in the horizontal direction. Positive values will pull the bullet to the right, and negative values will pull the bullet to the left.
* `bulletGravity.y` — change this value to add gravity in the vertical direction. Positive values will pull the bullet downwards, and negative values will pull the bullet upwards.

Adding a `bulletGravity.y` works well for "bullets" that are shot or thrown up into the air at an angle, such as arrows, a basketball, etc.

For example, to make the bullets from a `bowArrow` weapon fall downward:

```javascript
bowArrow.bulletGravity.y = 100;
```

### Bullet Rotate to Match Direction

You can use the `bulletRotateVelocity` property to make the bullets rotate to match the direction of their travel. This can create the effect of a bullet "pointing" to the path it is following \(such as: an arrow being fired from a bow, etc.\). This works best when you have set a `bulletGravity` value.

For example, to make the bullets from a `bowArrow` weapon rotate to align with the direction of their travel:

```javascript
bowArrow.bulletRotateVelocity = true;
```

## Make Weapon Track Sprite

You can make the position of the weapon automatically track the position of the sprite using the weapon.

For example, to make a weapon named `laser` track the position of the `player` sprite:

```javascript
laser.trackSprite(player, 0, 0, true);
```

* `player` represents the name of the sprite that the weapon will track. Change this to match your sprite.
* `0, 0` represents an x-y offset from the sprite's anchor position. Using `0, 0` means the weapon will fire from the player sprite's anchor. Change these numbers as necessary to position the firing point of the weapon.
* `true` means that the weapon's fire angle should track the sprite's rotation. If the sprite will rotate in the game, you should list this as `true`. Otherwise, list `false` and adjust the fire angle using your own code.

Include this command in your `create()` function after adding the weapon and the sprite to be tracked. Phaser will automatically update the weapon's position during gameplay.

### Change Which Sprite the Weapon Tracks

For a group of enemies, it is common to create a single enemy weapon that they all can share. Then during gameplay, specific members of the group are selected to use the weapon. A specific enemy is selected from the group, and then the enemy weapon is moved to that enemy's position and fired. Then the process can be repeated.

If you want to change which sprite the weapon is tracking, then you can use the `trackSprite()` method in your `update()` function \(or in a custom function called by the `update()` function\).

For example, this code could be included in your `update()` function to continuously select the closest enemy to the player and make that enemy fire a weapon:

```javascript
var closestEnemy = game.physics.arcade.closest(player, enemyGroup);
enemyWeapon.trackSprite(closestEnemy, 0, 0);
enemyWeapon.fire();
```

## Fire Weapon

The `fire()` method is used to fire a bullet from the weapon. The bullet speed, direction \(fire angle\), fire rate, etc. depend on the values set for the weapon's properties.

For example, to fire a weapon named `laser`:

```javascript
laser.fire();
```

### Adjust Fire Angle to Match Sprite's Direction

You may need to adjust the direction that a bullet will be fired by changing the fire angle based on the direction that the sprite using the weapon is moving \(or was last moving\).

For example, this code would move the `player` right or left and also change the fire angle of the `laser` weapon to match the player's direction:

```javascript
if (arrowKey.right.isDown) {
    // move player to right, and set fire angle to right
    player.body.velocity.x = 200;
    laser.fireAngle = 0;
}
else if (arrowKey.left.isDown) {
    // move player to left, and set fire angle to left
    player.body.velocity.x = -200;
    laser.fireAngle = 180;
}
else {
    // stop player (but don't change fire angle)
    player.body.velocity.x = 0;
}

if (spacebar.isDown) {
    laser.fire();
}
```

As another example, this code will detect the current horizontal direction that the `enemy` is moving and adjust its fire angle to match its direction before firing its weapon:

```javascript
if (enemy.body.velocity > 0) enemyLaser.fireAngle = 0;
else enemyLaser.fireAngle = 180;
enemyLaser.fire();
```

## Call Function When Weapon Fired Successfully

You can add a function that will be called whenever a weapon's bullet is successfully fired. The most common use for this is to play a sound effect representing the weapon firing.

For example, this will play `laserSound` whenever the `laser` weapon is fired:

```javascript
laser.onFire.add(function() {
        laserSound.play();
});
```

This code would be placed in your `create()` function after the code that adds the weapon.

The reason for using the `onFire` signal to play the weapon sound effect is that the firing of the weapon is limited by its `fireRate` \(and potentially also by a `fireLimit`\).

If your code played the sound whenever the player pressed the "fire key", the sound would play even if a bullet was not&lt;/&gt; actually fired \(because the weapon was being limited by its `fireRate` or `fireLimit`\). Using the `onFire` signal to play the sound will keep the sound effect synced with the actual weapon firing.

## Check for Bullets Hitting Objects

You can use the Arcade Physics `overlap()` method to detect when a weapon's bullet hits another object. \(You can also use the `collide()` method if you prefer.\)

For the purposes of the `overlap()` method, the weapon bullets are treated as a group referred to as `weapon.bullets` \(replace `weapon` with the specific variable name of your weapon\).

For example, to detect when one of the player's bullets hits a members of the `enemyGroup`:

```javascript
game.physics.arcade.overlap(enemyGroup, laser.bullets, enemyHit, null, this);
```

* `enemyGroup` represents the sprite or group to check for being hit by the bullets. Change this to match the name of your sprite or group.
* `laser.bullets` refers to the weapon's bullets. Change `laser` to match the name of your weapon.
* `enemyHit` is the name of a custom function to run when an overlap is detected. List the function name without parentheses. You will have to create this custom function in your game. Change this to any unique function name that makes sense for your game.
* `null, this` are additional parameters that you need to list.

Next, you will need to create the custom function by adding it after your `update()` function:

```javascript
function enemyHit(enemy, bullet) {
    // add code to perform actions, such as: bullet.kill();

}
```

* `enemyHit` is the name of the custom function. Change this to match the name you listed in your `collide()` command.
* `enemy, bullet` are the parameter variables that will be passed into the function by your `collide()` command. These represent the two specific sprites that were involved in the collision. Change these to names that make sense for your collision. 
  * The parameters are received in the same order as the original groups are listed in your `overlap()` command, so the order of the names matters \(in order to make your code perform the actions you intend\).
  * The parameter names that you use will be different from the original group names. However, it will be much easier to understand what's happening in the custom function if the names are similar to the original group names.
  * Inside the custom function, whatever you do to the parameter variables will affect the original sprites they represent.
    * In this example function, `enemy` represents the specific member of the `enemyGroup` that was hit, and `bullet` represents the specific bullet that hit the `enemy`. So if the commands `enemy.kill()` and `bullet.kill()` were used inside the custom function, it would remove those specific members from the game world.

For example, the complete code for the `enemyHit()` function might look like this:

```javascript
function enemyHit(enemy, bullet) {
    bullet.kill(); // remove bullet
    enemy.kill();
    boomSound.play();
    score += 100;
    scoreText.text = 'Score ' + score;
}
```

## Change Collision Rectangle for Bullets

By default, the physics body collision area for a weapon's bullet is treated as a rectangle the same width and height of the image or spritesheet frames used for the bullet.

Sometimes the image or spritesheet for a bullet includes transparent areas around the outer edges. These transparent areas are counted as part of the bullet's collision area, so it can make collisions look or feel unnatural.

If necessary, you can change the size and position of the rectangle used to detect collisions of the bullet's physics body. \(This won't change the visual appearance of the bullet.\)

For example, to change the collision rectangle for the bullets of a weapon named `laser`:

```javascript
laser.setBulletBodyOffset(24, 12, 6, 6);
```

* `24, 12` represent the width and height \(in pixels\) of the collision rectangle. Change these numbers to match the size you want to use for your bullet.
* `6, 6` represent the horizontal offset and vertical offset \(in pixels\) of the collision rectangle's top-left corner, relative to the top-left corner of the bullet. Change these numbers to position the collision rectangle where you need it to be.

## Display Visual Weapon

There is no visual indication that a weapon has been added to the game. When the weapon is fired, a bullet will appear — but otherwise, there is no actual "weapon" displayed.

If you want to have a visual weapon displayed, then either include the weapon as part of the image or spritesheet used for the character carrying the weapon \(e.g., include a weapon in the frames of the player's spritesheet\) — or create a separate sprite for the weapon itself \(using its own image or spritesheet\).

If you do create a separate sprite for the weapon, then you would probably want to constantly update its position to match the position of the character that is carrying the weapon.

Using a separate sprite for the weapon would allow you to place the weapon in the game world for the player to find and collect — and to allow the player to change weapons \(by either changing to a different sprite or by changing to a different sprite frame\).

For example, let's assume that you've created a Phaser weapon named `laser` which contains the bullets group. Separately, you've created a sprite named `laserGun` that displays a visual weapon.

In your `update()` function, you could add this code to make the `laserGun` sprite follow the `player` sprite's position:

```javascript
laserGun.x = player.x;
laserGun.y = player.y;
```

* If necessary, you could add \(or subtract\) numbers from the `player.x` or `player.y` positions to fine-tune the pixel position of the `laserGun` relative to the `player`.

Then you could set the Phaser weapon named `laser` \(containing the bullets\) to track this sprite:

```javascript
laser.trackSprite(laserGun);
```

If you needed to switch the orientation of the `laserGun` sprite \(e.g., to point left instead of right\), then you could either switch to a different sprite frame — or just flip the sprite horizontally by scaling its width to negative one:

```javascript
if (arrowKey.right.isDown) {
    player.body.velocity.x = 200;
    laserGun.scale.x = 1; // face to right
    laser.fireAngle = 0; // point to right
}
else if (arrowKey.left.isDown) {
    player.body.velocity.x = 200;
    laserGun.scale.x = -1; // flip to left
    laser.fireAngle = 180; // point to left
}
else {
    player.body.velocity.x = 0;
}
```

The weapon's bullets would still be fired from `laser`:

```javascript
if (spacebar.isDown) laser.fire();
```

