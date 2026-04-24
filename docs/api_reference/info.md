# MakeCode Arcade / API Reference Guide / Info

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Info** module.

This module keeps the score, run the countdown timer, and track life status.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Score

### setScore

Set the game score for a player to this amount.

```js
function info.setScore(value: number): void;
```

Your program has a score counter which you can set to record the current score for a game player.

**_Parameters_**

- **score:** a number to set the current score to.

**_Examples_**

**1. Give the player 110 points to before starting the game.**

```js
info.setScore(110)
```

**2. Give points to player 2, in multiplayer game.**

```js
info.player2.setScore(110)
```

### changeScoreBy

Change the player score up or down by this amount.

```js
function info.changeScoreBy(value: number): void;
```

The total score amount is increased by adding the change value when it is greater than zero (positive). If the change number is less than zero (negative), the total score is reduced by the value of the change number.

**_Parameters_**

- **value:** a number whice is the amount to change the current score by.

### score

Get the current game score for the player.

```js
function info.score(): number;
```

Your program has a score counter which you can set to record the current score for a game player.

**_Returns_**

a number that is the current game score for the player.

### highScore

Get the highest score recored for the game.

```js
function info.highScore(): number;
```

The highest score recorded by your game program is remembered. So, you can find out what the player’s best score was for the game.

**_Returns_**

a number that is the highest game score.


## Life Count

### setLife

Set the player life count to this amount.

```js
function info.setLife(value: number): void;
```

Your program has a life counter which you can set to record the number of lives remaining for a player in your game.

**_Parameters_**

- **score:** a number to set the life count to.

**_Examples_**

**1. Set the player life count to 9 lives before starting the game.**

```js
info.setLife(9)
```

**2. Set the life of player 2.**

```js
info.player2.setLife(9)
```

### changeLifeBy

Change the number of lives for the game player up or down by this amount.

```js
function info.changeLifeBy(value: number): void;
```

The number of lives is increased by adding the change value when it is greater than zero (positive). If the change number is less than zero (negative), the life count is reduced by the value of the change number.

**_Parameters_**

- **value:** a number to change the life count by. Positive numbers add life and negative numbers reduce life.

### life

Get the game player’s life count.

```js
function info.life(): number;
```

Your program has a life counter which you can set to record the number of lives remaining for a player in your game.

**_Returns_**

a number that is the current life count.

### onLifeZero

Run some code when the player life count reaches zero.

```js
function info.onLifeZero(handler: () => void): void;
```

If you’ve set a life count for your came, you can take an action when the life count reaches 0. Depending on the rules for your game, you may wish to end the game or remove a player sprite.

If you set a game life count (using setLife) and it decreases to 0 but you have no onLifeZero function in your program, the game will automatically end.

> ℹ️ Note
>
> Indiviual sprites have their own lifespan. They are destroyed if their lifespan was set and then reaches 0. This is different from the life count for the game. Game lives are awarded and removed based on your own rules for gameplay.

**_Parameters_**

- **handler:** the code to run when the life count reaches 0.

**_Examples_**

**1. Life zero message**

Set the life count to 3. In the game update function, decrease the life count by 1 each second. Show a message when the life count becomes 0.

```js
info.setLife(3)
game.onUpdateInterval(1000, function() {
    info.changeLifeBy(-1)
})
info.onLifeZero(function () {
    game.showLongText("Life is zero!", DialogLayout.Bottom)
})
```

**2. Multiplayer example**

```js
info.player2.setLife(3)
game.onUpdateInterval(1000, function() {
    info.player2.changeLifeBy(-1)
})
info.player2.onLifeZero(function () {
    game.showLongText("Life is zero!", DialogLayout.Bottom)
})
```

**3. No lives, game over**

Set the game life count to 9 lives. Run out the lives to end the game.

```js
info.setLife(9)
game.onUpdateInterval(300, function() {
    info.changeLifeBy(-1)
})
```


## Countdown Timer

### startCountdown

Start the countdown timer for the amount of gameplay time. The current game lasts for this amount of time.

```js
function info.startCountdown(duration: number): void;
```

The game time is set as a number of seconds. When the game time finishes, a game over notification happens and the current game ends.

**_Parameters_**

- **duration:** the number seconds to play the game for.

**_Examples_**

**1. Set the game time to 30 seconds.**

```js
info.startCountdown(30)
```

### stopCountdown

Stops the countdown timer but gameplay continues.

```js
function info.stopCountdown(): void;
```

When you stop the game timer the game continues but the there is no more time limit. You can’t resume the countdown from the previous time. You must start the countdown again to reset a time limit for the game.

**_Examples_**

**1. Start, stop countdown and end the game.**

Set the game time to 30 seconds. Wait 5 seconds, stop the countdown, wait 2 seconds and end the game.

```js
info.startCountdown(30)
pause(5000)
info.stopCountdown()
pause(2000)
game.gameOver(false)
```

### onCountdownEnd

Run some code when the game countdown timer reaches zero.

```js
function info.onCountdownEnd(handler: () => void): void;
```

If you want to take an action when the game’s countdown time reaches 0, you can put some code in the onCountdownEnd function. You might not want the game to end when the countdown goes to 0 but instead have the time reset or restart with different amount of time. This depends on the rules you want to set for your game.

If you’ve started a the game countdown (using startCountdown) and it decreases to 0 but you have no onCountdownEnd function in your program, the game will automatically end.

**_Parameters_**

- **handler:** the code to run when the game timer count reaches 0.

**_Examples_**

**1. Game time zero message**

Set the life count to 3. In the game update function, decrease the life count by 1 each second. Show a message when the life count becomes 0.

```js
info.startCountdown(3)
info.onCountdownEnd(function () {
    game.showLongText("Timer count is zero!", DialogLayout.Bottom)
})
```

**2. No more time, game over**

Set the game time count to 5 seconds. Run out the time and end the game.

```js
info.startCountdown(5)
```
