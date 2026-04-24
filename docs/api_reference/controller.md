# MakeCode Arcade / API Reference Guide / Controller

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Controller** module.

This module handles game controller buttons.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Single player

### onEvent

Run some code when a button is pressed or released.

```js
(method) controller.Button.onEvent(event: ControllerButtonEvent, handler: () => void): void;
```

**_Parameters_**

- **event:** the button action to wait for. The button actions (events) are:
- **pressed:** button was pressed
- **released:** button is released from being pressed
- **repeated:** this event keeps repeating when the button is pressed
- **handler:** the code you want to run when something happens to a button

**_Examples_**

**1. Move a box.**

Move a yellow box to a random location on the screen when a button is pressed.

```js
let yellowBox = sprites.create(img`
e e e e e e
e 1 1 1 1 e
e 1 6 6 1 e
e 1 6 6 1 e
e 1 1 1 1 e
e e e e e e
`)
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    yellowBox.x = Math.randomRange(3, scene.screenWidth() - 3)
    yellowBox.y = Math.randomRange(3, scene.screenHeight() - 3)
})
```

### isPressed

Check if a button is being pressed or not.

```js
(method) controller.Button.isPressed(): boolean;
```

**_Returns_**

a boolean: true if the button is pressed, false if the button is not pressed.

**_Examples_**

**1. Move a box while button is pressed.**

Ramdomly move a yellow box around the screen while the A button is pressed.

```js
let yellowBox = sprites.create(img`
e e e e e e
e 1 1 1 1 e
e 1 6 6 1 e
e 1 6 6 1 e
e 1 1 1 1 e
e e e e e e
`)
game.onUpdate(function () {
    if (controller.A.isPressed()) {
        yellowBox.x = Math.randomRange(3, scene.screenWidth() - 3)
        yellowBox.y = Math.randomRange(3, scene.screenHeight() - 3)
    }
})
```

**2. Move a box while player 2 keeps button pressed.**

```js
let yellowBox = sprites.create(img`
e e e e e e
e 1 1 1 1 e
e 1 6 6 1 e
e 1 6 6 1 e
e 1 1 1 1 e
e e e e e e
`)
game.onUpdate(function () {
    if (controller.player2.isPressed(ControllerButton.A)) {

        yellowBox.x = Math.randomRange(3, scene.screenWidth() - 3)
        yellowBox.y = Math.randomRange(3, scene.screenHeight() - 3)
    }
})
```

### moveSprite

Control the motion of a sprite with the direction buttons.

```js
function controller.moveSprite(sprite: Sprite, vx: number, vy: number): void;
```

Instead of tracking the direction buttons in a game update function and then updating a sprite’s position, you can set a sprite to move automatically when the buttons are pressed. You just decide how fast the sprite will move in both the horizontal and vertical directions when the related buttons are pressed.

**_Parameters_**

- **sprite:** the sprite to control with the buttons.
- **vx:** a number which is the speed the sprite moves in the horizontal direction when the left or right button is pressed.
- **vy:** a number which is the speed the sprite moves in the vertical direction when the up or down button is pressed.

**_Examples_**

**1. Move sprite with buttons.**

Create a sprite with a circular image. Move the sprite around the screen with the direction buttons.

```js
let mySprite: Sprite = null
mySprite = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . 7 7 7 7 7 7 . . . . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . . . . 7 7 7 7 7 7 . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 100)
```

**2. Move sprite with player 2 buttons.**

```js
let mySprite: Sprite = null
mySprite = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . 7 7 7 7 7 7 . . . . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . 7 7 7 7 7 7 7 7 7 7 7 7 . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . . 7 7 7 7 7 7 7 7 7 7 . . . 
. . . . . 7 7 7 7 7 7 . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Player)
controller.player2.moveSprite(mySprite, 100, 100)
```

### dx

Gets the amount of horizontal movement to use if a left or right key is pressed.

```js
function controller.dx(step: number): number;
```

If you’re controlling the location of a sprite by key pressess, you can decide how much change in position it will have when a key is pressed. This is done with steps. While a direction key is pressed, the movement value returned is based on if and how long the key is pressed, along with the step size you gave it. If you want fast movement, then you use a larger step size.

If the left key is pressed, then the movement value is negative. If the right key is pressed, the movement value is positive. You can use these values to update the location of a sprite. The step value is usually a positive number. If you want to reverse directions for the keys, you can use a negative step number.

**_Parameters_**

- **step:** the amount of horizontal movement assigned to a key direction each time a key is detected as pressed.

**_Returns_**

a number that is the amount movement left or right for a pressed key.

**_Examples_**

**1. Change position of sprite.**

Move the cosmo object on the screen with the direction controller.

```js
const cosmo = sprites.create(img`
....aaaaa
...aaaaaaa
aaaaaaaaaaaaa
...aaaaaaa
....aaaaa
.....a.a
....a...a
`)
game.onUpdate(function () {
    cosmo.x += controller.dx(100)
    cosmo.y += controller.dy(80)
})
```

**2. Change position of sprite with player 2.**

Move the cosmo object on the screen with the direction controller.

```js
const cosmo = sprites.create(img`
....aaaaa
...aaaaaaa
aaaaaaaaaaaaa
...aaaaaaa
....aaaaa
.....a.a
....a...a
`)
game.onUpdate(function () {
    cosmo.x += controller.player2.dx(100)
    cosmo.y += controller.player2.dy(80)
})
```

### dy

Gets the amount of vertical movement to use if an up or down key is pressed.

```js
function controller.dy(step: number): number;
```

If you’re controlling the location of a sprite by key pressess, you can decide how much change in position it will have when a key is pressed. This is done with steps. While a direction key is pressed, the movement value returned is based on if and how long the key is pressed, along with the step size you gave it. If you want fast movement, then you use a larger step size.

If the up key is pressed, then the movement value is negative. If the down key is pressed, the movement value is positive. You can use these values to update the location of a sprite. The step value is usually a positive number. If you want to reverse directions for the keys, you can use a negative step number.

**_Parameters_**

- **step:** the amount of vertical movement assigned to a key direction each time a key is detected as pressed.

**_Returns_**

a number that is the amount movement up or down for a pressed key.


## Multiplayer

### onButtonEvent

Run some code when a button is pressed or released.

```js
(method) controller.Controller.onButtonEvent(btn: ControllerButton, event: ControllerButtonEvent, handler: () => void): void;
```

**_Parameters_**

- **event:** the button action to wait for. The button actions (events) are:
- **pressed:** button was pressed
- **released:** button is released from being pressed
- **repeated:** this event keeps repeating when the button is pressed
- **handler:** the code you want to run when something happens to a button

### moveSprite

Control the motion of a sprite with the direction buttons.

```js
function controller.moveSprite(sprite: Sprite, vx: number, vy: number): void;
```

Instead of tracking the direction buttons in a game update function and then updating a sprite’s position, you can set a sprite to move automatically when the buttons are pressed. You just decide how fast the sprite will move in both the horizontal and vertical directions when the related buttons are pressed.

**_Parameters_**

- **sprite:** the sprite to control with the buttons.
- **vx:** a number which is the speed the sprite moves in the horizontal direction when the left or right button is pressed.
- **vy:** a number which is the speed the sprite moves in the vertical direction when the up or down button is pressed.

### onEvent

Run some code when a player connects or disconnects.

```js
(method) controller.Controller.onEvent(event: ControllerEvent, handler: () => void): void;
```

**_Parameters_**

- **event:** the connection action to wait for. The connection actions (events) are:
- **connected:** player is connected
- **disconnected:** button is released from being pressed
- **handler:** the code you want to run when a player connect event happens

### isPressed

Check if a button is being pressed or not.

```js
(method) controller.Button.isPressed(): boolean;
```

**_Returns_**

a boolean: true if the button is pressed, false if the button is not pressed.

### dx

Gets the amount of horizontal movement to use if a left or right key is pressed.

```js
function controller.dx(step: number): number;
```

If you’re controlling the location of a sprite by key pressess, you can decide how much change in position it will have when a key is pressed. This is done with steps. While a direction key is pressed, the movement value returned is based on if and how long the key is pressed, along with the step size you gave it. If you want fast movement, then you use a larger step size.

If the left key is pressed, then the movement value is negative. If the right key is pressed, the movement value is positive. You can use these values to update the location of a sprite. The step value is usually a positive number. If you want to reverse directions for the keys, you can use a negative step number.

**_Parameters_**

- **step:** the amount of horizontal movement assigned to a key direction each time a key is detected as pressed.

**_Returns_**

a number that is the amount movement left or right for a pressed key.

### dy

Gets the amount of vertical movement to use if an up or down key is pressed.

```js
function controller.dy(step: number): number;
```

If you’re controlling the location of a sprite by key pressess, you can decide how much change in position it will have when a key is pressed. This is done with steps. While a direction key is pressed, the movement value returned is based on if and how long the key is pressed, along with the step size you gave it. If you want fast movement, then you use a larger step size.

If the up key is pressed, then the movement value is negative. If the down key is pressed, the movement value is positive. You can use these values to update the location of a sprite. The step value is usually a positive number. If you want to reverse directions for the keys, you can use a negative step number.

**_Parameters_**

- **step:** the amount of vertical movement assigned to a key direction each time a key is detected as pressed.

**_Returns_**

a number that is the amount movement up or down for a pressed key.


## Configuration

### Button Repeat

#### repeatInterval (property)

Get or set the interval between each occurrence of the ControllerButtonEvent.Repeated event when a button is held.

#### repeatDelay (property)

Get or set the delay before the first occurrence of the ControllerButtonEvent.Repeated event when a button is held.
