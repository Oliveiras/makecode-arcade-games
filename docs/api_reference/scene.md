# MakeCode Arcade / API Reference Guide / Scene

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Scene** module.

This module sets scene backgrounds and view perspective, handles interactions between tiles and sprites.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Screen Settings

### screenWidth

Get the width in pixels of the screen.

```js
function scene.screenWidth(): number;
```

**_Returns_**

a number that’s the width of the screen on the game device or simulator.

**_Examples_**

**1. Draw a wide line across the entire screen.**

```js
let showLine: Sprite = null
let wideLine: Image = null
wideLine = image.create(scene.screenWidth(), 3)
wideLine.fill(5)
showLine = sprites.create(wideLine)
```

### screenHeight

Get the height in pixels of the screen.

```js
function scene.screenWidth(): number;
```

**_Returns_**

a number that’s the height of the screen on the game device or simulator.

### setBackgroundColor

Set the background color of the screen.

```js
function scene.setBackgroundColor(color: number): void;
```

You can set the background color of the screen at anytime. The background color is always behind other images shown on the screen.

**_Color number_**

There are 16 colors available to choose from. These colors are set in the current color pallete. The pallete contains a collection of colors, each of which have an index number from 0 to 15. The index is also known as the color number. Unless the pallete is changed, the pallete has these colors as the default:

- **0:** transparent
- **1:** white
- **2:** red
- **3:** pink
- **4:** orange
- **5:** yellow
- **6:** teal
- **7:** green
- **8:** blue
- **9:** light blue
- **10:** purple
- **11:** light purple
- **12:** dark purple
- **13:** tan
- **14:** brown
- **15:** black

**_Parameters_**

- **color:** the number for the color to set as the background color of the screen.

### setBackgroundImage

Sets an image as the background for the screen.

```js
function scene.setBackgroundImage(img: Image): void;
```

You can set an image for the background of the screen. The background image is always behind other images shown on the screen.

**_Parameters_**

- **img:** the image to set as the background for the screen.

### backgroundColor

Get the background color of the screen.

```js
function scene.backgroundColor(): number;
```

The background color is the color painted behind other images shown on the screen.

**_Returns_**

a number which is the the color set as the screen background color.

### backgroundImage

Get the background image for the screen.

```js
function scene.backgroundImage(): Image;
```

You can get the current image for the background of the screen. The background image is always behind other images shown on the screen.

**_Returns_**

the image set for the screen background.


## Tiles and Tilemaps

### setTilemap

Set a tilemap as the current tilemap for the game scene.

```js
function tiles.setTilemap(tilemap: tiles.TileMapData): void;
```

A tilemap is a data object that contains the dimensions, layers, and tile list for a tile mapping to set as the scene for a game. A game program can have more than one tilemap defined and the game’s scene or level can change by setting a different tilemap at certain times during a game.

Tilemaps aren’t coded by the user but are created using a tilemap editor. Each tilemap in a game project is a named resource and is contained within the project’s assets collection. In code, a particular tilemap is specified with it’s resource name like this:

```js
tiles.setTilemap(tilemap`level1`)
```

**_Parameters_**

- **tilemap:** the tilemap data containing the tilemap layout and tiles to set for the game scene. The tilemap can be specified by it’s resource name, such as: tilemap`level1`.

**_Examples_**

**1. Maze tilemap**

Create a maze tilemap for a sprite to travel through. Set the tilemap as the scene for the game. Create a sprite and start it travelling in the maze.

```js
tiles.setTilemap(tilemap`level1`)
let mySprite = sprites.create(img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . 5 5 5 5 5 5 . . . . . 
    . . . 5 5 5 5 5 5 5 5 5 5 . . . 
    . . . 5 5 5 5 5 5 5 5 5 5 . . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . 5 5 5 5 5 5 5 5 5 5 5 5 . . 
    . . . 5 5 5 5 5 5 5 5 5 5 . . . 
    . . . 5 5 5 5 5 5 5 5 5 5 . . . 
    . . . . . 5 5 5 5 5 5 . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    `, SpriteKind.Player)
mySprite.setPosition(0, 104)
mySprite.vx = 16
```

### setTileAt

Set a tile at a location in the tilemap.

```js
function tiles.setTileAt(loc: tiles.Location, tile: Image): void;
```

You can set a tile at a specific column and row in the tilemap using a tile location object. Specify a tile to set in the tilemap and a location for it.

**_Parameters_**

- **loc:** a tile location in the tilemap.
- **tile:** the to set in the tilemap.

**_Examples_**

**1. Set tile on empty tilemap.**

Create an empty tilemap and a solid color tile. Set the solid color tile at different places in the tilemap and replace its previous location with a transparent tile.

```js
tiles.setTilemap(tilemap`level1`)
let spot = tiles.getTileLocation(0, 0)
forever(function () {
    tiles.setTileAt(spot, assets.tile`transparency16`)
    spot = tiles.getTileLocation(randint(0, 9), randint(0, 6))
    tiles.setTileAt(spot, assets.tile`myTile`)
    pause(1000)
})
```

### setWallAt

Set a tile as a wall tile in the tilemap.

```js
function tiles.setWallAt(loc: tiles.Location, on: boolean): void;
```

Wall tiles create a barrier for sprites so that they can’t pass through tilemap at the tile location. You can set a tile location in the tilemap as a wall or turn it back to a regular tile.

**_Parameters_**

- **loc:** a tile location in the tilemap.
- **on:** a boolean value to set the tile location be a wall tile if true or a regular tile if false.

**_Examples_**

**1. Toggle wall**

Make a column of tiles from top to bottom of the screen. Set a sprite in motion and set it to bounce on walls. Every 5 seconds, set the tiles in the column to be wall tiles or regular tiles.

```js
let isWall = false
tiles.setTilemap(tilemap`level1`)
let mySprite = sprites.create(img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . 1 1 1 1 1 1 . . . . . 
    . . . 1 1 2 2 2 2 2 2 1 1 . . . 
    . . . 1 2 2 2 2 2 2 2 2 1 . . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . . 1 2 2 2 2 2 2 2 2 1 . . . 
    . . . 1 1 2 2 2 2 2 2 1 1 . . . 
    . . . . . 1 1 1 1 1 1 . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    `, SpriteKind.Player)
mySprite.setBounceOnWall(true)
mySprite.vx = 80
mySprite.vy = 70
game.onUpdateInterval(5000, function () {
    isWall = !(isWall)
    for (let wallTile of tiles.getTilesByType(assets.tile`myTile`)) {
        tiles.setWallAt(wallTile, isWall)
    }
})
```

### getTileLocation

Get the tile location object for a particular column and row position in the tilemap.

```js
function tiles.getTileLocation(col: number, row: number): tiles.Location;
```

A tile location object represents a column and a row position in the tilemap. Location objects are used to get and set tiles or tile information for specific places in the tilemap.

**_Parameters_**

- **col:** a number that is the tilemap column of the tile location.
- **row:** a number that is the tilemap row of the tile location.

**_Returns_**

a tile location object for a given column and row in the tilemap.

### getTilesByType

Return an array of tile locations from the tilemap that contain the specified tile.

```js
function tiles.getTilesByType(tile: Image): tiles.Location[];
```

**_Parameters_**

- **tile:** a tile image to search the tilemap for.

**_Returns_**

an array of tile locations from the tilemap that have the tile specified in tile.

### placeOnTile

Move a sprite’s position to the center of a tile location in the tilemap.

```js
function tiles.placeOnTile(sprite: Sprite, loc: tiles.Location): void;
```

A sprite will locate itself on top of a tile in the tilemap using a tilemap location object. A location object contains a tile row value and a tile column value.

**_Parameters_**

- **sprite:** the sprite to move onto the tile.
- **loc:** a tile location in the tilemap.

### placeOnRandomTile

Move a sprite’s position to the center of a random tile in the scene.

```js
function tiles.placeOnRandomTile(sprite: Sprite, tile: Image): void;
```

You can make a sprite locate itself right on top of a random tile in the tilemap. Choose a tile in the tilemap to indicate which type of tile to have the sprite randomly placed on. If there is only one tile of that type, the sprite will always go to that tile. If multiple tiles of the same type are in the tilemap, then the sprite will randomly locate on any one of them.

**_Parameters_**

- **sprite:** the sprite to move onto the tile.
- **tile:** the tile type to randomly select in the tilemap.

### tileAtLocationEquals

Check if a location in the tilemap contains the tile you’re looking for.

```js
function tiles.tileAtLocationEquals(location: tiles.Location, tile: Image): boolean;
```

**_Parameters_**

- **location:** a location in the tilemap.
- **tile:** an image that matches the tile you’re looking for.

**_Returns_**

a boolean value that is true if the tile at location matches the tile you want to check for. If not, false is returned.

### isHittingTile

Check if a sprite is currently hitting a wall.

```js
(method) Sprite.isHittingTile(direction: CollisionDirection): boolean;
```

A wall is either a tile set as a wall or the edge of the scene if a tilemap is set.

**_Parameters_**

- **direction:** the direction the sprite moves toward when detecting a wall collision: left, right, top, or bottom.

**_Returns_**

a boolean value that is true if the sprite is hitting a wall on the chosen side or false if not.

### tileKindAt

Check if the tile next to a sprite is the one you’re looking for.

```js
(method) Sprite.tileKindAt(direction: TileDirection, tile: Image): boolean;
```

A tile next to a sprite can be matched to a tile you want to check for. You choose the direction to check for to see if the tile is next to the sprite. If the tile matches the one you are looking for in that direction, it will detect it when the sprite is within one-half of it’s dimension for that direction.

**_Parameters_**

- **direction:** the direction from the sprite for the tile check: left, right, top, or bottom.
- **tile:** an image that matches the tile you’re looking for.

**_Returns_**

a boolean value that is true if the tile near the sprite at the direction matches the tile you want to check for. If not, false is returned.


## Tiles and Tilemaps Events

### onOverlapTile

Run code when a sprite overlaps a tile.

```js
function scene.onOverlapTile(kind: number, tile: Image, handler: (sprite: Sprite, location: tiles.Location) => void): void;
```

You can detect when a moving sprite overlaps a tile in the tilemap. If your sprite moves across a tile, you can have some code that runs when that happens. You pick the sprite kind to check for.

When an overlap is detected by the sprite of the kind you asked for, it is given to you in the sprite parameter of handler along with overlapped tile’s location.

A sprite hitting a wall is dectected when the outside edges of its image makes starts to overlap the tile.

**_Parameters_**

- **kind:** the type of sprite to check for a overlap.
- **handler:** the code to run when the sprite overlaps a tile. The handler has these parameters passed to it:
  - **sprite:** the sprite that overlapped the tile.
  - **location:** the location of the tile that the sprite overlapped in the tilemap.

**_Examples_**

**1. Detect overlap**

Create a tilemap with a section of tiles in the center of the scene. Set a sprite in motion and cause it to bounce on walls. When the sprite overlaps the tiles, make it trace a trail across the tiles.

```js
scene.onOverlapTile(SpriteKind.Player, assets.tile`myTile`, function (sprite, location) {
    sprite.startEffect(effects.trail, 100)
})
tiles.setTilemap(tilemap`level1`)
let mySprite = sprites.create(img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . 1 1 1 1 1 1 . . . . . 
    . . . 1 1 2 2 2 2 2 2 1 1 . . . 
    . . . 1 2 2 2 2 2 2 2 2 1 . . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . . 1 2 2 2 2 2 2 2 2 1 . . . 
    . . . 1 1 2 2 2 2 2 2 1 1 . . . 
    . . . . . 1 1 1 1 1 1 . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    `, SpriteKind.Player)
mySprite.setBounceOnWall(true)
mySprite.vx = 80
mySprite.vy = 70
```

### onHitWall

Run code when a sprite contacts a wall tile.

```js
function scene.onHitWall(kind: number, handler: (sprite: Sprite, location: tiles.Location) => void): void;
```

You can detect when a moving sprite contacts a wall tile in the tilemap. If your sprite touches a tile that is set as a wall, you can have some code that runs when that happens. You pick the sprite kind to check for.

When a wall hit is detected by the sprite of the kind you asked for, it is given to you in the sprite parameter of handler along with contacted tile’s location.

A sprite hitting a wall is dectected when the outside edges of its image makes contact with the tile. If a sprite has it’s ghost flag set, any contact with the wall tile isn’t noticed.

**_Parameters_**

- **kind:** the type of sprite to check for a wall hit.
- **handler:** the code to run when the sprite makes contact. The handler has these parameters passed to it:
  - **sprite:** the sprite that hit the wall tile.
  - **location:** the location of the wall the sprite contacted in the tilemap.

**_Examples_**

**1. Create effect on hit wall**

Create a tilemap with wall tiles surrounding the sides of the scene. Set a sprite in motion and cause it to bounce on wall tiles. When the sprite contacts the wall tiles, make a short fire effect on the sprite.

```js
scene.onHitWall(SpriteKind.Player, function (sprite, location) {
    sprite.startEffect(effects.fire, 200)
})
tiles.setTilemap(tilemap`level1`)
let mySprite = sprites.create(img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . 1 1 1 1 1 1 . . . . . 
    . . . 1 1 2 2 2 2 2 2 1 1 . . . 
    . . . 1 2 2 2 2 2 2 2 2 1 . . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . 1 2 2 2 2 2 2 2 2 2 2 1 . . 
    . . . 1 2 2 2 2 2 2 2 2 1 . . . 
    . . . 1 1 2 2 2 2 2 2 1 1 . . . 
    . . . . . 1 1 1 1 1 1 . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    `, SpriteKind.Player)
mySprite.setBounceOnWall(true)
mySprite.vx = 80
mySprite.vy = 70
```


## Screen Effects

### startScreenEffect

Start a displaying a built-in effect on the screen.

```js
(method) effects.ScreenEffect.startScreenEffect(duration: number, particlesPerSecond: number): void;
```

There are several built-in effects you can show on the screen. This will start the effect you choose. In the Blocks editor, you can select an effect from a list in the block.

**_Examples_**

**1. Makes blizzard effect for 5 seconds**

Start the blizzard effect on the screen. Wait 5 seconds and then stop the effect.

```js
effects.blizzard.startScreenEffect()
pause(5000)
effects.blizzard.endScreenEffect()
```

### endScreenEffect

Stop a built-in effect from displaying on the screen.

```js
(method) effects.ScreenEffect.startScreenEffect(duration: number, particlesPerSecond: number): void;
```

There are several built-in effects you can show on the screen. This will stop the effect you choose if it’s currently displaying. In the Blocks editor, you can select an effect from a list in the block.

**_Examples_**

**1. Creates two effects, but stop only one of them**

Start the blizzard and confetti effects on the screen. Wait 5 seconds and then stop only the confetti effect.

```js
effects.blizzard.startScreenEffect()
effects.confetti.startScreenEffect()
pause(5000)
effects.confetti.endScreenEffect()
```


## Camera View

### cameraFollowSprite

Make the game scene adjust to follow the motion of a sprite.

```js
function scene.cameraFollowSprite(sprite: Sprite): void;
```

Your game scene might not be the same size as your screen. If the scene is bigger that the screen size, your sprites might travel past the edge of the screen. The sprite is still in the scene but it moves out of view. In order to keep your sprite in view on the screen, you can set the scene camera to follow the sprite as it moves across the scene.

While a sprite is located on a part of the scene that currently is in the screen view, it will locate to a new position on the screen when moved. When the camera is following the sprite and it moves to a position outside the view of the screen, the sprite will center in the screen and scene moves instead. This creates the perspective of a camera view in motion across the scene.

**_Parameters_**

- **sprite:** a sprite that the scene camera will follow.

**_Examples_**

**1. Follow sprite on scene bigger than the screen.**

Make a tile map that creates a scene bigger than the screen. Move a sprite from one side of the screen to the other. Have the scene camera follow the sprite as it moves toward the edge of the scene.

```js
let sceneMap: Image = null
let baseTile: Image = null
let ball: Sprite = null
let sceneX = 0
let sceneY = 0
sceneX = scene.screenWidth() * 3 / 2
sceneY = scene.screenHeight()
sceneX = sceneX - sceneX % 16
sceneY = sceneY - sceneY % 16
baseTile = image.create(16, 16)
baseTile.fill(4)
scene.setTile(4, baseTile)
sceneMap = image.create(sceneX / 16, sceneY / 16)
sceneMap.fill(4)
sceneMap.drawLine(0, 0, 0, sceneY / 16, 9)
sceneMap.drawLine(sceneX / 16, 0, sceneX /16, sceneY / 16, 9)
sceneMap.drawLine(sceneX / 32, 0, sceneX / 32, sceneY / 16, 7)

scene.setTileMap(sceneMap)
ball = sprites.create(img`
. . . . f f f f f f f f . . . . 
. . f e e e e e e e e e e f . . 
. f e e e e e e e e e e e e f . 
. f e e e e e e e e e e e e f . 
f e e e e e e e e e e e e e e f 
f e e e e e e e e e e e e e e f
f e e e e e e e e e e e e e e f 
f e e e e e e e e e e e e e e f 
f e e e e e e e e e e e e e e f 
f e e e e e e e e e e e e e e f 
. f e e e e e e e e e e e e f .
. f e e e e e e e e e e e e f .
. . f e e e e e e e e e e f . .
. . . . f f f f f f f f . . . . 
`)
ball.vx = 70
scene.cameraFollowSprite(ball)
game.onUpdate(function () {
    if (ball.x < 16 || ball.x > sceneX - 16) {
        ball.vx = ball.vx * -1
    }
})
```

### centerCameraAt

Center the camera view at a location in the scene.

```js
function scene.centerCameraAt(x: number, y: number): void;
```

The size of your scene might be larger that the screen size. To move a part of the scene into the view of the screen, you can set the camera view to center at a location in the scene.

**_Parameters_**

- **x:** a number that’s the horizontal location of the scene to center the camera view at.
- **y:** a number that’s the vertical location of the scene to centere the camera view at.

**_Examples_**

**1. Draw 'X' and center camera at the corners**

Make a tile map that’s 1.5 times the size of the screen. Draw a “X” pattern in the scene with tiles. Randomly center the camera view to each corner of the scene.

```js
let sceneMap: Image = null
let baseTile: Image = null
let sceneY = 0
let corner = 0
let sceneX = 0
sceneX = scene.screenWidth() * 3 / 2
sceneY = scene.screenHeight() * 3 / 2
sceneX = sceneX - sceneX % 8
sceneY = sceneY - sceneY % 8
baseTile = image.create(8, 8)
baseTile.fill(8)
sceneMap = image.create(sceneX / 8, sceneY / 8)
scene.setTile(8, baseTile)
sceneMap.fill(8)
sceneMap.drawRect(0, 0, sceneX / 8, sceneY / 8, 3)
sceneMap.drawLine(0, 0, sceneX / 8, sceneY / 8, 3)
sceneMap.drawLine(0, sceneY / 8, sceneX / 8, 0, 3)
scene.setTileMap(sceneMap)
game.onUpdateInterval(500, function () {
    corner = Math.randomRange(0, 3)
    if (corner == 0) {
        scene.centerCameraAt(0, 0)
    } else if (corner == 1) {
        scene.centerCameraAt(sceneX - 1, 0)
    } else if (corner == 2) {
        scene.centerCameraAt(0, sceneY - 1)
    } else {
        scene.centerCameraAt(sceneX - 1, sceneY - 1)
    }
})
```

### cameraShake

Shake the camera view to create a shock effect.

```js
function scene.cameraShake(amplitude: number, duration: number): void;
```

**_Parameters_**

- **amplitude:** a number maximum offset the camera will move.
- **duration:** a number duration in milli-seconds
