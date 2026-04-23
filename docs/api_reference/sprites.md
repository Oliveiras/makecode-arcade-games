# MakeCode Arcade / API Reference Guide / Sprites

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Sprites** module.

This module creates and moves game objects. It also handle overlaps between objects.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Create sprites

### create

Create a new sprite with a pixel image.

```js
function sprites.create(img: Image, kind: number): Sprite;
```

Sprites provide all the operations to move and animate images. Your sprites can detect collisions and overlaps with other objects. Initially, sprites have a position in the middle of screen and have no motion. You set the location and movement of a sprite in your code. Sprites are associated with a kind so that they can be identifed as part of group or type, such as Player and Enemy.

**_Parameters_**

- **img:** an image to create a sprite for.
- **kind:** the type of sprite to create. There are default sprite kinds already defined like Player, Enemy, Food, etc. You can add your own kind to the currently defined SpriteKind (namespace) types also.

**_Returns_**

a game sprite containing an image.

**_Examples_**

**1. Smiley face**

Make a sprite for a smiley face image and display it on the screen.

```js
let smiley: Sprite = null
smiley = sprites.create(img`
. . . . . 1 1 1 1 1 1 1 . . . . 
. . . 1 1 e e e e e e e 1 . . . 
. . 1 e e e e e e e e e e 1 . . 
. 1 e e e e e e e e e e e e 1 . 
1 e e e e 1 1 e e e 1 1 e e e 1 
1 e e e e 1 1 e e e 1 1 e e e 1 
1 e e e e e e e e e e e e e e 1 
1 e e e e e e e e e e e e e e 1 
1 e e e e e e e 1 e e e e e e 1 
1 e e e e e e e e e e e e e e 1 
1 e e e e 1 e e e e e 1 e e e 1 
1 e e e e e 1 1 1 1 1 e e e e 1 
. 1 e e e e e e e e e e e e 1 . 
. . 1 e e e e e e e e e e 1 . . 
. . . 1 1 e e e e e e e 1 . . . 
. . . . . 1 1 1 1 1 1 1 . . . . 
`, SpriteKind.Player)
```

**2. Drive the car**

Set the scene background color and make a sprite with a car image. Set the car in motion toward the right of the screen.

```js
let theCar: Sprite = null
scene.setBackgroundColor(3)
theCar = sprites.create(img`
. . . . . f f f f f f f f f f f f f f f f . . . . . . . . . . . 
. . . . f d d d d d d d d d d d d d d d d f . . . . . . . . . . 
. . . f d d d d f f f f f f d d f f f f f f f . . . . . . . . . 
. . . f d d d d f . . . . f d d f . . . . . f f . . . . . . . . 
. . . f d d d d f . . . . f d d f . . . . . . f . . . . . . . . 
. . f d d d d d f f . . . f d d f . . . f f f d f f f f f . . . 
. f d d d d d d d f f f f f d d f f f f d d d d d d d d f . . . 
. f d d d f f f d d d d d d d d d d d d d d f f f d d d f . . . 
. f d d f f f f f d d d d d d d d d d d d f f f f f d d f . . . 
. f f . f f f f f . f f f f f f f f f f . f f f f f . f f . . . 
. . . . f f f f f . . . . . . . . . . . . f f f f f . . . . . . 
. . . . . f f f . . . . . . . . . . . . . . f f f . . . . . . . 

`, SpriteKind.Player)
theCar.vx = 20
game.onUpdate(function () {
    if (theCar.x > scene.screenWidth() + 16) {
        theCar.x = -16
    }
})
```

### createProjectileFromSprite

Create a new motion sprite that starts from the center of another sprite.

```js
function sprites.createProjectileFromSprite(img: Image, sprite: Sprite, vx: number, vy: number): Sprite;
```

A projectile is a motion sprite that moves from the location it’s created at. It moves with speeds (velocities vx and vy) that you set in both the horizontal and vertical directions.

The projectile sprite starts from the center of a sprite that you set for it. The vx and vy velocities have either positive or negative values which determine the direction that the projectile moves in.

The projectile has many of the same properties that a non-moving sprite has. It will overlap with other sprites, hit and overlap tiles.

Projectiles are destroyed when they move off of the screen.

**_Parameters_**

- **img:** an image for the projectile sprite.
- **sprite:** the sprite to start the projectile from.
- **vx:** the speed in the horizontal direction for the sprite to move at.
- **vy:** the speed in the vertical direction for the sprite to move at.

**_Returns_**

a new projectile sprite that moves with set velocities.

**_Examples_**

**1. Send projectiles out of the box**

Create a sprite in the shape of a box. Send projectiles out in random directions from the center of the box.

```js
let projectile: Sprite = null
let mySprite = sprites.create(img`
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 . . . . . . . . . . . . . . 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    `, SpriteKind.Player)
game.onUpdateInterval(1000, function () {
    projectile = sprites.createProjectileFromSprite(img`
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . 8 8 8 8 . . . . . . 
        . . . . . 8 8 8 8 8 8 . . . . . 
        . . . . . 8 8 a a 8 8 . . . . . 
        . . . . . 8 8 a a 8 8 . . . . . 
        . . . . . 8 8 8 8 8 8 . . . . . 
        . . . . . . 8 8 8 8 . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        `, mySprite, randint(-50, 50), randint(-50, 50))
})
```

**2. Photon blaster**

Send photons from a spaceship when the B button is pressed.

```js
let photon: Sprite = null
let target = sprites.create(img`
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    e e e e e e e . . . . . . . . . 
    `, SpriteKind.Enemy)
let ship = sprites.create(img`
    .....bbbbbbbbbb...........
    bbbbbbddddddddbbbb........
    bbdddddddddddddddbbbb.....
    .bdddddffffffdddddddbbbb..
    .bdddddddddddddddffddddbb.
    .bbddddddddddddddffdddddbb
    ..bdddfffffffffddddddddbb.
    ..bddddddddddddddddbbbbb..
    .bbdddddddddddddbbbb......
    .bddddddddddbbbb..........
    bbddbbbbbdbbb.............
    bbbbb...bbb...............
    `, SpriteKind.Player)
ship.x = 20
target.x = scene.screenWidth() - 10
controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    photon = sprites.createProjectileFromSprite(img`
        . . . . . . 1 1 1 . . 
        . . . 1 1 1 1 1 1 1 . 
        1 1 1 1 1 1 1 1 1 1 1 
        . . . 1 1 1 1 1 1 1 . 
        . . . . . . 1 1 1 . . 
        `, ship, 150, 0)
})
sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprite.startEffect(effects.disintegrate, 100)
})
```

### createProjectileFromSide

Create a new motion sprite that starts at the side of the screen.

```js
function sprites.createProjectileFromSide(img: Image, vx: number, vy: number): Sprite;
```

A projectile is a motion sprite that moves from the location it’s created at. It moves with speeds (velocities vx and vy) that you set in both the horizontal and vertical directions.

The side of the screen that the projectile starts from depends on the direction of the velocity values it’s given. The vx and vy velocities have either positive or negative values which determine the direction. The projectile starts from a side of the screen based on the velocity direction shown in the following table.

| Side        | vx       | vy
| ----------- | -------- | --------
| upper left  | positive | positive
| lower left  | positive | negative
| upper right | negative | positive
| lower right | negative | negative


The projectile has many of the same properties that a non-moving sprite has. It will overlap with other sprites, hit and overlap tiles.

Projectiles are destroyed when they move off of the screen.

**_Parameters_**

- **img:** an image for the projectile sprite.
- **vx:** the speed in the horizontal direction for the sprite to move at.
- **vy:** the speed in the vertical direction for the sprite to move at.

**_Returns_**

a new projectile sprite that moves with set velocities.

**_Examples_**

**1. Create a projectile at the bottom left**

Start a projectile sprite from the bottom left of the screen. Make it bounce on the sides of the screen.

```js
let projectile = sprites.createProjectileFromSide(img`
    . . . . 5 5 5 5 5 5 5 5 . . . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
    . 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
    5 5 5 5 5 5 . . . . 5 5 5 5 5 5 
    5 5 5 5 5 . . . . . . 5 5 5 5 5 
    5 5 5 5 . . . . . . . . 5 5 5 5 
    5 5 5 5 . . . . . . . . 5 5 5 5 
    5 5 5 5 . . . . . . . . 5 5 5 5 
    5 5 5 5 . . . . . . . . 5 5 5 5 
    5 5 5 5 5 . . . . . . 5 5 5 5 5 
    5 5 5 5 5 5 . . . . 5 5 5 5 5 5 
    . 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
    . 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . . . 5 5 5 5 5 5 5 5 . . . . 
    `, 40, -30)
projectile.setFlag(SpriteFlag.BounceOnWall, true)
```

**2. Create projectiles for all sides**

Create projectiles that start randomly from all sides of the screen. Show the projectile direction by displaying the physics properties.

```js
let projectile: Sprite = null
game.onUpdateInterval(2000, function () {
    projectile = sprites.createProjectileFromSide(img`
        . 7 7 7 7 7 7 7 7 7 7 7 7 7 7 . 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
        . 7 7 7 7 7 7 7 7 7 7 7 7 7 7 . 
        `, randint(-50, 50), randint(-50, 50))
    projectile.setFlag(SpriteFlag.ShowPhysics, true)
})
```


## Sprite actions

### say

Display a caption with some text near a sprite.

```js
(method) Sprite.say(text: any, timeOnScreen: number, textColor: number, textBoxColor: number): void;
```

Sometimes you may want to have a message show up for a sprite. You can have a caption with a text string appear next to sprite and have it say something to the player.

**_Parameters_**

- **text:** a string that contains the text of the caption.
- **millis:** an optional number of milliseconds to display the caption for.

**_Examples_**

**1. Smiley message**

Make a sprite for a smiley face image and display it on the screen with a message.

```js
let smiley: Sprite = null
smiley = sprites.create(img`
. . . . . f f f f f f f . . . . 
. . . f f e e e e e e e f . . . 
. . f e e e e e e e e e e f . . 
. f e e e e e e e e e e e e f . 
f e e e e f f e e e f f e e e f 
f e e e e f f e e e f f e e e f 
f e e e e e e e e e e e e e e f 
f e e e e e e e e e e e e e e f 
f e e e e e e e f e e e e e e f 
f e e e e e e e e e e e e e e f 
f e e e e f e e e e e f e e e f 
f e e e e e f f f f f e e e e f 
. f e e e e e e e e e e e e f . 
. . f e e e e e e e e e e f . . 
. . . f f e e e e e e e f . . . 
. . . . . f f f f f f f . . . . 
`)
smiley.say("Hello!")
```

**2. Yellow message**

Make a square yellow sprite in the middle of the screen. Send a moving person sprite around the screen. When the person sprite crosses the yellow square, make the person say “Yellow!”.

```js
sprites.onOverlap(SpriteKind.Player, SpriteKind.Player, function (sprite, otherSprite) {
    sprite.sayText("Yellow!", 100, false)
})
scene.setBackgroundColor(13)
let box = sprites.create(img`
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
    `, SpriteKind.Player)
let person = sprites.create(img`
    . . . . . . f f f f . . . . . . 
    . . . . f f f 2 2 f f f . . . . 
    . . . f f f 2 2 2 2 f f f . . . 
    . . f f f e e e e e e f f f . . 
    . . f f e 2 2 2 2 2 2 e e f . . 
    . . f e 2 f f f f f f 2 e f . . 
    . . f f f f e e e e f f f f . . 
    . f f e f b f 4 4 f b f e f f . 
    . f e e 4 1 f d d f 1 4 e e f . 
    . . f e e d d d d d d e e f . . 
    . . . f e e 4 4 4 4 e e f . . . 
    . . e 4 f 2 2 2 2 2 2 f 4 e . . 
    . . 4 d f 2 2 2 2 2 2 f d 4 . . 
    . . 4 4 f 4 4 5 5 4 4 f 4 4 . . 
    . . . . . f f f f f f . . . . . 
    . . . . . f f . . f f . . . . . 
    `, SpriteKind.Player)
person.setBounceOnWall(true)
person.setVelocity(50, 50)
```

### overlapsWith

Check if this sprite is overlapping another sprite.

```js
(method) Sprite.overlapsWith(other: Sprite): boolean;
```

An overlap of two sprites is dectected when the first non-transparent pixel in the image of the first sprite overlaps the first non-transparent pixel of the second sprite. If a sprite has it’s ghost flag set, any overlap with another sprite won’t be noticed. Also, an overlap occurs even when the values of Z for the sprites are different.

**_Parameters_**

- **other:** the other sprite to check for overlap with this sprite.

**_Returns_**

a boolean value which is true if the other sprite is overlapping this sprite.

**_Examples_**

**1. Change color on overlap.**

Send a blue square toward a red square. While the red square is overlapping the blue square, randomly change the color of the “red” square to something else.

```js
let redBox: Image = null
let blueBox: Image = null
let blueBoxStay: Sprite = null
let redBoxStay: Sprite = null
blueBox = image.create(32, 32)
blueBox.fill(4)
redBox = image.create(32, 32)
redBox.fill(10)
blueBoxStay = sprites.create(blueBox)
redBoxStay = sprites.create(redBox)
blueBoxStay.x = 0
blueBoxStay.vx = 10
game.onUpdateInterval(100, function () {
    if (redBoxStay.overlapsWith(blueBoxStay)) {
        redBox.fill(Math.randomRange(1, 15))
    }
})
```

### destroy

Destroy the sprite.

```js
(method) Sprite.destroy(effect: effects.ParticleEffect, duration: number): void;
```

The sprite destroys itself immediately. The sprite is removed from the game and will no longer overlap or collide with any other sprites.

Sprites will also destroy when their lifespan count reaches 0 or they have auto destroy set as a sprite flag when they move off screen.

You can also select an optional particle effect to display at the sprite when it’s destroyed.

**_Parameters_**

- **effect:** an optional built-in effect to display when the sprite is destroyed.

**_Examples_**

**1. Gulp the player**

Make a Player and an Enemy sprite. The player moves into the open area of the enemy sprite. When an overlap of the two is detected, destroy the Player.

```js
let enemy: Sprite = null
let catchBox: Image = null
let player: Sprite = null
let yellowBlock: Image = null

yellowBlock = image.create(16, 16)
yellowBlock.fill(5)
yellowBlock.drawRect(0, 0, 16, 16, 1)
player = sprites.create(yellowBlock, SpriteKind.Player)
player.left = 0
catchBox = image.create(32, 32)
catchBox.drawRect(0, 0, 32, 32, 1)
catchBox.drawLine(0, 6, 0, 25, 0)
enemy = sprites.create(catchBox, SpriteKind.Enemy)
enemy.right = scene.screenWidth() - 16
player.vx = 20
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    otherSprite.say("Gulp!", 1000)
    sprite.destroy()
})
```

**2. Show a destroy effect**

Create a square sprite. Destroy the sprite after 3 seconds and show a fountain effect.

```js
let square = image.create(32, 32)
square.fill(2)
square.drawRect(0, 0, 32, 32, 1)
let sqSprite = sprites.create(square)
pause(3000)
sqSprite.destroy(effects.fountain)
```

### setFlag

Set a sprite flag to ON or OFF.

```js
(method) Sprite.setFlag(flag: SpriteFlag, on: boolean): void;
```

Sprite flags are settings that change the way a sprite reacts on the screen. When sprites are created they have default behaviors that are set by these flags. The flags determine if a sprite is forced to always stay on the screen or if it gets destroyed when moves off of the screen. Another behavior, ghosting, causes the sprite to not overlap other sprites or collide with obstacles. There are flags to set the behavior of a sprite when it hits walls or overlaps other sprites.

**_Parameters_**

- **flag:** the sprite flag to turn on or off. The flags are:
  - **stay in screen:** the sprite is forced to stay on the screen when it reaches a screen edge.
  - **ghost:** the sprite never overlaps other sprites, it goes through wall tiles, and tile overlaps and wall hits aren’t detected.
  - **auto destroy:** the sprite is automatically destroyed when it moves off the screen.
  - **destroy on wall:** the sprite is automatically destroyed when it collides with a wall tile.
  - **bounce on wall:** the sprite will deflect when it collides with a wall tile
  - **show physics:** the sprite will display its position, velocity, and acceleration below it.
  - **invisible:** the sprite will not be drawn to the screen.
  - **relative to camera:** the sprite is drawn relative to the camera rather than relative to the world and the sprite never overlaps other sprites or collides with obstacles. This is useful for drawing static elements to the screen (scores, dialog boxes, etc.) that shouldn’t move when the camera pans.
  - **ghost through sprites:** the sprite will pass over or under other sprites and not create an overlap event.
  - **ghost through tiles:** the sprite will not trigger events when passing over tiles.
  - **ghost through walls:** the sprite will pass through wall tiles not trigger a will hit event.

**_Flags_**

**Stay in screen**

Setting stay in screen to ON forces the sprite to remain in the view of the screen. The sprite stays in the current view of the screen even if a new position set for it is outside of the screen. The sprite in the following example always appears on the screen even though is initial position is off the screen and its motion will send it past the screen edge.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setFlag(SpriteFlag.StayInScreen, true)
pause(500)
mySprite.left = -20
mySprite.top = -50
pause(500)
mySprite.vx = 30
```

> Scene or screen? \
> A sprite will always stay inside the limits of your scene if your game has a tilemap. Setting the stay in screen flag to OFF will not let your sprite keep moving past the edge of the game scene. If the scene is larger than the screen and you have auto destroy set to ON, the sprite will destroy itself when it moves past the edge of the screen even though it’s still inside the scene.

**Ghost**

A ghost sprite will pass through wall tiles and causes no overlap events with tiles or other sprites.

The example here shows a ghost sprite moving past another sprite without an overlap event occuring. It also passes over a non-wall tile and through a wall tile without stopping or triggering tile events.

```js
scene.onHitWall(SpriteKind.Player, function (sprite, location) {
    sprite.say("Ouch!")
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Player, function (sprite, otherSprite) {
    otherSprite.say("Boo!")
})
scene.onOverlapTile(SpriteKind.Player, sprites.dungeon.floorDark2, function (sprite, location) {
    sprite.say("Pass!")
})
let ghost = sprites.create(img`
    . . . . . . d d d d d . . . . . 
    . . . d d d d 1 1 1 d d d . . . 
    . . d d 1 1 1 1 1 1 1 1 d d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d d . 
    . d d 1 1 1 f 1 1 1 f 1 1 1 d . 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 d d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d d 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 d 1 1 1 1 1 1 d 
    d 1 d d d 1 1 d d d d 1 d 1 1 d 
    d d d . d d d d . . d d d d d d 
    d d . . . d d . . . . d . . d d 
    `, SpriteKind.Player)
let person = sprites.create(img`
    . . . . . . . . . . . . . . 
    . . . . . f f f f . . . . . 
    . . . f f 5 5 5 5 f f . . . 
    . . f 5 5 5 5 5 5 5 5 f . . 
    . f 5 5 5 5 5 5 5 5 5 5 f . 
    c b 5 5 5 d b b d 5 5 5 b c 
    f 5 5 5 b 4 4 4 4 b 5 5 5 f 
    f 5 5 c c 4 4 4 4 c c 5 5 f 
    f b b f b f 4 4 f b f b b f 
    f b b e 1 f d d f 1 e b b f 
    c f b f d d d d d 4 4 b f c 
    . c e c 6 9 9 9 4 d d d c . 
    . e 4 c 9 9 9 9 4 d d 4 c . 
    . e c b b 3 b b b e e c . . 
    . . c c 3 3 b 3 b 3 c c . . 
    . . . . c b b c c c . . . . 
    `, SpriteKind.Player)
tiles.setTilemap(tilemap`level_2`)
ghost.setFlag(SpriteFlag.Ghost, true)
ghost.left = 0
person.left = 30
ghost.vx = 40
```

**Auto destroy**

Sprites with the auto destroy flag on are destroyed when the sprite’s image moves past the edge of the screen.

This example continuously creates sprites and destroys them when they move off of the screen. The score value is used to count the sprites that are destroyed.

```js
sprites.onDestroyed(SpriteKind.Player, function (sprite) {
    info.changeScoreBy(1)
})
let mySprite: Sprite = null
info.setScore(0)
game.onUpdateInterval(500, function () {
    mySprite = sprites.create(img`
        . . . . . 2 2 2 2 2 2 . . . . . 
        . . . 2 2 2 2 2 2 2 2 2 2 . . . 
        . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
        . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
        . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
        2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
        2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
        2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
        2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
        2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
        2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
        . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
        . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
        . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
        . . . 2 2 2 2 2 2 2 2 2 2 . . . 
        . . . . . 2 2 2 2 2 2 . . . . . 
        `, SpriteKind.Player)
    mySprite.setFlag(SpriteFlag.AutoDestroy, true)
    mySprite.left = 0
    mySprite.vx = 200
})
```

**Destroy on wall**

The sprite is destroyed when it meets a wall tile or reaches the edge of the scene’s tilemap.

```js
sprites.onDestroyed(SpriteKind.Player, function (sprite) {
    sprite.startEffect(effects.disintegrate)
})
let mySprite: Sprite = null
mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
tiles.setTilemap(tilemap`level_0`)
mySprite.setFlag(SpriteFlag.DestroyOnWall, true)
mySprite.left = 0
mySprite.vx = 30
```

**Bounce on wall**

The bounce on wall flag causes the sprite to bounce away from a wall tile when it contacts it. The sprite will also bounce back from the edge of the scene’s tilemap.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setFlag(SpriteFlag.BounceOnWall, true)
tiles.setTilemap(tilemap`level_0`)
mySprite.vx = 50
mySprite.vy = 50
```

If the scene has no tilemap set, the sprite will bounce off the edges of the screen.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setFlag(SpriteFlag.BounceOnWall, true)
mySprite.vx = 50
mySprite.vy = 50
```

**Show physics**

A sprite with show physics set ON will show its position and motion settings in a caption next to it.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setFlag(SpriteFlag.ShowPhysics, true)
mySprite.x = 10
mySprite.y = 10
mySprite.ax = 7
mySprite.ay = 5
```

**Invisible**

Setting the invisible sprite flag causes the sprite to not be displayed on the screen.

```js
let visible = false
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.left = 0
mySprite.vx = 10
game.onUpdateInterval(1000, function () {
    if (visible) {
        mySprite.setFlag(SpriteFlag.Invisible, false)
    } else {
        mySprite.setFlag(SpriteFlag.Invisible, true)
    }
    visible = !(visible)
})
```

**Relative to camera**

The relative to camera flag fixes the position of the sprite on the screen as the view of the scene changes. This keep the sprite from moving off the screen as the camera view moves. Also, there sprite creates no collisions and doesn’t overlap other sprites.

The following example sets a sprite in the upper right corner of the screen. The sprite remains at the same location on the screen as the camera view of the scene moves from left to right.

```js
tiles.setTilemap(tilemap`level_3`)
let mover = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
let fixer = sprites.create(img`
    . . . . . 7 7 7 7 7 7 . . . . . 
    . . . 7 7 7 7 7 7 7 7 7 7 . . . 
    . . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
    . 7 7 7 7 7 7 7 7 7 7 7 7 7 7 . 
    . 7 7 7 7 7 7 7 7 7 7 7 7 7 7 . 
    7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
    7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
    7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
    7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
    7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
    7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
    . 7 7 7 7 7 7 7 7 7 7 7 7 7 7 . 
    . 7 7 7 7 7 7 7 7 7 7 7 7 7 7 . 
    . . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
    . . . 7 7 7 7 7 7 7 7 7 7 . . . 
    . . . . . 7 7 7 7 7 7 . . . . . 
    `, SpriteKind.Player)
fixer.setFlag(SpriteFlag.RelativeToCamera, true)
fixer.top = 0
fixer.right = scene.screenWidth()
mover.left = 0
mover.vx = 50
scene.cameraFollowSprite(mover)
```

**Ghost through sprites**

Setting ghost through sprites will allow a sprite to pass over or under another sprite without causing an overlap event.

The following example shows a ghost sprite passing under another sprite without triggering an overlap event.

```js
sprites.onOverlap(SpriteKind.Player, SpriteKind.Player, function (sprite, otherSprite) {
    otherSprite.say("Boo!")
})
let ghost = sprites.create(img`
    . . . . . . d d d d d . . . . . 
    . . . d d d d 1 1 1 d d d . . . 
    . . d d 1 1 1 1 1 1 1 1 d d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d d . 
    . d d 1 1 1 f 1 1 1 f 1 1 1 d . 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 d d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d d 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 d 1 1 1 1 1 1 d 
    d 1 d d d 1 1 d d d d 1 d 1 1 d 
    d d d . d d d d . . d d d d d d 
    d d . . . d d . . . . d . . d d 
    `, SpriteKind.Player)
    let person = sprites.create(img`
    . . . . . . . . . . . . . . 
    . . . . . f f f f . . . . . 
    . . . f f 5 5 5 5 f f . . . 
    . . f 5 5 5 5 5 5 5 5 f . . 
    . f 5 5 5 5 5 5 5 5 5 5 f . 
    c b 5 5 5 d b b d 5 5 5 b c 
    f 5 5 5 b 4 4 4 4 b 5 5 5 f 
    f 5 5 c c 4 4 4 4 c c 5 5 f 
    f b b f b f 4 4 f b f b b f 
    f b b e 1 f d d f 1 e b b f 
    c f b f d d d d d 4 4 b f c 
    . c e c 6 9 9 9 4 d d d c . 
    . e 4 c 9 9 9 9 4 d d 4 c . 
    . e c b b 3 b b b e e c . . 
    . . c c 3 3 b 3 b 3 c c . . 
    . . . . c b b c c c . . . . 
    `, SpriteKind.Player)
ghost.setFlag(SpriteFlag.GhostThroughSprites, true)
ghost.left = 0
ghost.vx = 40
```

**Ghost through tiles**

Setting ghost through tiles to ON lets a sprite pass over a tile without causing an overlap event.

The example here shows a ghost sprite crossing over a tile without being detected in an overlap event.

```js
scene.onOverlapTile(SpriteKind.Player, sprites.dungeon.floorDark0, function (sprite, location) {
    sprite.say("Pass!")
})
let ghost = sprites.create(img`
    . . . . . . d d d d d . . . . . 
    . . . d d d d 1 1 1 d d d . . . 
    . . d d 1 1 1 1 1 1 1 1 d d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d d . 
    . d d 1 1 1 f 1 1 1 f 1 1 1 d . 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 d d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d d 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 d 1 1 1 1 1 1 d 
    d 1 d d d 1 1 d d d d 1 d 1 1 d 
    d d d . d d d d . . d d d d d d 
    d d . . . d d . . . . d . . d d 
    `, SpriteKind.Player)
tiles.setTilemap(tilemap`level_0`)
ghost.setFlag(SpriteFlag.GhostThroughTiles, true)
ghost.left = 0
ghost.vx = 40
```

**Ghost through walls**

Setting the ghost through walls flag lets a sprite pass through wall tiles. Also, a wall hit won’t occur when it reaches a wall tile.

The example here shows a ghost sprite passing through a wall tile without being detected.

```js
scene.onHitWall(SpriteKind.Player, function (sprite, location) {
    sprite.say("Ouch!")
})
let ghost = sprites.create(img`
    . . . . . . d d d d d . . . . . 
    . . . d d d d 1 1 1 d d d . . . 
    . . d d 1 1 1 1 1 1 1 1 d d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d d . 
    . d d 1 1 1 f 1 1 1 f 1 1 1 d . 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 d d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d d 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 d 1 1 1 1 1 1 d 
    d 1 d d d 1 1 d d d d 1 d 1 1 d 
    d d d . d d d d . . d d d d d d 
    d d . . . d d . . . . d . . d d 
    `, SpriteKind.Player)
tiles.setTilemap(tilemap`level_1`)
ghost.setFlag(SpriteFlag.GhostThroughWalls, true)
ghost.left = 0
ghost.vx = 40
```

### setImage

Set a new image for the sprite.

```js
(method) Sprite.setImage(img: Image): void;
```

The image currently set for the sprite is replaced by the new image. The new image is displayed for the sprite.

**_Parameters_**

- **img:** image that is the currently set for the sprite.

**_Examples_**

**1. Change image of sprite every second.**

Make two square block images, one with green pixels, the other with orange pixels. Every second, set the green or the orange image to the sprite on the screen.

```js
let mySprite: Sprite = null
let image2: Image = null
let image1: Image = null
let toggle = false
image1 = image.create(32, 32)
image2 = image1.clone()
image1.fill(7)
image2.fill(4)
mySprite = sprites.create(image1, 0)
game.onUpdateInterval(1000, function () {
    if (toggle) {
        mySprite.setImage(image1)
    } else {
        mySprite.setImage(image2)
    }
    toggle = !(toggle)
})
```

### setPosition

Set the center position of a sprite on the screen.

```js
(method) Sprite.setPosition(x: number, y: number): void;
```

**_Parameters_**

- **x:** the new horizontal center position for the sprite on the screen.
- **y:** the new vertical center position for the sprite on the screen.

**_Sprite locations_**

The sprite image forms a rectangle with some number of pixel rows and columns. The x postion of the sprite is the horizontal location of the center column of the sprite’s pixels on the screen. The y postion of the sprite is the vertical location of the center row of the sprite’s pixels.

The x position of the sprite can have a value that is greater than the width of the screen. It can also have a value that is less than the left side of the screen (the left of screen is 0 and the value of the x position of the sprite in this case is negative). When this happens, some or all of the sprite isn’t visible on the screen.

Similarly, the y position of the sprite can have a value that is greater than the height of the screen. It can also have a value that is less than the top side of the screen (the top of screen is 0 and the value of the y position of the sprite in this case is negative).

**_Examples_**

**1. Set and say coordinates.**

Set the sprite x and y locations to 64. Have the sprite say what it’s x and y values are.

```js
namespace SpriteKind {
    export const Example = SpriteKind.create()
}
let mySprite = sprites.create(img`
7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
7 7 2 2 2 2 2 2 2 2 2 2 2 2 7 7 
7 5 7 2 2 2 2 2 2 2 2 2 2 7 4 7 
7 5 5 7 2 2 2 2 2 2 2 2 7 4 4 7 
7 5 5 5 7 2 2 2 2 2 2 7 4 4 4 7 
7 5 5 5 5 7 2 2 2 2 7 4 4 4 4 7 
7 5 5 5 5 5 7 2 2 7 4 4 4 4 4 7 
7 5 5 5 5 5 5 7 7 4 4 4 4 4 4 7 
7 5 5 5 5 5 5 7 7 4 4 4 4 4 4 7 
7 5 5 5 5 5 7 8 8 7 4 4 4 4 4 7 
7 5 5 5 5 7 8 8 8 8 7 4 4 4 4 7 
7 5 5 5 7 8 8 8 8 8 8 7 4 4 4 7 
7 5 5 7 8 8 8 8 8 8 8 8 7 4 4 7 
7 5 7 8 8 8 8 8 8 8 8 8 8 7 4 7 
7 7 8 8 8 8 8 8 8 8 8 8 8 8 7 7 
7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 
`, SpriteKind.Example)
mySprite.setPosition(64, 64)
mySprite.say("I'm centered at x:" + mySprite.x + ", y:" + mySprite.y)
```

### setKind

Set the sprite kind.

```js
(method) Sprite.setKind(value: number): void;
```

**_Parameter_**

- **value:** a number value that is the new kind to set for the sprite.

**_Sprite kinds_**

To keep track of different types of sprites, you can assign a kind to them. This is a value that will help identify them and decide what actions to take when events happen in the game. There are no particular rules or names for how you decide on what the kinds should be. The way to do it though is to assign a kind in the SpriteKind namespace.

```js
namespace SpriteKind {
    export const Star = SpriteKind.create()
    export const Comet = SpriteKind.create()
}
```

There are some default kinds that are already part of SpriteKind. These are:

```js
SpriteKind.Player
SpriteKind.Enemy
SpriteKind.Food
SpriteKind.Projectile
```

When you create a sprite, you can optionally assign it a kind:

```js
let mySprite = sprites.create(img`
2 4
4 2
`, SpriteKind.Player)
```

If you were making a space game, you might have kinds like this:

```js
namespace SpriteKind {
    export const Ship = SpriteKind.create()
    export const Planet = SpriteKind.create()
    export const Asteroid = SpriteKind.create()
    export const moon = SpriteKind.create()
}
```

During the game, if you need a sprite to change to a different kind, say a Planet becomes an Asteroid, you just set the new kind with setKind.

**_Examples_**

**1. Change kind of sprites.**

Make a Player sprite and two Friend sprites. Every 2 seconds, turn one of the friend sprites into an Enemy. Every 10 seconds have the enemy sprites become friends again.

```js
namespace SpriteKind {
    export const Friend = SpriteKind.create()
}
let player: Sprite = null
let friend2: Sprite = null
let friend1: Sprite = null
let greenBlock: Image = null
greenBlock = image.create(16, 16)
greenBlock.fill(7)
player = sprites.create(greenBlock, SpriteKind.Player)
friend1 = sprites.create(greenBlock, SpriteKind.Friend)
friend1.x = scene.screenWidth() / 4
friend2 = sprites.create(greenBlock, SpriteKind.Friend)
friend2.x = scene.screenWidth() * 3 / 4
game.onUpdateInterval(10000, function () {
    friend1.setKind(SpriteKind.Friend)
    friend2.setKind(SpriteKind.Friend)
})
game.onUpdateInterval(2000, function () {
    if (friend1.kind() == SpriteKind.Friend) {
        friend1.setKind(SpriteKind.Enemy)
    } else if (friend2.kind() == SpriteKind.Friend) {
        friend2.setKind(SpriteKind.Enemy)
    }
})
```

### kind

Get the kind of the sprite.

```js
(method) Sprite.kind(): number;
```

To keep track of different types of sprites, you can assign a kind to them. This is a value that will help identify them and decide what actions to take when events happen in the game. There are no particular rules or names for how you decide on what the kinds should be. The sprite kinds in your game might be defined like this:

```js
namespace SpriteKind {
    export const Player = SpriteKind.create()
    export const Enemy = SpriteKind.create()
}
```

**_Returns_**

a number value that is the current sprite kind.

### setBounceOnWall

Set a sprite to bounce away from a wall tiles or the edge of the screen.

```js
(method) Sprite.setBounceOnWall(on: boolean): void;
```

Setting a sprite to bounce on a wall makes the sprite to bounce away from a wall tile when it contacts it. The sprite will also bounce back from the edge of the scene’s tilemap. If there is no tilemap defined for the scene, the sprite will bounce off the edges of screen and remain in the screen view.

**_Parameters_**

**on:** a boolean value to set the bounce on wall flag for the sprite. A true value means set to ON and a false value means OFF.

**_Examples_**

**1. Bounce on wall tiles**

Set a sprite to bounce away from a wall of tiles.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setBounceOnWall(true)
tiles.setTilemap(tilemap`level`)
mySprite.vx = 50
mySprite.vy = 50
```

**2. Bounce on the the edge of the screen**

With a scene that has no tilemap set, make a sprite bounce on the edges of the screen.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setBounceOnWall(true)
mySprite.vx = 50
mySprite.vy = 50
```

### setStayInScreen

Set a sprite to always stay in the screen view.

```js
(method) Sprite.setStayInScreen(on: boolean): void;
```

You can make your sprite always remain in the screen view. This sets a sprite flag to keep the sprite on screen even when it’s position or movement would case it to move outside of these screen view.

**_Parameters_**

- **on:** a boolean value to set the stay in screen flag for the sprite. A true value means set to ON and a false value means OFF.

**_Examples_**

**1. Pass through screen.**

Set a sprite to appear on the screen even though is initial position is off the screen and its motion will send it past the screen edge.

```js
let mySprite = sprites.create(img`
    . . . . . 2 2 2 2 2 2 . . . . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . 2 2 2 2 2 2 2 2 2 2 2 2 2 2 . 
    . . 2 2 2 2 2 2 2 2 2 2 2 2 . . 
    . . . 2 2 2 2 2 2 2 2 2 2 . . . 
    . . . . . 2 2 2 2 2 2 . . . . . 
    `, SpriteKind.Player)
mySprite.setFlag(SpriteFlag.StayInScreen, true)
pause(500)
mySprite.left = -20
mySprite.top = -50
pause(500)
mySprite.vx = 30
```

### setScale

Set the scale factor for a sprite based on an anchor point.

```js
(method) Sprite.setScale(value: number, anchor: ScaleAnchor): void;
```

You can increase or decrease a sprite from it’s current size to a new size by setting a scale value. If a sprite’s scale is set to 2, then the sprite’s new size is two times larger than it’s original size. The sprite’s size will shrink if you give a negative value. If you use -3 for the scale value, the sprite’s new size will three times smaller than it’s original size.

The anchor determines which direction the sprite will expand or shrink. If you increase the scale from the middle, the sprite will grow equally from it’s center. If you choose to scale from the bottom left, the sprite will grow toward the top right of the screen.

**_Parameters_**

**value:** a number that is the scale factor for the sprite.
**anchor:** an anchor point to scale the sprite from:
  - middle
  - top
  - left
  - right
  - bottom
  - top left
  - top right
  - bottom right
  - bottom left

**_Examples_**

**1. Scale in 4 directions**

Create a sprite with a square image. Make the sprite grow and shrink by 3 in each of the 4 corner directions.

```js
let scaleZone = 0
let mySprite = sprites.create(img`
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 5 5 5 7 7 7 7 7 7 7 7 5 5 5 2 
    2 5 5 7 7 7 7 7 7 7 7 7 7 5 5 2 
    2 5 7 7 7 7 7 7 7 7 7 7 7 7 5 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 8 8 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 8 8 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 5 7 7 7 7 7 7 7 7 7 7 7 7 5 2 
    2 5 5 7 7 7 7 7 7 7 7 7 7 5 5 2 
    2 5 5 5 7 7 7 7 7 7 7 7 5 5 5 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    `, SpriteKind.Player)
game.onUpdateInterval(500, function () {
    if (scaleZone == 0) {
        mySprite.setScale(3, ScaleAnchor.TopLeft)
    } else if (scaleZone == 1) {
        mySprite.setScale(1, ScaleAnchor.TopLeft)
    } else if (scaleZone == 2) {
        mySprite.setScale(3, ScaleAnchor.TopRight)
    } else if (scaleZone == 3) {
        mySprite.setScale(1, ScaleAnchor.TopRight)
    } else if (scaleZone == 4) {
        mySprite.setScale(3, ScaleAnchor.BottomRight)
    } else if (scaleZone == 5) {
        mySprite.setScale(1, ScaleAnchor.BottomRight)
    } else if (scaleZone == 6) {
        mySprite.setScale(3, ScaleAnchor.BottomLeft)
    } else if (scaleZone == 7) {
        mySprite.setScale(1, ScaleAnchor.BottomLeft)
    } else {
        scaleZone = -1
    }
    scaleZone += 1
})
```

### changeScale

Change the scale factor for a sprite by an increment or decrement.

```js
(method) Sprite.changeScale(value: number, anchor: ScaleAnchor): void;
```

You can increase or decrease a sprite’s size from it’s current size by a certain amount. If a sprite’s scale is 2 and you change it by 1, then the sprite is now 3 times larger than it’s original size. The sprite’s size will shrink if you give a negative value. If you use -2 for the change value, the sprite’s new size will be half of what is was before.

The anchor determines which direction the sprite will expand or shrink. If you increase the scale from the middle, the sprite will grow equally from it’s center. If you choose to scale from the bottom left, the sprite will grow toward the top right of the screen.

**_Parameters_**

- **value:** a number to change the sprite’s scale by.
- **anchor:** an anchor point to scale the sprite from:
  - middle
  - top
  - left
  - right
  - bottom
  - top left
  - top right
  - bottom right
  - bottom left

**_Examples_**

**1. Rescale sprite**

Create a sprite with a square image. In a game update event, make the sprite grow and shrink continuously.

```js
let scaleFactor = 0
let mySprite = sprites.create(img`
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    2 5 5 5 7 7 7 7 7 7 7 7 5 5 5 2 
    2 5 5 7 7 7 7 7 7 7 7 7 7 5 5 2 
    2 5 7 7 7 7 7 7 7 7 7 7 7 7 5 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 8 8 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 8 8 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 7 7 7 7 7 7 7 7 7 7 7 7 7 7 2 
    2 5 7 7 7 7 7 7 7 7 7 7 7 7 5 2 
    2 5 5 7 7 7 7 7 7 7 7 7 7 5 5 2 
    2 5 5 5 7 7 7 7 7 7 7 7 5 5 5 2 
    2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 
    `, SpriteKind.Player)
game.onUpdateInterval(500, function () {
    if (mySprite.scale == 1) {
        scaleFactor = 1
    } else if (mySprite.scale == 7) {
        scaleFactor = -1
    }
    mySprite.changeScale(scaleFactor, ScaleAnchor.Middle)
})
```


## Sprite effects

### startEffect

Start a built-in effect at a sprite.

```js
(method) Sprite.startEffect(effect: effects.ParticleEffect, duration: number): void;
```

There are several built-in particle motion effects you can put on a sprite. In the Blocks editor, the effect is chosen from the list shown on the block. In JavaScript, choose one of the effects that are contained in the effects namespace. The effect will run continuously unless you give a value for the duration. You can stop the effect by using the clearParticles function.

**_Parameters_**

- **effect:** the effect to start at the sprite.
- **duration:** a number that is an optional amount of time, in milliseconds, for the effect to run. If you don’t give a value for the duration, the effect will continues to run.

**_Examples_**

**1. Cool radial effect.**

Start a 5 second cool radial spray effect on the blob sprite.

```js
let blobject: Sprite = null
blobject = sprites.create(img`
    . a a a a .
    . a a a a .
    a a a a a a
    a a a a a a
    . a a a a .
    . a a a a .
`, 0)
blobject.startEffect(effects.coolRadial, 5000)
```

### clearParticles

Stop the particle effect currently displaying at a sprite.

```js
function effects.clearParticles(anchor: Sprite | particles.ParticleAnchor): void;
```

Particle effects, when started, are set for a sprite. You can stop the effect early, if a duration was set, or stop it from displaying continuously if no duration was set.

**_Parameters_**

- **anchor:** the sprite that the effect is displaying at.

**_Examples_**

**1. Spray the confetti**

Start a confetti effect on the blob sprite for 5 seconds but then stop it after just 2 seconds.

```js
let blobject: Sprite = null
blobject = sprites.create(img`
    . a a a a .
    . a a a a .
    a a a a a a
    a a a a a a
    . a a a a .
    . a a a a .
`, 0)
blobject.startEffect(effects.confetti, 5000)
pause(2000)
effects.clearParticles(blobject)
```

**2. Campfire**

Show a fire effect coming from a sprite image of firewood. When button A is pressed, the campfire goes out and the blizzard with the bubble effect simulates the fire smoldering.

```js
let logs: Sprite = null
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    effects.clearParticles(logs)
    logs.startEffect(effects.blizzard, 750)
    logs.startEffect(effects.bubbles, 2000)
})
logs = sprites.create(img`
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . e . . .
    . . e e . . . . . . . e e . . .
    . . . e e e . . . e e d e . . .
    . . . e d e e e . e d e e . . .
    . . . e e d d e e d d e . . . .
    . . . . . e e d d e e . . . . .
    . . . e e d e e d d e . . . . .
    . . e e d d e e e d d e e . . .
    . . e d d e e . e e e d e e . .
    . . e e e . . . . . e e e . . .
    . . . . . . . . . . . . . . . .
`, 0)
logs.startEffect(effects.fire)
```

**3. Immediately clear all particles**

The clear particles block gets rid of the sources, but if you want to immediately get rid of all particles on the screen you need to also call particles.clearAll(), which is currently only available in text coding.

```js
let logs: Sprite = null
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    effects.clearParticles(logs)
    logs.startEffect(effects.blizzard, 750)
    logs.startEffect(effects.bubbles, 2000)
})

controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    effects.clearParticles(logs)
    particles.clearAll()
})
logs = sprites.create(img`
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . e . . .
    . . e e . . . . . . . e e . . .
    . . . e e e . . . e e d e . . .
    . . . e d e e e . e d e e . . .
    . . . e e d d e e d d e . . . .
    . . . . . e e d d e e . . . . .
    . . . e e d e e d d e . . . . .
    . . e e d d e e e d d e e . . .
    . . e d d e e . e e e d e e . .
    . . e e e . . . . . e e e . . .
    . . . . . . . . . . . . . . . .
`, 0)
logs.startEffect(effects.fire)
```


## Sprite events

### onCreated

Run some code when a sprite is created.

```js
function sprites.onCreated(kind: number, handler: (sprite: Sprite) => void): void;
```

Sprites are created by using the create and create projectile functions. You can respond to a sprite create event and run some code when it happens.

Sprites have three types: player, food, coin. You can track the creation of any sprite with a particular type by using the sprite object parameter called sprite.

```js
sprites.onCreated(SpriteKind.Player, function (sprite) {
    sprite.say("Hello, I'm new here")
})
let smiley = sprites.create(img`
    . . . . . f f f f f f f . . . .
    . . . f f e e e e e e e f . . .
    . . f e e e e e e e e e e f . .
    . f e e e e e e e e e e e e f .
    f e e e e f f e e e f f e e e f
    f e e e e f f e e e f f e e e f
    f e e e e e e e e e e e e e e f
    f e e e e e e e e e e e e e e f
    f e e e e e e e f e e e e e e f
    f e e e e e e e e e e e e e e f
    f e e e e f e e e e e f e e e f
    f e e e e e f f f f f e e e e f
    . f e e e e e e e e e e e e f .
    . . f e e e e e e e e e e f . .
    . . . f f e e e e e e e f . . .
    . . . . . f f f f f f f . . . .
    `, SpriteKind.Player)
```

**_Parameters_**

- **type:** the type of sprite to watch for a create event on. These are player, food, and coin.
- **sprite:** the sprite to watch for a create event on. Use sprite for a destroy event on all sprites of a certain type.
- **handler:** the code to run when the sprite is created.

**_Examples_**

**1. Observe creation of a sprite.**

Create a player sprite as an “observer” for the creation and destruction of a food sprite. Have the observer say something when some food is created and when it’s created.

```js
sprites.onDestroyed(SpriteKind.Food, function (sprite) {
    viewer.say("food's gone!!", 1000)
})
sprites.onCreated(SpriteKind.Food, function (sprite) {
    viewer.say("I see food!", 1000)
})
let viewer: Sprite = null
let playerBlock = image.create(16, 16)
playerBlock.fill(7)
viewer = sprites.create(playerBlock, SpriteKind.Player)
viewer.setPosition(8, 32)
let foodBlock = image.create(32, 32)
foodBlock.fill(8)
pause(1000)
let snack = sprites.create(foodBlock, SpriteKind.Food)
snack.say("new food here")
pause(2000)
snack.destroy()
```

### onDestroyed

Run some code when a sprite of a certain kind is destroyed.

```js
function sprites.onDestroyed(kind: number, handler: (sprite: Sprite) => void): void;
```

**_Parameters_**

- **kind**: the type of sprite to wait for a destroy event from.
- **handler**: the code to run when the sprite is destroyed.

**_Examples_**

**1. End game when sprite is destroyed.**

Create a Ghost sprite and set it’s lifespan to 1500. You win the game when the ghost is destroyed.

```js
namespace SpriteKind {
    export const Ghost = SpriteKind.create()
}
let ghost: Sprite = null
ghost = sprites.create(img`
. . . . . . d d d d d . . . . . 
. . . d d d d 1 1 1 d d d . . . 
. . d d 1 1 1 1 1 1 1 1 d d . . 
. . d 1 1 1 1 1 1 1 1 1 1 d . . 
. . d 1 1 1 1 1 1 1 1 1 1 d d . 
. d d 1 1 1 f 1 1 1 f 1 1 1 d . 
. d 1 1 1 1 1 1 1 1 1 1 1 1 d d 
. d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
. d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
d d 1 1 1 1 1 1 f f 1 1 1 1 1 d 
d 1 1 1 1 1 1 1 f f 1 1 1 1 1 d 
d 1 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
d 1 1 1 1 1 1 1 d 1 1 1 1 1 1 d 
d 1 d d d 1 1 d d d d 1 d 1 1 d 
d d d . d d d d . . d d d d d d 
d d . . . d d . . . . d . . d d 
`, SpriteKind.Ghost)
ghost.setFlag(SpriteFlag.Ghost, true)
ghost.lifespan = 1500
sprites.onDestroyed(SpriteKind.Ghost, function (sprite) {
    game.gameOver(true)
})
```

### onOverlap

Run some code when a sprite of a certain kind overlaps another sprite.

```js
function sprites.onOverlap(kind: number, otherKind: number, handler: (sprite: Sprite, otherSprite: Sprite) => void): void;
```

An overlap with a sprite of a different or the same kind is detected. If you want to know when a Player sprite overlaps an Enemy sprite, you set the first sprite kind to Player and the second sprite kind in otherKind to Enemy. You can detect overlap with two player sprites, for example, by setting both kind and otherKind to Player.

When an overlap is detected the sprite of the first kind is given to you in the sprite parameter of handler. The sprite for the second kind is in otherSprite.

An overlap of two sprites is detected when the first non-transparent pixel in the image of the first sprite overlaps the first non-transparent pixel of the second sprite. If a sprite has it’s ghost flag set, any overlap with another sprite won’t be noticed. Also, an overlap occurs even when the values of Z for the sprites are different.

**_Parameters_**

- **kind:** the first type of sprite to check for overlap.
- **otherKind:** the second type of sprite to check for overlap.
- **handler:** the code to run when the sprites overlap.
- **sprite:** the first overlapping sprite.
- **otherSprite:** the second overlapping sprite.

**_Examples_**

**1. Ghost sprite.**

Create a Ghost sprite that is blasted by yellow balls. Let the balls go through the sprite until it’s ghost status is removed by pressing the A button. When the Ghost sprite is exposed, any contact with the balls is detected in on overlap. Make the balls push the Ghost sprite off the screen.

```js
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    ghost.setFlag(SpriteFlag.Ghost, false)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    otherSprite.setVelocity(-50, 50)
    sprite.sayText("Ouch!", 200, false)
    sprite.setPosition(sprite.x + 2, sprite.y + 2)
})
sprites.onDestroyed(SpriteKind.Player, function (sprite) {
    game.gameOver(false)
})
let projectile: Sprite = null
let ghost: Sprite = null
ghost = sprites.create(img`
    . . . . . . d d d d d . . . . . 
    . . . d d d d 1 1 1 d d d . . . 
    . . d d 1 1 1 1 1 1 1 1 d d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d . . 
    . . d 1 1 1 1 1 1 1 1 1 1 d d . 
    . d d 1 1 1 f 1 1 1 f 1 1 1 d . 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 d d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    . d 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d d 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 f f 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 1 1 1 1 1 1 1 d 
    d 1 1 1 1 1 1 1 d 1 1 1 1 1 1 d 
    d 1 d d d 1 1 d d d d 1 d 1 1 d 
    d d d . d d d d . . d d d d d d 
    d d . . . d d . . . . d . . d d 
    `, SpriteKind.Player)
ghost.setPosition(60, 60)
ghost.setFlag(SpriteFlag.AutoDestroy, true)
ghost.setFlag(SpriteFlag.Ghost, true)
game.onUpdateInterval(1000, function () {
    projectile = sprites.createProjectileFromSide(img`
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . 5 5 . . . . . . . 
        . . . . . . 5 5 5 5 . . . . . . 
        . . . . . 5 5 5 5 5 5 . . . . . 
        . . . . . 5 5 5 5 5 5 . . . . . 
        . . . . . . 5 5 5 5 . . . . . . 
        . . . . . . . 5 5 . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        `, 50, 50)
})
```


## Sprite properties

### Position

#### x (property)

Get or set the horizontal center position of a sprite on the screen.

#### y (property)

Get or set the vertical center position of a sprite on the screen.

#### z (property)

Get or set the depth level (Z-order) of a sprite on the screen.

**Sprite depth**

The sprite image is a two-dimensional rectangle. You can assign a depth level to a sprite so that it will appear above or below sprites that are at different depth levels. This gives a third dimension to sprites even though they aren’t really solid objects. The depth level is called the Z-order referring to the third dimension of z.
 
Sprites have a depth of 0 when they are created. Sprites at the same depth level will have their pixels overlap based on when they were created. Sprites created later will overlap the sprites created earlier when they are in the same level. To control the overlap of sprites, you can assign them to different levels in the Z-order. Sprites with a higher value for z will overlap the sprites with a lower z value.

#### left (property)

Get or set the left position of the sprite on the screen.

#### right (property)

Get or set the right position of the sprite on the screen.

#### top (property)

Get or set the top position of the sprite on the screen.

#### bottom (property)

Get or set the bottom position of the sprite on the screen.

### Physics

#### vx (property)

Get or set the horizontal speed of motion for a sprite in pixels per second.


 **Sprite motion**

A sprite that isn’t a projectile has no motion when it’s created. You make the sprite move by setting it’s speed, or velocity, in the x direction, the y direction, or both. For the x direction, setting the speed to a positive value makes the sprite move to the right. To make the sprite move to the left, you use a negative speed value.
 
If you don’t follow the sprite with the camera or check for when the sprite reaches the end of the screen, the sprite will move off the screen.

> ℹ️ **What is speed, really?**
> 
> Speed, or velocity, is how much distance an object moves during some period of time. A car can travel 50 kilometers in one hour so it moves at 50 kilometers per hour (50 km/h). A jet airplane can fly as fast as 913 k/mh and travel across some continents in 3 or 4 hours.
> 
> Distance in your game is measured in pixels so the speed of a sprite is in pixels per second. If the screen is 160 pixels wide, then a sprite with a speed of 40 pixels per second will move across the screen in 4 seconds.

#### vy (property)

Get or set the vertical speed of motion for a sprite in pixels per second.

#### ax (property)

Get or set the horizontal acceleration for a sprite in pixels per second, per second.

**Sprite horizontal acceleration**

The value for acceleration determines how quickly the speed of the sprite will change. Acceleration of a sprite makes it speed up in a direction toward the right or left. A positive value causes the sprite’s speed in the right direction to increase. A negative value causes the sprite’s speed in the left direction to increase.

Acceleration can be used as an opposing force too. If a sprite is travelling toward the right at a certain speed and a negative horizontal acceleration is applied, the sprite will keep moving to the right but it will slow down. When the sprite’s speed reaches 0, the sprite will begin to move to the left.

> ℹ️ **How speed changes**
>
> Speed, or velocity, is how much distance an object moves during some period of time. Distance in your game is measured in pixels so the speed of a sprite is in pixels per second. Speed is changed by accelerating an object. In your game, a sprite is accelerated by some amount of change in speed per second. So, acceleration is measured in pixels per second per second.
>
> If the speed of a sprite is currently at 0 and it’s acceleration is set to 2, the sprite will be moving at a speed of 2 pixels per second after one second of time. After two seconds the sprite is moving at 4 pixels per second and so on.

#### ay (property)

Get or set the vertical acceleration for a sprite in pixels per second, per second.

#### fx (property)

Get or set the friction opposing a sprite’s motion in the horizontal direction.

**Sprite horizontal friction**

Friction is an opposing force against the motion of a sprite. If a sprite has a horizontal velocity (vx), it’s friction will slow the sprite down until it’s horizontal velocity becomes 0. This is similar to setting an opposite horizontal acceleration but the opposite acceleration goes away once the sprite stops.

#### fy (property)

Get or set the friction opposing a sprite’s motion in the vertical direction.

### Image and Attributes

#### image (property)

Get the sprite’s current image.

#### width (property)

Get the width of the sprite in pixels.

**Sprite sizes**

The sprite size matches the size of the image it contains. The size of the sprite only changes when it’s image changes. The sprite width is also the same as the difference between the left and right sides.

#### height (property)

Get the height of the sprite in pixels.

#### lifespan (property)

Get or set the lifespan of sprite in milliseconds.

**Sprite lifespan**

Sprites that aren’t set to auto destroy will remain in the game until they are destroyed by calling their destroy function or until their lifespan expires. You must set the lifespan to give a sprite a limited amount of time live. You set the lifespan of sprite to make it leave the game after some amount of time.

The lifespan of a sprite is infinite when it’s created and stays that way until you actually set it to a number. Once you set it, the lifespan begins to count down to 0. You can reset the lifespan value to keep a sprite alive longer as a bonus to your player.

### Scaling

#### sx (property)

Get or set the scaling factor for the width of a sprite.

#### sy (property)

Get or set the scaling factor for the height of a sprite.

#### scale (property)

Get or set the scaling factor for the width and height of a sprite.
