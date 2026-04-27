# MakeCode Arcade / API Reference Guide / Arcade Mini Menu Extension

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **arcade-mini-menu** extension.

This extension allows you to create menus for your arcade game!

Some of its features include:

1. Menus that can be used as sprites (with all the blocks in the Sprites category)
2. Support for both text and icons
3. Automatic scrolling when space is limited
4. Flexible menu layouts including single-column, single-row, and an x-y grid
5. A wide range of menu style options to match the look and feel of your game

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Create

### createMenuFromArray

Creates a MenuSprite from an array of MenuItems

```js
miniMenu.createMenuFromArray(items: MenuItem[]): Sprite;
```

After you create the MenuSprite, any changes to the array passed into this function will affect the MenuSprite.
That makes this function great to use for something like a player inventory which may gain or lose items over time.

**_Parameters_**

- **items** - an array of MenuItems to populate the menu with

**_Returns_**

A new menu (which is a Sprite).

**_Examples_**

**1. This example creates a menu from an array of the days of the week**

```js
let myMenu = miniMenu.createMenuFromArray([
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
])
```

### createMenu

Creates a MenuSprite with up to 12 MenuItems. If you need more than 12 items, use `create menu from array` instead.

```js
miniMenu.createMenu(item1: MenuItem, item2?: MenuItem, item3?: MenuItem, item4?: MenuItem, item5?: MenuItem, item6?: MenuItem, item7?: MenuItem, item8?: MenuItem, item9?: MenuItem, item10?: MenuItem, item11?: MenuItem, item12?: MenuItem,): Sprite;
```

### createMenuItem

Creates a MenuItem with some text and an optional icon

```js
miniMenu.createMenuItem(text: string, image?: Image, disabled?: boolean): MenuItem;
```

**_Parameters_**

- **text** - The text for the MenuItem
- **icon** - An optional icon for the MenuItem

**_Returns_**

A new MenuItem.

**_Examples_**

**1. This example creates a menu where some of the items have icons and some do not.**

```js
let myMenu = miniMenu.createMenu(
miniMenu.createMenuItem("Apple", img`
    . . . . . . . e c 7 . . . . . .
    . . . . e e e c 7 7 e e . . . .
    . . c e e e e c 7 e 2 2 e e . .
    . c e e e e e c 6 e e 2 2 2 e .
    . c e e e 2 e c c 2 4 5 4 2 e .
    c e e e 2 2 2 2 2 2 4 5 5 2 2 e
    c e e 2 2 2 2 2 2 2 2 4 4 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 4 2 e
    . e e e 2 2 2 2 2 2 2 2 2 4 e .
    . 2 e e 2 2 2 2 2 2 2 2 4 2 e .
    . . 2 e e 2 2 2 2 2 4 4 2 e . .
    . . . 2 2 e e 4 4 4 2 e e . . .
    . . . . . 2 2 e e e e . . . . .
    `),
miniMenu.createMenuItem("Lemon", img`
    4 4 4 . . 4 4 4 4 4 . . . . . .
    4 5 5 4 4 5 5 5 5 5 4 4 . . . .
    b 4 5 5 1 5 1 1 1 5 5 5 4 . . .
    . b 5 5 5 5 1 1 5 5 1 1 5 4 . .
    . b d 5 5 5 5 5 5 5 5 1 1 5 4 .
    b 4 5 5 5 5 5 5 5 5 5 5 1 5 4 .
    c d 5 5 5 5 5 5 5 5 5 5 5 5 5 4
    c d 4 5 5 5 5 5 5 5 5 5 5 1 5 4
    c 4 5 5 5 d 5 5 5 5 5 5 5 5 5 4
    c 4 d 5 4 5 d 5 5 5 5 5 5 5 5 4
    . c 4 5 5 5 5 d d d 5 5 5 5 5 b
    . c 4 d 5 4 5 d 4 4 d 5 5 5 4 c
    . . c 4 4 d 4 4 4 4 4 d d 5 d c
    . . . c 4 4 4 4 4 4 4 4 5 5 5 4
    . . . . c c b 4 4 4 b b 4 5 4 4
    . . . . . . c c c c c c b b 4 .
    `),
miniMenu.createMenuItem("Strawberry", img`
    . . . . . . . 6 . . . . . . . .
    . . . . . . 8 6 6 . . . 6 8 . .
    . . . e e e 8 8 6 6 . 6 7 8 . .
    . . e 2 2 2 2 e 8 6 6 7 6 . . .
    . e 2 2 4 4 2 7 7 7 7 7 8 6 . .
    . e 2 4 4 2 6 7 7 7 6 7 6 8 8 .
    e 2 4 5 2 2 6 7 7 6 2 7 7 6 . .
    e 2 4 4 2 2 6 7 6 2 2 6 7 7 6 .
    e 2 4 2 2 2 6 6 2 2 2 e 7 7 6 .
    e 2 4 2 2 4 2 2 2 4 2 2 e 7 6 .
    e 2 4 2 2 2 2 2 2 2 2 2 e c 6 .
    e 2 2 2 2 2 2 2 4 e 2 e e c . .
    e e 2 e 2 2 4 2 2 e e e c . . .
    e e e e 2 e 2 2 e e e c . . . .
    e e e 2 e e c e c c c . . . . .
    . c c c c c c c . . . . . . . .
    `),
miniMenu.createMenuItem("No Icon")
)
```

### close

Destroys a menu sprite. This is exactly the same as using the destroy block in the sprites category

```js
miniMenu.close(menu: Sprite);
```

**_Parameters_**

- **menu** - The menu to be closed.

**_Examples_**

**1. This example demonstrates how to close a menu when the B button is pressed.**

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.y = 60
myMenu.x = 80
myMenu.onButtonPressed(controller.B, function (selection, selectedIndex) {
    myMenu.close()
})
```

### setTitle

Sets the title for the MenuSprite.

```js
miniMenu.setTitle(menu: Sprite, title: string);
```

The title styling can be customized using the `set style property` function.

**_Parameters_**

- **menu** - the menu to be customized
- **title** - a title to show at the top of the menu

### setDimensions

Sets the width and height of the menu sprite. This is the same as setting the width and height using the `set menu style property` function.

Setting the width or height to 0 will result in the menu growing to fit its content.

```js
miniMenu.setDimensions(menu: Sprite, width: number, height: number);
```

**_Parameters_**

- **menu** - the menu to be customized
- **width** - the desired width of the menu or 0 for content width
- **height** - the desired height of the menu or 0 for content height


## Controls

### onButtonPressed

Runs some code when a button is pressed while this MenuSprite is not destroyed or closed.
Each MenuSprite can only have one event handler for each button.
Using this function with one of the direction buttons will override the default behavior for that direction.
The `set button events enabled` block can also be used to control if these events run or not.

```js
miniMenu.onButtonPressed(menu: Sprite, button: Button, handler: (selection: string, selectedIndex: number) => void);
```

**_Parameters_**

- **menu** - the menu to be controlled
- **button** - the button that, when pressed, will cause the code to run
- **handler** - handler function with following parameters:
  - **selection** the text of the currently selected menu item. This can be dragged out of the block and used within the event handler
  - **selectionIndex** the index of the currently selected menu item. This can be dragged out of the block and used within the event handler

**_Examples_**

**1. Selecting an item**

This example creates a menu and uses a button event to close it and splash the selected value to the screen.

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.onButtonPressed(controller.A, function (selection, selectedIndex) {
    myMenu.close()
    game.splash(selection)
})
```

**2. Overriding default behavior**

This example overrides the default behavior for the up button. Notice how pressing up no longer moves the selection for the menu, but pressing down still works.

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.onButtonPressed(controller.up, function (selection, selectedIndex) {
    // implement new behavior here
})
```

### onSelectionChanged

Runs some code when the selected item of a MenuSprite changes. The code will also run immediately when this function is called.

```js
miniMenu.onSelectionChanged(menu: Sprite, handler: (selection: string, selectedIndex: number) => void);
```

**_Parameters_**

- **menu** - the menu to be controlled
- **handler** - handler function with following parameters:
  - **selection** the text of the currently selected menu item. This can be dragged out of the block and used within the event handler
  - **selectionIndex** the index of the currently selected menu item. This can be dragged out of the block and used within the event handler

**_Examples_**

**1. Show information when selection changes**

This example creates a menu and has a sprite say information about the selected item each time the selection changes.

```js
scene.setBackgroundColor(9)
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
mySprite.bottom = 118
let myMenu = miniMenu.createMenu(
miniMenu.createMenuItem("Apple", img`
    . . . . . . . e c 7 . . . . . .
    . . . . e e e c 7 7 e e . . . .
    . . c e e e e c 7 e 2 2 e e . .
    . c e e e e e c 6 e e 2 2 2 e .
    . c e e e 2 e c c 2 4 5 4 2 e .
    c e e e 2 2 2 2 2 2 4 5 5 2 2 e
    c e e 2 2 2 2 2 2 2 2 4 4 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
    c e e 2 2 2 2 2 2 2 2 2 2 4 2 e
    . e e e 2 2 2 2 2 2 2 2 2 4 e .
    . 2 e e 2 2 2 2 2 2 2 2 4 2 e .
    . . 2 e e 2 2 2 2 2 4 4 2 e . .
    . . . 2 2 e e 4 4 4 2 e e . . .
    . . . . . 2 2 e e e e . . . . .
    `),
miniMenu.createMenuItem("Lemon", img`
    4 4 4 . . 4 4 4 4 4 . . . . . .
    4 5 5 4 4 5 5 5 5 5 4 4 . . . .
    b 4 5 5 1 5 1 1 1 5 5 5 4 . . .
    . b 5 5 5 5 1 1 5 5 1 1 5 4 . .
    . b d 5 5 5 5 5 5 5 5 1 1 5 4 .
    b 4 5 5 5 5 5 5 5 5 5 5 1 5 4 .
    c d 5 5 5 5 5 5 5 5 5 5 5 5 5 4
    c d 4 5 5 5 5 5 5 5 5 5 5 1 5 4
    c 4 5 5 5 d 5 5 5 5 5 5 5 5 5 4
    c 4 d 5 4 5 d 5 5 5 5 5 5 5 5 4
    . c 4 5 5 5 5 d d d 5 5 5 5 5 b
    . c 4 d 5 4 5 d 4 4 d 5 5 5 4 c
    . . c 4 4 d 4 4 4 4 4 d d 5 d c
    . . . c 4 4 4 4 4 4 4 4 5 5 5 4
    . . . . c c b 4 4 4 b b 4 5 4 4
    . . . . . . c c c c c c b b 4 .
    `),
miniMenu.createMenuItem("Strawberry", img`
    . . . . . . . 6 . . . . . . . .
    . . . . . . 8 6 6 . . . 6 8 . .
    . . . e e e 8 8 6 6 . 6 7 8 . .
    . . e 2 2 2 2 e 8 6 6 7 6 . . .
    . e 2 2 4 4 2 7 7 7 7 7 8 6 . .
    . e 2 4 4 2 6 7 7 7 6 7 6 8 8 .
    e 2 4 5 2 2 6 7 7 6 2 7 7 6 . .
    e 2 4 4 2 2 6 7 6 2 2 6 7 7 6 .
    e 2 4 2 2 2 6 6 2 2 2 e 7 7 6 .
    e 2 4 2 2 4 2 2 2 4 2 2 e 7 6 .
    e 2 4 2 2 2 2 2 2 2 2 2 e c 6 .
    e 2 2 2 2 2 2 2 4 e 2 e e c . .
    e e 2 e 2 2 4 2 2 e e e c . . .
    e e e e 2 e 2 2 e e e c . . . .
    e e e 2 e e c e c c c . . . . .
    . c c c c c c c . . . . . . . .
    `)
)
myMenu.top = 2
myMenu.x = 80
myMenu.onSelectionChanged(function (selection, selectedIndex) {
    if (selectedIndex == 0) {
        mySprite.sayText("Meh, apples are okay")
    } else if (selectedIndex == 1) {
        mySprite.sayText("Too sour for me")
    } else {
        mySprite.sayText("I love strawberries!")
    }
})
```

### setButtonEventsEnabled

Controls whether or not button events are enabled on this menu sprite. Setting this to false will also disable the default button behavior of changing the menu selection when a button is pressed.

```js
miniMenu.setButtonEventsEnabled(menu: Sprite, enabled: boolean);
```

**_Parameters_**

- **menu** - the menu to be controlled
- **enabled** - if true, button events will work as normal. If false, they will be disabled

### setController

Sets the controller that will send button events to the specified MenuSprite. This is useful for multiplayer games.

```js
miniMenu.setController(menu: Sprite, ctrl: controller.Controller);
```

**_Parameters_**

- **menu**: The MenuSprite to set the controller for
- **ctrl**: The controller that will control this MenuSprite

### moveSelection

Moves the selection of the menu in the given direction. This is useful for implementing your own custom menu controls.

```js
miniMenu.moveSelection(menu: Sprite, direction: number);
```

For some menu layouts, some movement directions will have no effect:

- **Single column menu** - can only move up and down
- **Single row menu** - can only move left and right
* **Grid menu** - can move in all directions

For more on menu layouts, see the `set menu style property` function.

**_MoveDirection Enum_**

The `direction` parameter comes from the `MoveDirection` enum, whose values are:

- **Up** = 0
- **Down** = 1
- **Left** = 2
- **Right** = 3

**_Parameters_**

- **menu** - the menu to be controlled
- **direction** - the direction to move the selection in

**_Examples_**

**1. Move menu item up and down**

This example uses the move selection block with the controller repeat events so that holding a button causes the selection to move quickly.

```js
controller.up.onEvent(ControllerButtonEvent.Repeated, function () {
    myMenu.moveSelection(miniMenu.MoveDirection.Up)
})
controller.down.onEvent(ControllerButtonEvent.Repeated, function () {
    myMenu.moveSelection(miniMenu.MoveDirection.Down)
})
```


## Styling

### setFrame

Sets the frame for the MenuSprite.

A frame is a special image that is used to create borders that surround the MenuSprite. It must be a square image where the width and height are both divisible by 3.

```js
miniMenu.setFrame(menu: Sprite, frame: Image);
```

This function is very similar to the `set dialog frame` block in the game category except that it ignores the center of the image.

**_Parameters_**

- **menu** - the menu to be styled
- **frame** - a square image used for drawing the frame of the menu

**_Examples_**

**1. Show frame around menu item**

This example creates a menu and gives it a frame. Take a look at the frame image and see how the colors map to the MenuSprite: each color only shows up on the side it corresponds to and the pink in the center of the sprite is ignored.

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.setDimensions(100, 40)
myMenu.setFrame(img`
    2 2 2 2 2 5 5 5 5 5 8 8 8 8 8
    2 2 2 2 2 5 5 5 5 5 8 8 8 8 8
    2 2 2 2 2 5 5 5 5 5 8 8 8 8 8
    2 2 2 2 2 c b c b c 8 8 8 8 8
    2 2 2 2 2 c b c b c 8 8 8 8 8
    9 9 9 c c 3 3 3 3 3 c c 7 7 7
    9 9 9 b b 3 3 3 3 3 b b 7 7 7
    9 9 9 c c 3 3 3 3 3 c c 7 7 7
    9 9 9 b b 3 3 3 3 3 b b 7 7 7
    9 9 9 c c 3 3 3 3 3 c c 7 7 7
    a a a a a c b c b c 6 6 6 6 6
    a a a a a c b c b c 6 6 6 6 6
    a a a a a 4 4 4 4 4 6 6 6 6 6
    a a a a a 4 4 4 4 4 6 6 6 6 6
    a a a a a 4 4 4 4 4 6 6 6 6 6
    `)
```

### setMenuStyleProperty

Sets one of the available style options on a MenuSprite.

```js
miniMenu.setMenuStyleProperty(menu: Sprite, property: MenuStyleProperty, value: number);
```

The available styles include:

- **width** - the width of the menu in pixels. This will cause the menu to scroll if it is smaller than the default width. Defaults to 0
- **height** - the height of the menu in pixels. This will cause the menu to scroll if it is smaller than the default height. Defaults to 0
- **border** - the width of the border of the menu. This can either be a single number (in which case it will apply to all sides) or the output of the `createBorderBox` function. Defaults to 0
- **border color** - sets the color of the menu border. This will not be visible unless the border property is changed. Defaults to 0
- **padding** - the width of the padding around the content of the menu. This can either be a single number (in which case it will apply to all sides) or the output of the `createBorderBox` function. Defaults to 0
- **background color** - sets the color of the menu background. This will not be visible unless the padding property is changed or the menu item style for this menu allow the background to show through (e.g. with margins or a transparent background). Defaults to 0
- **scroll speed** - sets the speed at which text scrolls for the selected menu item if the selected menu item overflows the menu width. Defaults to 150
- **rows** - if set, limits the number of rows that are displayed in the menu before scrolling. Defaults to 0
- **columns** - if set, limits the number of columns that are displayed in the menu before scrolling. Defaults to 0
- **scroll indicator color** - if set, causes a scroll indicator in the specified color to appear next to the menu when the menu can be scrolled in a given direction. Defaults to 0
- **use as template** - if set to any number other than 0, all other MenuSprites will use this MenuSprite's styles as a template. In other words, all menu style properties and menu item properties will be inherited from this MenuSprite unless explicitly set. Defaults to 0

**_Changing the menu layout_**

The **rows** and **columns** styles can be used to control the layout and navigation of the menu. The following configurations are supported:

- Single column menu, scrolling vertically - `rows = 0` and `columns = 0` or `rows = 0` and `columns = 1`
- Single row menu, scrolling horizontally - `rows = 1` and `columns = 0`
- Grid menu, scrolling vertically - `rows >= 1` and `columns >= 1`

Changing the layout will also change how the default control scheme works for the menu.
Single column menus use the up and down buttons to move, single row menus use the left and right buttons to move, and grid menus use all four directions.

To see these layouts in actions, take a look at the examples below!

**_Parameters_**

- **menu** - the menu to be styled
- **property**: the style option to change
- **value**: a number to set the style option to

**_Examples_**

**1. Single column menu with styling**

This example creates a single column menu and applies some style options to it.

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.y = 60
myMenu.x = 80
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Height, 30)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.BorderColor, 2)
myMenu.setMenuStyleProperty(
    miniMenu.MenuStyleProperty.Border, 
    miniMenu.createBorderBox(0, 4, 0, 4)
)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.ScrollIndicatorColor, 2)
myMenu.setMenuStyleProperty(
    miniMenu.MenuStyleProperty.Border, 
    miniMenu.createBorderBox(0, 4, 0, 4)
)
```

**2. Single row menu**

This example creates a single row menu that can be scrolled using the left and right buttons

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Rows, 1)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Width, 60)
myMenu.y = 60
myMenu.x = 80
```

### setStyleProperty

Sets one of the available style options for the meneu items of a MenuSprite.

```js
miniMenu.setStyleProperty(menu: Sprite, kind: StyleKind, property: StyleProperty, value: number);
```

**_Style targets_**

There are three types of styles that can be changed with this block:

- **default** - changes the style of the menu items that are not selected in the menu
- **selected** - changes the style of the selected menu item
- **title** - changes the style of the title that appears above the menu. This is only visible if the title has been set

**_Style properties_**

The available styles include:

- **padding** - the width of the padding around the content of the menu item. This can either be a single number (in which case it will apply to all sides) or the output of the `createBorderBox` function. Defaults to 2
- **foreground** - sets the color of the menu item text. Defaults to 15 for title/default menu items, and 1 for selected nenu items
- **background** - sets the color of the menu item background. Defaults to 1 for title/default menu items, and 3 for selected menu items
- **border** - the width of the border of the menu item. This can either be a single number (in which case it will apply to all sides) or the output of the `createBorderBox` function. Defaults to 0
- **border color** - sets the color of the menu item border. This will not be visible unless the border property is changed. Defaults to 0
- **margin** - sets the spacing between a menu item and the adjacent menu items. This can either be a single number (in which case it will apply to all sides) or the output of the `createBorderBox` function. Defaults to 0
- **icon-text spacing** - sets the number of pixels between a menu item's icon and its text. If the menu item has no icon, this does nothing. Defaults to 0
- **alignment** - sets the alignment of the text/icon in the menu item. Use 0 for left align, 1 for center align, and 2 for right align. Defaults to 0
- **icon only** - if set to any number other than 0, causes the menu item to be drawn with just the icon and no text. Defaults to 0

**_Parameters_**

- **menu** - the menu to be styled
- **kind**: the part of the menu to set the style on
- **property**: the style option to change
- **value**: a [number](/types/number) to set the style option to

**_Examples_**

**1. Styled menu**

This example adds margin between the menu items, customizes the menu's colors, and gives each menu item a border on the left side.

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.y = 60
myMenu.x = 80
myMenu.setStyleProperty(
    miniMenu.StyleKind.DefaultAndSelected, 
    miniMenu.StyleProperty.Margin, 
    miniMenu.createBorderBox(0, 0, 0, 2)
)
myMenu.setStyleProperty(
    miniMenu.StyleKind.DefaultAndSelected, 
    miniMenu.StyleProperty.Border, 
    miniMenu.createBorderBox(4, 0, 0, 0)
)
myMenu.setStyleProperty(miniMenu.StyleKind.Default, miniMenu.StyleProperty.BorderColor, 6)
myMenu.setStyleProperty(miniMenu.StyleKind.Selected, miniMenu.StyleProperty.BorderColor, 7)
myMenu.setStyleProperty(miniMenu.StyleKind.Default, miniMenu.StyleProperty.Background, 3)
myMenu.setStyleProperty(miniMenu.StyleKind.Default, miniMenu.StyleProperty.Foreground, 1)
myMenu.setStyleProperty(miniMenu.StyleKind.Selected, miniMenu.StyleProperty.Background, 1)
myMenu.setStyleProperty(miniMenu.StyleKind.Selected, miniMenu.StyleProperty.Foreground, 15)
```

**2. Icon only menu**

This example creates an inventory-like menu by using the icon only style

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Apple", img`
        . . . . . . . e c 7 . . . . . .
        . . . . e e e c 7 7 e e . . . .
        . . c e e e e c 7 e 2 2 e e . .
        . c e e e e e c 6 e e 2 2 2 e .
        . c e e e 2 e c c 2 4 5 4 2 e .
        c e e e 2 2 2 2 2 2 4 5 5 2 2 e
        c e e 2 2 2 2 2 2 2 2 4 4 2 2 e
        c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
        c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
        c e e 2 2 2 2 2 2 2 2 2 2 2 2 e
        c e e 2 2 2 2 2 2 2 2 2 2 4 2 e
        . e e e 2 2 2 2 2 2 2 2 2 4 e .
        . 2 e e 2 2 2 2 2 2 2 2 4 2 e .
        . . 2 e e 2 2 2 2 2 4 4 2 e . .
        . . . 2 2 e e 4 4 4 2 e e . . .
        . . . . . 2 2 e e e e . . . . .
        `),
    miniMenu.createMenuItem("Burger", img`
        . . . . c c c b b b b b . . . .
        . . c c b 4 4 4 4 4 4 b b b . .
        . c c 4 4 4 4 4 5 4 4 4 4 b c .
        . e 4 4 4 4 4 4 4 4 4 5 4 4 e .
        e b 4 5 4 4 5 4 4 4 4 4 4 4 b c
        e b 4 4 4 4 4 4 4 4 4 4 5 4 4 e
        e b b 4 4 4 4 4 4 4 4 4 4 4 b e
        . e b 4 4 4 4 4 5 4 4 4 4 b e .
        8 7 e e b 4 4 4 4 4 4 b e e 6 8
        8 7 2 e e e e e e e e e e 2 7 8
        e 6 6 2 2 2 2 2 2 2 2 2 2 6 c e
        e c 6 7 6 6 7 7 7 6 6 7 6 c c e
        e b e 8 8 c c 8 8 c c c 8 e b e
        e e b e c c e e e e e c e b e e
        . e e b b 4 4 4 4 4 4 4 4 e e .
        . . . c c c c c e e e e e . . .
        `),
    miniMenu.createMenuItem("Lemon", img`
        4 4 4 . . 4 4 4 4 4 . . . . . .
        4 5 5 4 4 5 5 5 5 5 4 4 . . . .
        b 4 5 5 1 5 1 1 1 5 5 5 4 . . .
        . b 5 5 5 5 1 1 5 5 1 1 5 4 . .
        . b d 5 5 5 5 5 5 5 5 1 1 5 4 .
        b 4 5 5 5 5 5 5 5 5 5 5 1 5 4 .
        c d 5 5 5 5 5 5 5 5 5 5 5 5 5 4
        c d 4 5 5 5 5 5 5 5 5 5 5 1 5 4
        c 4 5 5 5 d 5 5 5 5 5 5 5 5 5 4
        c 4 d 5 4 5 d 5 5 5 5 5 5 5 5 4
        . c 4 5 5 5 5 d d d 5 5 5 5 5 b
        . c 4 d 5 4 5 d 4 4 d 5 5 5 4 c
        . . c 4 4 d 4 4 4 4 4 d d 5 d c
        . . . c 4 4 4 4 4 4 4 4 5 5 5 4
        . . . . c c b 4 4 4 b b 4 5 4 4
        . . . . . . c c c c c c b b 4 .
        `),
    miniMenu.createMenuItem("Chicken", img`
        . . 2 2 b b b b b . . . . . . .
        . 2 b 4 4 4 4 4 4 b . . . . . .
        2 2 4 4 4 4 d d 4 4 b . . . . .
        2 b 4 4 4 4 4 4 d 4 b . . . . .
        2 b 4 4 4 4 4 4 4 d 4 b . . . .
        2 b 4 4 4 4 4 4 4 4 4 b . . . .
        2 b 4 4 4 4 4 4 4 4 4 e . . . .
        2 2 b 4 4 4 4 4 4 4 b e . . . .
        . 2 b b b 4 4 4 b b b e . . . .
        . . e b b b b b b b e e . . . .
        . . . e e b 4 4 b e e e b . . .
        . . . . . e e e e e e b d b b .
        . . . . . . . . . . . b 1 1 1 b
        . . . . . . . . . . . c 1 d d b
        . . . . . . . . . . . c 1 b c .
        . . . . . . . . . . . . c c . .
        `),
    miniMenu.createMenuItem("Ham", img`
        . . . . . . 2 2 2 2 . . . . . .
        . . . . 2 2 3 3 3 3 2 e . . . .
        . . . 2 3 d 1 1 d d 3 2 e . . .
        . . 2 3 1 d 3 3 3 d d 3 e . . .
        . 2 3 1 3 3 3 3 3 d 1 3 b e . .
        . 2 1 d 3 3 3 3 d 3 3 1 3 b b .
        2 3 1 d 3 3 1 1 3 3 3 1 3 4 b b
        2 d 3 3 d 1 3 1 3 3 3 1 3 4 4 b
        2 d 3 3 3 1 3 1 3 3 3 1 b 4 4 e
        2 d 3 3 3 1 1 3 3 3 3 1 b 4 4 e
        e d 3 3 3 3 d 3 3 3 d d b 4 4 e
        e d d 3 3 3 d 3 3 3 1 3 b 4 b e
        e 3 d 3 3 1 d d 3 d 1 b b e e .
        . e 3 1 1 d d 1 1 1 b b e e e .
        . . e 3 3 3 3 3 3 b e e e e . .
        . . . e e e e e e e e e e . . .
        `),
    miniMenu.createMenuItem("Pizza", img`
        . . . . . . b b b b . . . . . .
        . . . . . . b 4 4 4 b . . . . .
        . . . . . . b b 4 4 4 b . . . .
        . . . . . b 4 b b b 4 4 b . . .
        . . . . b d 5 5 5 4 b 4 4 b . .
        . . . . b 3 2 3 5 5 4 e 4 4 b .
        . . . b d 2 2 2 5 7 5 4 e 4 4 e
        . . . b 5 3 2 3 5 5 5 5 e e e e
        . . b d 7 5 5 5 3 2 3 5 5 e e e
        . . b 5 5 5 5 5 2 2 2 5 5 d e e
        . b 3 2 3 5 7 5 3 2 3 5 d d e 4
        . b 2 2 2 5 5 5 5 5 5 d d e 4 .
        b d 3 2 d 5 5 5 d d d 4 4 . . .
        b 5 5 5 5 d d 4 4 4 4 . . . . .
        4 d d d 4 4 4 . . . . . . . . .
        4 4 4 4 . . . . . . . . . . . .
        `),
    miniMenu.createMenuItem("Donut", img`
        . . . . . . b b b b a a . . . .
        . . . . b b d d d 3 3 3 a a . .
        . . . b d d d 3 3 3 3 3 3 a a .
        . . b d d 3 3 3 3 3 3 3 3 3 a .
        . b 3 d 3 3 3 3 3 b 3 3 3 3 a b
        . b 3 3 3 3 3 a a 3 3 3 3 3 a b
        b 3 3 3 3 3 a a 3 3 3 3 d a 4 b
        b 3 3 3 3 b a 3 3 3 3 3 d a 4 b
        b 3 3 3 3 3 3 3 3 3 3 d a 4 4 e
        a 3 3 3 3 3 3 3 3 3 d a 4 4 4 e
        a 3 3 3 3 3 3 3 d d a 4 4 4 e .
        a a 3 3 3 d d d a a 4 4 4 e e .
        . e a a a a a a 4 4 4 4 e e . .
        . . e e b b 4 4 4 4 b e e . . .
        . . . e e e e e e e e . . . . .
        . . . . . . . . . . . . . . . .
        `),
    miniMenu.createMenuItem("Cake", img`
        . . . . . . . . . . b b b . . .
        . . . . . . . . b e e 3 3 b . .
        . . . . . . b b e 3 2 e 3 a . .
        . . . . b b 3 3 e 2 2 e 3 3 a .
        . . b b 3 3 3 3 3 e e 3 3 3 a .
        b b 3 3 3 3 3 3 3 3 3 3 3 3 3 a
        b 3 3 3 d d d d 3 3 3 3 3 d d a
        b b b b b b b 3 d d d d d d 3 a
        b d 5 5 5 5 d b b b a a a a a a
        b 3 d d 5 5 5 5 5 5 5 d d d d a
        b 3 3 3 3 3 3 d 5 5 5 d d d d a
        b 3 d 5 5 5 3 3 3 3 3 3 b b b a
        b b b 3 d 5 5 5 5 5 5 5 d d b a
        . . . b b b 3 d 5 5 5 5 d d 3 a
        . . . . . . b b b b 3 d d d b a
        . . . . . . . . . . b b b a a .
        `),
    miniMenu.createMenuItem("Ice Cream", img`
        . . . . . 3 3 b 3 3 d d 3 3 . .
        . . . . 3 1 1 d 3 d 1 1 1 1 3 .
        . . . 3 d 1 1 1 d 1 1 1 d 3 1 3
        . . 3 d d 1 1 1 d d 1 1 1 3 3 3
        . 3 1 1 d 1 1 1 1 d d 1 1 b . .
        . 3 1 1 1 d 1 1 1 1 1 d 1 1 3 .
        . b d 1 1 1 d 1 1 1 1 1 1 1 3 .
        . 4 b 1 1 1 1 d d 1 1 1 1 d 3 .
        . 4 4 d 1 1 1 1 1 1 d d d b b .
        . 4 d b d 1 1 1 1 1 1 1 1 3 . .
        4 d d 5 b d 1 1 1 1 1 1 1 3 . .
        4 5 d 5 5 b b d 1 1 1 1 d 3 . .
        4 5 5 d 5 5 d b b b d d 3 . . .
        4 5 5 5 d d d d 4 4 b 3 . . . .
        . 4 5 5 5 4 4 4 . . . . . . . .
        . . 4 4 4 . . . . . . . . . . .
        `),
    miniMenu.createMenuItem("Strawberry", img`
        . . . . . . . 6 . . . . . . . .
        . . . . . . 8 6 6 . . . 6 8 . .
        . . . e e e 8 8 6 6 . 6 7 8 . .
        . . e 2 2 2 2 e 8 6 6 7 6 . . .
        . e 2 2 4 4 2 7 7 7 7 7 8 6 . .
        . e 2 4 4 2 6 7 7 7 6 7 6 8 8 .
        e 2 4 5 2 2 6 7 7 6 2 7 7 6 . .
        e 2 4 4 2 2 6 7 6 2 2 6 7 7 6 .
        e 2 4 2 2 2 6 6 2 2 2 e 7 7 6 .
        e 2 4 2 2 4 2 2 2 4 2 2 e 7 6 .
        e 2 4 2 2 2 2 2 2 2 2 2 e c 6 .
        e 2 2 2 2 2 2 2 4 e 2 e e c . .
        e e 2 e 2 2 4 2 2 e e e c . . .
        e e e e 2 e 2 2 e e e c . . . .
        e e e 2 e e c e c c c . . . . .
        . c c c c c c c . . . . . . . .
        `),
    miniMenu.createMenuItem("Cherry", img`
        . . . . . . . . . . . 6 6 6 6 6
        . . . . . . . . . 6 6 7 7 7 7 8
        . . . . . . 8 8 8 7 7 8 8 6 8 8
        . . e e e e c 6 6 8 8 . 8 7 8 .
        . e 2 5 4 2 e c 8 . . . 6 7 8 .
        e 2 4 2 2 2 2 2 c . . . 6 7 8 .
        e 2 2 2 2 2 2 2 c . . . 8 6 8 .
        e 2 e e 2 2 2 2 e e e e c 6 8 .
        c 2 e e 2 2 2 2 e 2 5 4 2 c 8 .
        . c 2 e e e 2 e 2 4 2 2 2 2 c .
        . . c 2 2 2 e e 2 2 2 2 2 2 2 e
        . . . e c c e c 2 2 2 2 2 2 2 e
        . . . . . . . c 2 e e 2 2 e 2 c
        . . . . . . . c e e e e e e 2 c
        . . . . . . . . c e 2 2 2 2 c .
        . . . . . . . . . c c c c c . .
        `),
    miniMenu.createMenuItem("Taco", img`
        . . . . . . . e e e e . . . . .
        . . . . . e e 4 5 5 5 e e . . .
        . . . . e 4 5 6 2 2 7 6 6 e . .
        . . . e 5 6 6 7 2 2 6 4 4 4 e .
        . . e 5 2 2 7 6 6 4 5 5 5 5 4 .
        . e 5 6 2 2 8 8 5 5 5 5 5 4 5 4
        . e 5 6 7 7 8 5 4 5 4 5 5 5 5 4
        e 4 5 8 6 6 5 5 5 5 5 5 4 5 5 4
        e 5 c e 8 5 5 5 4 5 5 5 5 5 5 4
        e 5 c c e 5 4 5 5 5 4 5 5 5 e .
        e 5 c c 5 5 5 5 5 5 5 5 4 e . .
        e 5 e c 5 4 5 4 5 5 5 e e . . .
        e 5 e e 5 5 5 5 5 4 e . . . . .
        4 5 4 e 5 5 5 5 e e . . . . . .
        . 4 5 4 5 5 4 e . . . . . . . .
        . . 4 4 e e e . . . . . . . . .
        `)
)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Rows, 3)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Columns, 5)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.BackgroundColor, 1)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Border, 1)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.BorderColor, 11)
myMenu.setStyleProperty(miniMenu.StyleKind.DefaultAndSelected, miniMenu.StyleProperty.IconOnly, 1)
myMenu.setStyleProperty(miniMenu.StyleKind.Selected, miniMenu.StyleProperty.Background, 9)
myMenu.setStyleProperty(miniMenu.StyleKind.Title, miniMenu.StyleProperty.Foreground, 15)
myMenu.setStyleProperty(
    miniMenu.StyleKind.Title, 
    miniMenu.StyleProperty.Border, 
    miniMenu.createBorderBox(0, 0, 0, 2)
)
myMenu.setStyleProperty(miniMenu.StyleKind.Title, miniMenu.StyleProperty.Background, 1)
myMenu.setStyleProperty(miniMenu.StyleKind.Title, miniMenu.StyleProperty.BorderColor, 11)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Width, 102)
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.Height, 73)
myMenu.bottom = 100
myMenu.left = 30
myMenu.setTitle("Apple")
myMenu.onSelectionChanged(function (selection, selectedIndex) {
    myMenu.setTitle(selection)
})
```

### createBorderBox

Creates a border box with different widths on each side for use with the `set menu style property` and `set style property` functions. This function returns a special number in which the border box widths have been encoded.

```js
miniMenu.createBorderBox(left: number, top: number, right: number, bottom: number): number;
```

This function can be used with the following properties of the `set menu style property` block:

- border
- padding

and these properties from the `set style property` block:

- margin
- border
- padding

**_Parameters_**

- **left** - the width of the border box on the left
- **top** - the width of the border box on the top
- **right** - the width of the border box on the right
- **bottom** - the width of the border box on the bottom

**_Returns_**

A number in which the left, top, right, and bottom have been encoded.

**_Examples_**

**1. This example creates a menu and gives it a red border with different widths on each side**

```js
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Sunday"),
    miniMenu.createMenuItem("Monday"),
    miniMenu.createMenuItem("Tuesday"),
    miniMenu.createMenuItem("Wednesday"),
    miniMenu.createMenuItem("Thursday"),
    miniMenu.createMenuItem("Friday"),
    miniMenu.createMenuItem("Saturday")
)
myMenu.y = 60
myMenu.x = 80
myMenu.setMenuStyleProperty(miniMenu.MenuStyleProperty.BorderColor, 2)
myMenu.setMenuStyleProperty(
    miniMenu.MenuStyleProperty.Border, 
    miniMenu.createBorderBox(1, 2, 4, 8)
)
```


## Menu Items

### getMenuItem

```js
miniMenu.getMenuItem(menu: Sprite, index: number): MenuItem;
```

### getMenuItems

```js
miniMenu.getMenuItems(menu: Sprite): MenuItem[];
```

### removeMenuItem

```js
miniMenu.removeMenuItem(menu: Sprite, index: number): void;
```

### insertMenuItem

```js
miniMenu.insertMenuItem(menu: Sprite, item: MenuItem, index?: number): void;
```
