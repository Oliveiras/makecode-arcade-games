# MakeCode Arcade / API Reference Guide / Game

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Game** module.

This module controls and text display actions.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Game control

### gameOver

Finish the game and show the score.

```js
function game.gameOver(win: boolean): void;
```

When you end the game, all game control stops and the player gets a message that the game is over (finished). The last action is frozen on the screen and both the current and highest scores are displayed. The game program will reset when you press a key or button.

If a game over message set for a win or lose, it is displayed. If a game over effect is set, then it also is shown and a game over sound if one is also set. If no message, effect, or sound is set for either win or lose, then a default is used.

**_Parameters_**

- **win:** an optional boolean value to say if the player has won the game.

**_Examples_**

**1. No lives? Game Over**

Check every second to see if the player has any lives left. If not, end the game!

```js
game.onUpdateInterval(1000, function () {
    if (info.life() < 1) {
        game.setGameOverEffect(false, effects.dissolve)
        game.gameOver(false)
    }
})
```

**2. Show Game Over effect**

When the game score reaches 20, end the game and show the bubbles effect.

```js
game.onUpdateInterval(100, function () {
    info.changeScoreBy(1)
})
game.onUpdate(function () {
    if (info.score() > 20) {
        game.setGameOverEffect(false, effects.bubbles)
        game.gameOver(false)
    }
})
```

### setGameOverEffect

Set an effect to display when the game is over.

```js
function game.setGameOverEffect(win: boolean, effect: effects.BackgroundEffect): void;
```

**_Parameters_**

- **win:** a boolean value set to true to start the effect if the player wins the game. Set to false to start the effect if the player loses.
- **effect:** the effect to display when the game is over, such as confettti, hearts, or smiles.

**_Examples_**

**1. Game over on touch.**

Make the game over when the kitten sprite touches the left of the screen. Show the splatter effect when the game is over.

```js
game.setGameOverEffect(true, effects.splatter)
let mySprite = sprites.create(img`
    . . . . . . . . . . . . . . 
    e e e . . . . e e e . . . . 
    c d d c . . c d d c . . . . 
    c b d d f f d d b c . . . . 
    c 3 b d d b d b 3 c . . . . 
    f b 3 d d d d 3 b f . . . . 
    e d d d d d d d d e . . . . 
    e d f d d d d f d e . b f b 
    f d d f d d f d d f . f d f 
    f b d d b b d d 2 b f f d f 
    . f 2 2 2 2 2 2 d b b d b f 
    . f d d d d d d d f f f f . 
    . . f d b d f d f . . . . . 
    . . . f f f f f f . . . . . 
    `, SpriteKind.Player)
mySprite.vx = -20
game.onUpdateInterval(500, function () {
    if (mySprite.left < 0) {
        game.gameOver(true)
    }
})
```

### setGameOverMessage

Set a message to display when the game is over.

```js
function game.setGameOverMessage(win: boolean, message: string): void;
```

**_Parameters_**

- **win:** a boolean value set to true to start the sound if the player wins the game. Set to false to start the sound if the player loses.
- **message:** a message string to show when the game is over.

### setGameOverPlayable

Set a sound to play when the game is over.

```js
function game.setGameOverPlayable(win: boolean, sound: music.Playable, looping: boolean): void;
```

**_Parameters_**

- **win:** a boolean value set to true to start the sound if the player wins the game. Set to false to start the sound if the player loses.
- **sound:** the sound to play when the game is over, such as ba ding, siren, or buzzer.
- **looping:** loop the sound when set to true. Play the sound only once if false.

**_Examples_**

**1. Play sound when player wins the game.**

Make the game over when the kitten sprite touches the left of the screen. Play the siren sound when the player wins the game.

```js
game.setGameOverPlayable(true, music.melodyPlayable(music.siren), false)
let mySprite = sprites.create(img`
    . . . . . . . . . . . . . . 
    e e e . . . . e e e . . . . 
    c d d c . . c d d c . . . . 
    c b d d f f d d b c . . . . 
    c 3 b d d b d b 3 c . . . . 
    f b 3 d d d d 3 b f . . . . 
    e d d d d d d d d e . . . . 
    e d f d d d d f d e . b f b 
    f d d f d d f d d f . f d f 
    f b d d b b d d 2 b f f d f 
    . f 2 2 2 2 2 2 d b b d b f 
    . f d d d d d d d f f f f . 
    . . f d b d f d f . . . . . 
    . . . f f f f f f . . . . . 
    `, SpriteKind.Player)
mySprite.vx = -20
game.onUpdateInterval(500, function () {
    if (mySprite.left < 0) {
        game.gameOver(true)
    }
})
```

### setGameOverScoringType

Set the scoring type to decide the best game score for multiple games.

```js
function game.setGameOverScoringType(type: game.ScoringType): void;
```

When mulitple games are played with your game program, you can set the method to record the best game score. You can use the highest score, lowest score, or no score as the best game score.

**_Parameters_**

- **type:** the type of score used to record the best game score for multiple games:
- **highest:** use the highest score as the best game score.
- **lowest:** use the lowest score as the best game score.
- **none:** don’t record any score as the best score.

**_Examples_**

**1. Best score is the lowest.**

Set the best score type to lowest. Move the player sprite up or down to avoid the projectiles. If your player is hit by projectiles, your score is increased. The lowest score is recorded for all of the games you play.

```js
info.onCountdownEnd(function () {
    game.gameOver(true)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    info.changeScoreBy(1)
})
let projectile: Sprite = null
game.setGameOverScoringType(game.ScoringType.LowScore)
let mySprite = sprites.create(img`
    . . . . . . . . . . b 5 b . . . 
    . . . . . . . . . b 5 b . . . . 
    . . . . . . . . . b c . . . . . 
    . . . . . . b b b b b b . . . . 
    . . . . . b b 5 5 5 5 5 b . . . 
    . . . . b b 5 d 1 f 5 5 d f . . 
    . . . . b 5 5 1 f f 5 d 4 c . . 
    . . . . b 5 5 d f b d d 4 4 . . 
    b d d d b b d 5 5 5 4 4 4 4 4 b 
    b b d 5 5 5 b 5 5 4 4 4 4 4 b . 
    b d c 5 5 5 5 d 5 5 5 5 5 b . . 
    c d d c d 5 5 b 5 5 5 5 5 5 b . 
    c b d d c c b 5 5 5 5 5 5 5 b . 
    . c d d d d d d 5 5 5 5 5 d b . 
    . . c b d d d d d 5 5 5 b b . . 
    . . . c c c c c c c c b b . . . 
    `, SpriteKind.Player)
mySprite.left = 10
controller.moveSprite(mySprite, 0, 100)
info.startCountdown(10)
game.onUpdateInterval(500, function () {
    projectile = sprites.createProjectileFromSide(img`
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . 2 2 . . . . . . . 
        . . . . . . 3 1 1 3 . . . . . . 
        . . . . . 2 1 1 1 1 2 . . . . . 
        . . . . . 2 1 1 1 1 2 . . . . . 
        . . . . . . 3 1 1 3 . . . . . . 
        . . . . . . . 2 2 . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        `, -60, randint(-30, 30))
})
```

### reset

Reset the game.

```js
function game.reset(): void;
```

Resetting the game makes the game program stop. The game program is then restarted. If the game is running on a device, this is similar to the reset button

### runtime

Get the time in milliseconds since the game was started.

```js
function game.runtime(): number;
```

**_Returns_**

a number which is the amount of time in millseconds since the game started.


## Game events

### onUpdate

Run game update code.

```js
function game.onUpdate(fn: () => void): void;
```

There are events available to run code when sprites overlap, collide, are created, or destroyed. Also, you can use events to take action when buttons are pressed or when game counts reach zero. When you want to have code to control what happens in a game on a regular basis though, you need to run that code in an update function.

Your program works with the game engine using an update function. The update function is called by the game engine at a regular interval. Inside the update function, you might put in code that checks positions of sprites, conditions that change the score, adjust the life count, or maybe if something happened to end the game.

**_The Game Loop_**

One of the basic parts of game programming is the use of a game loop. This is part of the game engine in Arcade. A game loop keeps the actions in the game moving along. As simple code it might look like this:

```js
while (!gameOver) {
    checkInputs()
    gameUpdate()
    showGameUpdates()
}
```

As part of gameUpdate, the code inside your onUpdate is run to handle the regular updates that are needed by your game. The time between updates (update frequency) is set by the game engine. This interval is often enough to update the game so that the player is never waiting for your program to make its updates. You put code inside an onUpdate that needs to run regularly but doesn’t depend on a fixed time interval. If you want update code to run on an interval of time that you choose, then use onUpdateInterval.

**_Parameters_**

- **fn:** the code to run to move or change the sprites.

**_Example_**

**1. Check sprite position on every update.**

Create a sprite and start it moving. In the onUpdate function, check if the sprite reaches the edge of the screen. If it does, send it in the opposite direction.

```js
let mySprite: Sprite = null
mySprite = sprites.create(img`
. . . . . 5 5 5 5 5 5 . . . . . 
. . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
. 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
. 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
. 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 
. 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
. 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
. 5 5 5 5 5 5 5 5 5 5 5 5 5 5 . 
. . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
. . . . . 5 5 5 5 5 5 . . . . . 
`, SpriteKind.Player)
mySprite.vx = 30
game.onUpdate(function () {
    if (mySprite.x > scene.screenWidth()) {
        mySprite.vx = -30
    } else if (mySprite.x < 0) {
        mySprite.vx = 30
    }
})
```

### onUpdateInterval

Run some code each time an interval of time elapses.

```js
function game.onUpdateInterval(period: number, fn: () => void): void;
```

There are events available to run code when sprites overlap, collide, are created, or destroyed. Also, you can use events to take action when buttons are pressed or when game counts reach zero. When you want to have code to control what happens in a game on a regular basis though, you need to run that code in an update function.

Your program works with the game engine using an update function. The update function is called by the game engine at an interval of time that you choose. Inside the update function, you might put in code that checks positions of sprites, conditions that change the score, adjust the life count, or maybe if something happened to end the game.

With onUpdateInterval, you decide how often you want your update code to run. This is different from onUpdate where the game engine decides when to run your update code.

If your program has both onUpdate and onUpdateInterval functions, the code in your onUpdateInterval function will run first.

**_Parameters_**

- **period:** a number of milliseconds to wait until you want the interval code to run.
- **fn:** the code to run when the time in period has elapsed

**_Examples_**

**1. Check lives every second.**

Check every second to see if the player has any lives left. If not, end the game!

```js
game.onUpdateInterval(1000, function () {
    if (info.life() < 1) {
        game.gameOver(false)
    }
})
```


## Text input and output

### ask

Ask the player a ‘yes’ or ‘no’ question which answered with the A or B button.

```js
function game.ask(title: any, subtitle: any): boolean;
```

You ask a question which the player answers by pressing either the A or B button. If the player presses A, a true condition results. If the B button is pressed, the condition is false. You can include an optional subtitle as a second part of your question.

**_Parameters_**

- **title:** a string that has the text of your question.
- **subtitle:** a string that is an optional second part of your question.

**_Returns_**

a boolean value that is true if the player pressed A in response to your question and false if the player pressed B.

**_Examples_**

**1. Ask the player before continuing.**

Ask the player if they’re ready to play the next game level. If so, set the score to 0. Otherwise, finish the game.

```js
if (game.ask("Ready to play?", "Next: advanced level")) {
    info.setScore(0)
} else {
    game.gameOver(false)
}
```

### askForString

Ask the player for a string value.

```js
function game.askForString(message: any, answerLength: number, useOnScreenKeyboard: boolean): string;
```

The player is prompted to input a string. Your message is displayed and the player chooses from character selections below it to make a response. Each character of the response string is selected with the A button. You can limit the number of characters the player can type in the response.

The player selects OK and presses the A button to finish typing the response. The response string is then returned to your program.

**_Parameters_**

- **message:** a string that is the prompt message to ask for a response.
- **answerLength:** the number of characters you want let the player enter. The maximum you can set is 24 and the default is 12.

**_Returns_**

a string value that is the response typed by the player.

**_Examples_**

**1. Ask the player for a code.**

Ask the player for a unlock code to continue playing the game.

```js
let unlock = "98745"
if (game.askForString("unlock code:", 5) != unlock) {
    game.gameOver(false)
}
```

### splash

Show a title and a subtitle on the screen.

```js
function game.splash(title: any, subtitle: any): void;
```

You can splash a title on the screen at the beginning of your game or at sometime later in the game. To show more information, add an optional subtitle string. The splash text goes away when you press a key or button.

**_Parameters_**

- **title:** a string that is your splash text.
- **subtitle:** a string that is an optional second line of splash text.

### showLongText

Show a dialog window with a longer amount of text.

```js
function game.showLongText(str: any, layout: DialogLayout): void;
```

If your to give your player a long message, you can do it with a long text dialog. The dialog appears on the screen at the position you where you want it. The positions are on the sides of the screen, the center, or just the whole screen itself.

When the player is finished reading the message, the dialog is closed by pressing the A button and the game continues. If the length of the text shown in the dialog is too long to see all at once, the player can click on the dialog window to scroll it.

**_Parameters_**

- **str:** a string of text to show in the long text dialog.
- **layout:** the position of the long text dialog on the screen. The positions are:
- **left:** dialog is displayed on the left side of the screen
- **right:** dialog is displayed on the right side of the screen
- **top:** dialog is displayed on the top side of the screen
- **bottom:** dialog is displayed on the bottom side of the screen
- **center:** dialog is displayed n the center of the screen
- **full screen:** dialog is displayed using all of the screen

**_Examples_**

**1. Text on all sides**

Show a long text dialog at a different position on the screen for each of the controller buttons pressed.

```js
controller.left.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("Long text on the left", DialogLayout.Left)
})
controller.right.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("Long text on the right", DialogLayout.Right)
})
controller.up.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("Long text on the top", DialogLayout.Top)
})
controller.down.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("Long text on the bottom", DialogLayout.Bottom)
})
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("Long text in the center", DialogLayout.Center)
})
controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("Long text in full screen", DialogLayout.Full)
})
```

**2. Scroll the text**

Show a very long text string in a long text dialog when button A is pressed. Scroll the dialog to see all of the text.

```js
let veryLongText = "A very"
for (let i = 0; i < 40; i++) {
    veryLongText = veryLongText + ", very"
}
veryLongText = veryLongText + " long text string!"
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText(veryLongText, DialogLayout.Center)
})
```

### setDialogCursor

Set the image for the dialog cursor.

```js
function game.setDialogCursor(cursor: Image): void;
```

The dialog cursor is a small image that shows inside the dialog frame. It’s used to show the player which button causes the dialog to close. You can change the way the cursor looks by setting a different image for it.

You can change the frame back to the original (default) cursor by using an empty image:

**_Parameters_**

- **cursor:** an image to use as the dialog cursor.

### setDialogFrame

Set the frame image for the long text dialog window.

```js
function game.setDialogFrame(frame: Image): void;
```

A dialog frame includes the outside edge and the inside of the dialog. You make a dialog frame by creating an image that has pixels to represent the borders and the inside of the frame.

The frame image needs enough pixels to tell the dialog what the frame design should look like. The simplist frame is a image with 3 rows and 3 columns. This has enough pixels to show what colors the edges have and what color the middle has:

You can change the frame back to the original (default) frame by using an empty image:

**_Parameters_**

- **frame:** an image to use as the dialog frame.

**_Examples_**

**1. Simple frame**

Show a message in a dialog with a simple frame made from a 3 x 3 image.

```js
game.setDialogFrame(img`
3 3 3
3 9 3
3 3 3
`)
game.showLongText("A really simple frame here", DialogLayout.Center)
```

**2. Fancy frame**

Make an new dialog frame with a fancy border and a color on the inside.

```js
game.showLongText("Here's the old frame...", DialogLayout.Center)
game.setDialogFrame(img`
2 2 1 1 2 2 1 1 2 2 1 1 
2 9 9 9 9 9 9 9 9 9 9 1 
1 9 9 9 9 9 9 9 9 9 9 2 
1 9 9 9 9 9 9 9 9 9 9 2 
2 9 9 9 9 9 9 9 9 9 9 1 
2 9 9 9 9 9 9 9 9 9 9 1 
1 9 9 9 9 9 9 9 9 9 9 2 
1 9 9 9 9 9 9 9 9 9 9 2 
2 9 9 9 9 9 9 9 9 9 9 1 
2 9 9 9 9 9 9 9 9 9 9 1 
1 9 9 9 9 9 9 9 9 9 9 2 
1 1 2 2 1 1 2 2 1 1 2 2 
`)
game.showLongText("How do you like my new fancy frame?", DialogLayout.Center)
```

### setDialogTextColor

Set the color for the text that’s displayed in the long text dialog.

```js
function game.setDialogTextColor(color: number): void;
```

**_Parameters_**

- **color:** the number for the color used to display the dialog text.

**_Examples_**

**1. Set text color**

Show text with color 10 in the long text dialog.

```js
game.setDialogTextColor(10)
game.showLongText("Some long text with color!", DialogLayout.Center)
```
