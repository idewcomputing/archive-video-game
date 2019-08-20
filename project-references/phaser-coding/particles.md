# Particles

You can use Arcade Physics to add a particle emitter that can release particles for visual effects. Particle emitters can be used for one-time effects \(such as [explosions](http://phaser.io/examples/v2/particles/click-burst), etc.\) or continuous effects \(such as [rain](http://phaser.io/examples/v2/particles/rain), [snow](http://phaser.io/examples/v2/particles/snow), etc.\).

The "particle emitter" is really just a group of "particles" ready to be emitted \(similar to how a Phaser weapon is just a group of bullet objects\). The particles can be emitted from a single point or from an extended area.

By default, particles are emitted in random directions with slightly different speeds and rotations. As the particles fan out, they are pulled downward by gravity until they are removed after two seconds. However, you can customize the emitter to change how the particles are emitted \(speed, rotation, gravity, lifespan, etc.\).

### Phaser coding references in this section:

* Load Image or Spritesheet for Particles
* Add Emitter
* Add Particles to Emitter
* Set Particle Emitter Properties
  * Change Emitter Position
  * Set Emitter Size
  * Make Emitter Stay Fixed to Camera
  * Set Particle Speed
  * Set Particle Rotation
  * Set Particle Gravity
  * Set Particle Lifespan
  * Set Particle Transparency \(Fade In or Out\)
  * Set Particle Scale \(Change Size\)
* Emit Single Particle
* Emit One-Time Burst of Particles
* Emit Continuous Flow of Particles
* Make Particles Collide

### Phaser API Documentation:

* [Properties and Methods for Arcade Particle Emitter](https://photonstorm.github.io/phaser-ce/Phaser.Particles.Arcade.Emitter.html)

## Load Image or Spritesheet for Particles

The particles for a particle emitter can use an image or a spritesheet \(e.g., for animated particles\).

The image or spritesheet file for your particles must be loaded into the game's memory before you can add the particle emitter to your game.

```javascript
game.load.image('particle', 'assets/images/asteroid-piece.png');
```

The command to load an asset is always placed in your `preload()` function.

* `'particle'` represents an asset key — a key is sort of like a variable name. You decide what string to use for the key, as long as each image asset has a unique key.
* `'assets/images/asteroid-piece.png'` represents the folder path and filename of the image to load. Change this to match your specific folder path and filename.

## Add Emitter

Once the asset for the particles has been loaded into memory, you can add the particle emitter to your game.

You will need to declare a global variable for the particle emitter and assign the particle emitter to the variable in your `create()` function.

For example, to add a particle emitter to a variable named `asteroidEmitter`:

```javascript
asteroidEmitter = game.add.emitter(0, 0, 50);
```

* `0, 0` represent the center `x` and `y` position where the particles will be emitted from. For one-time effects \(such as explosions, etc.\), you will typically change the position later during gameplay. For continuous effects \(such as rain, etc.\), you might already know the specific position to use. Later, you can also change the size of the emitter to release particles from an extended area \(useful for effects like rain, etc.\). If necessary, change this position for your game.
* `50` represents the number of particles for this particle emitter. Particles are automatically recycled as they are emitted and removed, so this represents the maximum number that could be on-screen in the game at any one time. Change this number to what you need.

Even if the emitter is only intended for one-time bursts of particles, you may need to create enough particles for multiple bursts being on-screen at the same time, depending on on how frequently the bursts may occur.

As another example, to add a `rainEmitter` that will be positioned at the top-center of the game display \(and have 400 "rain" particles\):

```javascript
rainEmitter = game.add.emitter(game.camera.width / 2, 0, 400);
```

**NOTE:** There is no visual indication that a particle emitter has been added to the game — until you instruct it to release particles.

## Add Particles to Emitter

Once the particle emitter has been added, you need to actually add the particles to the emitter by using the image or spritesheet asset for the particles.

For example, to add the particles to the `asteroidEmitter` emitter:

```javascript
asteroidEmitter.makeParticles('particle');
```

* `'particle'` is the asset key for the particles. Change this to match your asset key name.

**NOTE:** There is no visual indication that the particles have been added to the emitter — until you instruct it to release particles.

## Set Particle Emitter Properties

This sub-section describes the most common particle emitter properties that you might change in your game.

These particle emitter properties are typically set in your `create()` function after adding the particle emitter. However, sometimes certain properties are adjusted during gameplay \(such as position, etc.\).

### Change Emitter Position

You can change the emitter position during gameplay by setting new values for the emitter's `x` and `y` coordinates. This can be done in the `update()` function or in a custom function.

Changing the position of an emitter is typically done when the emitter is used for one-time bursts of particles \(e.g., creating a particle explosion when an object is destroyed, etc.\).

For example, to set the emitter to a specific position using numbers:

```javascript
emitter.x = 400;
emitter.y = 300;
```

You can also set the emitter position by referencing the position of another object, such as a sprite.

For example, an emitter named `asteroidEmitter` is used to create a one-time burst of asteroid particles whenever a specific `asteroid` is destroyed. The emitter is set to the asteroid's position before removing the asteroid and creating the particle explosion:

```javascript
asteroidEmitter.x = asteroid.x;
asteroidEmitter.y = asteroid.x;
asteroid.kill();
asteroidEmitter.explode(1500, 5);
```

### Set Emitter Size

By default, a particle emitter will emit particles from a single point defined by its `x` and `y` coordinates.

However, you can set the size of the emitter to emit particles from an extended area. For example, you could emit "rain" particles from across the entire width of the game display.

To set the size of the emitter, just specify values for its width and height \(in pixels\).

```javascript
emitter.setSize(width, height);
```

For example, to set `rainEmitter` to the width of the game display \(but a height of just 1 pixel\):

```javascript
rainEmitter.setSize(game.camera.width, 1);
```

### Make Emitter Stay Fixed to Camera

If you have an extended game world, you can make the particle emitter stay fixed to the camera view. This is typically done for emitters that produce continuous weather effects \(rain, snow, etc.\).

First, set the emitter's `fixedToCamera` property to `true`. Next, set the center `x` position of the emitter using the `cameraOffset.x` property.

* **NOTE:** For some reason, the original `emitter.x` center position gets set to `0` when `emitter.fixedToCamera` is set to `true`. Setting `cameraOffset.x` seems to be the only way to shift the emitter position. If needed, you can also set a value for `cameraOffset.y`.

For example, to set `rainEmitter` to be fixed to the camera and centered horizontally within the game display \(assume that its `y` value was set to `0` when the emitter was added\):

```javascript
rainEmitter.fixedToCamera = true;
rainEmitter.cameraOffset.x = game.camera.width / 2;
```

### Set Particle Speed

By default, particles are emitted outward at random speeds \(though relatively slow\) and random directions. If needed, you can specify a range of speeds for the `x` direction and/or `y` direction. Each particle will receive a random speed within your specified range.

This can be used not only to change how fast or slow the particles move — but also which direction they move \(e.g., you could make "rain" particles only move downward, etc.\)

### Set Range for X Speed \(Horizontal\)

To set a range of speeds for the `x` direction \(horizontal\), provide values for the minimum and maximum values \(in pixels per second\):

```javascript
emitter.setXSpeed(min, max);
```

* `min` and `max` can be any values you want. Negative values for the `x` speed make particles move to the left, and positive values make particles move to the right.

### Set Range for Y Speed \(Vertical\)

To set a range of speeds for the `y` direction \(vertical\), provide values for the the minimum and maximum values \(in pixels per second\): :

```javascript
emitter.setYSpeed(min, max);
```

* `min` and `max` can be any values you want. Negative values for the `y` speed make particles move upwards, and positive values make particles move downwards.

For example, here are particle speed ranges for a `rainEmitter`:

```javascript
rainEmitter.setXSpeed(-5, 5);
rainEmitter.setYSpeed(300, 500);
```

* The `x` speed range has been set to allow the rain particles to move left \(negative\) or right \(positive\) but only at a very slow speed \(5 or less\).
* The `y` speed range has been set to only allow the rain particles to move down \(positive values\) but at fairly fast speed \(between 300-500 pixels per second\).

### Set Particle Rotation

By default, particles are emitted with a slow random rotation speed \(angular velocity\). If needed, you can specify a range for the angular velocity that controls the rotation. Each particle will receive a random rotation speed within your specified range. You can also turn off the rotation if desired.

To set a range of speeds for the rotation \(angular velocity\), provide values for the the minimum and maximum values \(in degrees per second\):

```javascript
emitter.setRotation(min, max);
```

* `min` and `max` can be any values you want. Negative values for rotation make particles rotate counter-clockwise, and positive values make particles rotate clockwise.

### Stop Particle Rotation

For certain particle emitters, you may not want the particles to rotate. For example, you may want "rain" particles to simply fall down without rotating.

To prevent particles from rotating, set the rotation range to zero:

```javascript
rainEmitter.setRotation(0, 0);
```

### Set Particle Gravity

By default, particles have a small amount of gravity in the vertical direction that pulls them downward. If desired, you can change the gravity values for the `x` and/or `y` directions. You can also turn off the particle's gravity.

This can be used make the particles respond more realistically to your game's simulated environment — but it also could be used as another way \(besides particle speed range\) to direct particles \(e.g., by pulling them in a specific direction\).

For certain games \(e.g., game using top-down perspective, game set in outer space, etc.\), you might not want the particles to be affected by gravity.

### Set Gravity for X Direction \(Horizontal\)

By default, the emitter's `gravity.x` value is set to `0` \(no gravity\).

To set a different gravity value for the `x` direction \(horizontal\), provide a value \(in pixels per second squared\):

```javascript
emitter.gravity.x = 50;
```

* `50` represents the gravity value \(in pixels per second squared\). Change this to the value you need for your particle effect. Negative values for `gravity.x` pull particles to the left, and positive values pull particles to the right.

### Set Gravity for Y Direction \(Vertical\)

By default, the emitter's `gravity.x` value is set to `100`.

To set a different gravity value for the `y` direction \(vertical\), provide a value \(in pixels per second squared\):

```javascript
emitter.gravity.y = 200;
```

* `200` represents the gravity value \(in pixels per second squared\). Change this to the value you need for your particle effect. Negative values for `gravity.y` pull particles upwards, and positive values pull particles downwards.

### Turn Off Particle Gravity

To turn off the gravity effect for particles, just set both gravity values to zero:

```javascript
emitter.gravity.setTo(0, 0);
```

### Set Particle Lifespan

By default, each particle will be automatically removed 2 seconds after being emitted. However, you can change the particle lifespan.

**NOTE:** You will typically set the particle lifespan when using the `explode()` or `flow()` methods to emit particles. So you don't necessarily need to directly set the `lifespan` property unless necessary for other reasons.

To set the particle lifespan:

```javascript
emitter.lifespan = 1000;
```

* `1000` represents the lifespan \(in milliseconds\) of each particle \(1000 ms = 1 second\). Change this to the duration you want.

### Set Particle Transparency \(Fade In or Out\)

If desired, you can change the transparency of a particle by changing its alpha value. You can make a particle semi-transparent by lowering its alpha value. You can also change the alpha value of each particle over time to make the particles fade in or fade out.

For example, "smoke" particles for an explosion would look more realistic if they were semi-transparent and faded out over time.

### Set Range for Particle Transparency

By default, particles have an alpha value of 1 \(no transparency applied\).

However, you can set your own range for the particle transparency by setting the minimum and maximum alpha values for the particles. Each particle will receive a random alpha value within your specified range.

For example, to set the alpha range for a `smokeEmitter`:

```javascript
smokeEmitter.minParticleAlpha = 0.2;
smokeEmitter.maxParticleAlpha = 0.8;
```

Change the values to whatever you need:

* An alpha value of 0 is completely transparent.
* Alpha values between 0-1 are semi-transparent. Lower numbers are more transparent.
* An alpha value of 1 has no transparency applied.

### Make Each Particle Fade Over Time

If desired, you can make each particle fade in or out over a specified amount of time.

To do this, you specify the starting and ending alpha values, as well as the amount of time over which the change in transparency should occur.

```javascript
emitter.setAlpha(startAlpha, endAlpha, duration);
```

* `startAlpha` represents the starting alpha value for each particle. Use whatever number you want.
* `endAlpha` represents the ending alpha value for each particle. Use whatever number you want, but it should be different from the starting value. Use a larger value to make the particles become less transparent \(fade in\), or use a smaller value to make the particles more transparent \(fade out\).
* `duration` represents the amount of time \(in milliseconds\) over which each particle will fade \(1000 ms = 1 second\). Change this to the duration you want.

A "fade in" will typically change the alpha from 0 to 1. It could start or end at other values, as long as the ending value is higher.

A "fade out" will typically change the alpha from 1 to 0. It could start or end at other values, as long as the ending value is lower.

For example, to make "rain" particles fade in over time

```javascript
rainEmitter.setAlpha(0, 1, 250);
```

For example, to make "smoke" particles fade out over time:

```javascript
smokeEmitter.setAlpha(0.8, 0, 2000);
```

### Make Each Particle Fade In and Fade Out

For example, to make "smoke" particles fade in and then fade out:

```javascript
smokeEmitter.setAlpha(0, 0.8, 2000, Phaser.Easing.Linear.None, true);
```

### Set Particle Scale \(Change Size\)

By default, all the particles will be displayed at actual size \(the size of your particle's image or spritesheet frame\). If desired, you can set a range to scale the particles, so they vary in size. Alternatively, you can make the particles change in size over a specified duration.

### Set Range for Particle Size

You can set a range for the particle sizes by setting the minimum and maximum scales for the particles. Each particle will receive a random scale within your specified range.

For example, to set the scale range for the `asteroidEmitter`:

```javascript
asteroidEmitter.minParticleScale = 0.5;
asteroidEmitter.maxParticleScale = 1.5;
```

Change the values to whatever you need:

* A scale value **less than 1** will make the particle smaller. For example, a value of 0.5 would make the particle half of its original size.
* A scale value of **exactly 1** will keep the particle its original size.
* A scale value **greater than 1** will make the particle larger. For example, a value of 2 would make the particle twice its original size.

### Make Each Particle Change in Size Over Time

If desired, you can make each particle change in size over a specified amount of time.

To do this, you specify the starting and ending scale values for the `x` direction and the `y` direction. \(To avoid distorting your particle images, use the same values for both `x` and `y`.\) Then specify the amount of time over which the change in size should occur.

```javascript
emitter.setScale(startX, endX, startY, endY, duration);
```

* `startX` represents the starting scale value in the `x` direction \(horizontal\). Use whatever number you want.
* `endX` represents the ending scale value in the `x` direction \(horizontal\). Use whatever number you want, but it should be different from the starting value. Use a larger value to make the particles grow larger, or use a smaller value to make the particles get smaller.
* `startY` represents the starting scale value in the `y` direction \(vertical\). It is recommended to use the same value as you did for `startX`.
* `endY` represents the ending scale value in the `y` direction \(vertical\). It is recommended to use the same value as you did for `endX`.
* `duration` represents the amount of time \(in milliseconds\) over which each particle will change in size \(1000 ms = 1 second\). Change this to the duration you want.

For example, to make "rain" particles grow in size over time

```javascript
rainEmitter.setScale(0.1, 0.5, 0.1, 0.5, 750);
```

For example, to make "asteroid" particles shrink in size over time:

```javascript
asteroidEmitter.setScale(1, 0.2, 1, 0.2, 1500);
```

### Make Each Particle Grow and Then Shrink

For example, to make "smoke" particles grow larger and then shrink:

```javascript
smokeEmitter.setScale(0.5, 1.5, 0.5, 1.5, 2000, Phaser.Easing.Linear.None, true);
```

## Emit Single Particle

If needed, you can emit a single particle from the emitter:

```javascript
emitter.emitParticle();
```

## Emit One-Time Burst of Particles

The `explode()` method is used to emit a one-time burst of particles from the emitter. You can either emit all the particles or a certain quantity. This is commonly used to create an explosion effect.

The `explode()` method is typically placed in the `update()` function or in a custom function to start the particle burst when needed.

For example, to emit a one-time burst of particles from an emitter named `asteroidEmitter`:

```javascript
asteroidEmitter.explode(1000, 5);
```

* `1000` represents the lifespan \(in milliseconds\) of each particle \(1000 ms = 1 second\). Change this to the duration you want.
* `5` represents the number of particles to emit. Change this to the quantity you want. \(If you want to emit all the particles, then just leave out this number\).

## Emit Continuous Flow of Particles

The `flow()` method is used to emit a continuous flow of particles from the emitter at specified time intervals.

You can make the flow emit one particle at a time — or multiple particles each time. You can allow the flow to run forever — or have it stop after a certain number of particles have been released. You can also stop the flow manually if needed.

For particle emitters being used for weather effects \(such as rain, etc.\), the `flow()` method is typically placed in the `create()` method, so that the particle flow will be running at the start of the game.

However, you can also use the `flow()` method in your `update()` function or in a custom function to start or stop the particle flow when needed.

### Flow that Emits One Particle at a Time

To continuously emit one particle at specified intervals:

```javascript
rainEmitter.flow(1500, 25);
```

* `1500` represents the lifespan \(in milliseconds\) of each particle \(1000 ms = 1 second\). Change this to the duration you want.
* `25` represents the time interval \(in milliseconds\) between each release of a particle \(1000 ms = 1 second\). Change this to the interval you want.

### Flow that Emits Multiple Particles at a Time

To continuously emit multiple particles at specified intervals:

```javascript
emitter.flow(lifespan, interval, quantity);
```

```javascript
rainEmitter.flow(1500, 25, 5);
```

* `1500` represents the lifespan \(in milliseconds\) of each particle \(1000 ms = 1 second\). Change this to the duration you want.
* `25` represents the time interval \(in milliseconds\) between each release of a particle \(1000 ms = 1 second\). Change this to the interval you want.
* `5` represents the quantity of particles to release at each interval. Change this to the quantity you want.

### Flow that Stops Emitting After Certain Number of Particles

To have the emitter stop flowing after a certain number of particles have been released:

```javascript
rainEmitter.flow(1500, 25, 5, 2000);
```

* `1500` represents the lifespan \(in milliseconds\) of each particle \(1000 ms = 1 second\). Change this to the duration you want.
* `25` represents the time interval \(in milliseconds\) between each release of a particle \(1000 ms = 1 second\). Change this to the interval you want.
* `5` represents the quantity of particles to release at each interval. Change this to the quantity you want.
* `2000` represents the total number of particles to release before stopping. Change this to the total you want.

### Manually Stop Flow

You can also manually stop the flow from an emitter by using:

```javascript
rainEmitter.flow(0, 0, 1, 1);
```

This makes the emitter release one last particle and then turn off automatically. If necessary, you can always turn the emitter back on again using a different `flow()` command.

## Make Particles Collide

By default, particles do not collide with each other or with other sprites. However, if you want, you can [make them collide](http://phaser.io/examples/v2/particles/when-particles-collide). The particle emitter acts like a group of sprites, so just use the variable name for the particle emitter in `collide()` methods to make the individual particles collide with each other and/or with other sprites \(or groups\).

* See the [Physics and Collisions](physics-and-collisions.md) reference section for more details on the `collide()` method.

