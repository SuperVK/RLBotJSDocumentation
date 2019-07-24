# RLBot JavaScript Documentation

**Table of Contents** _generated with_ [Doctoc](https://github.com/thlorenz/doctoc)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Links](#links)
- [Bot Making](#bot-making)
  - [Starting a JavaScript Bot](#starting-a-javascript-bot)
  - [Moving](#moving)
- [API Reference](#api-reference)
  - [Objects](#objects)
    - [`BaseAgent`](#baseagent)
    - [`Manager`](#manager)
    - [`SimpleController`](#simplecontroller)
    - [`quickChats`](#quickchats)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Links

-   [RLBot](https://github.com/rlbot/rlbot)
-   [RLBotJS](https://github.com/supervk/rlbotjs)
-   [RLBot JavaScript Example](https://github.com/supervk/rlbotjavascriptexample)

## Bot Making

### Starting a JavaScript Bot

The first thing you'll want to do is start a class that extends the `rlbot.BaseAgent` class. This allows you to easily accomplish things that are harder without extending the `BaseAgent`, like sending quick chats. You'll also need to make a new `Manager` that has a parameter of the class extending `BaseAgent`.

Example:

```javascript
const { BaseAgent, SimpleController } = require("rlbot-test");

class MyBot extends BaseAgent {
    constructor(name, team, index, fieldInfo) {
        super(name, team, index); // pushes these all to `this`.
        console.log(this.name); // Print our name to the console,  e.g. "MyBot"
    }

    getOutput(gameTickPacket, ballPrediction, fieldInfo) {
        return SimpleController(); // Do nothing in-game
    }
}

const manager = new Manager(MyBot); // This makes a manager
manager.start(); // This starts the manager
```

### Moving

To move your bot, you'll have to change the values of the `SimpleController` that we returned earlier. It has the following values:

-   `throttle`: Default is `0`. To go forward, change this to `1`. To go backwards, change this to `-1`. Setting this to `0` will stop the throttle. **NOTE**: Setting this to `0` does not slow the car down very well. If you want to brake, change the throttle to the opposite from what it was. E.g. when this is set to `1`, set this to `-1` until the bot has slowed down.
-   `steer`: Default is `0`. To steer right, change this to `1`. To steer left, change this to `-1`. Setting this to `0` will steer forwards.
-   `pitch`: Default is `0`. To tip the bot forwards, change this to `-1`. To tip the bot backwards, change this to `1`. **NOTE**: Changing this is a way to dodge.
-   `roll`: Default is `0`. To roll clockwise(relative to the back of the car), change this to `-1`. To roll anti-clockwise(relative to the back of the car), change this to `1`. **NOTE**: Changing this is a way to dodge.
-   `yaw`: Default is `0`. To rotate the bot clockwise(relative to the top of the car), change this to `-1`. To rotate the bot clockwise(relative to the top of the car), change this to `1`. **NOTE**: Changing this is a way to dodge.
-   `boost`: Default is `false`. To use boost, change this to `true`. To not use boost, change this to `false`.
-   `jump`: Default is `false`. To jump, change this to `true`. To not jump, change this to `false`
-   `handbrake`: Default is `false`. To powerslide, change this to `true`. To not powerslide, change this to `false`.
-   `useItem`: Default is `false`. To use a rumble item, change this to `true`. To not use a rumble item, change this to `false`.

## API Reference

### Objects

#### `BaseAgent`

**Importing it**: `require("rlbot-test").BaseAgent`

**Arguments**:

-   `name`: The bot name.
-   `team`: The bot's team. `0` = Blue, `1` = Orange
-   `index`: The bot's index in the `gameTickPacket.players` array.
-   `fieldInfo`: The field info packet. Should be a `FieldInfo` object.

**Methods**:

-   `getOutput`: Gets the bot output. Should return a `SimpleController` **Should be overwritten.**
    -   Arguments it will be called with:
        -   `gameTickPacket`: The game packet. Should be a `GameTickPacket` object.
        -   `ballPrediction`: The ball prediction. Should be a `BallPrediction` object.
        -   `fieldInfo`: The field info packet. Should be a `FieldInfo` object.
-   `sendQuickChat`: \***DO NOT OVERWRITE THIS, IT CAN DISQUALIFY YOUR BOT FROM A TOURNAMENT!!!!!**\*. Sends a quick chat.
    -   Arguments:
        -   `QuickChatSelection`: A number. Should be one in [`quickChats`](#quickchats).
        -   `teamOnly`: A boolean. Whether or not the quick chat should be sent so that only the bot's team can see it.
-   `setGameState`: \***DO NOT OVERWRITE IF YOU DON'T KNOW WHAT YOU ARE DOING**\*. Sets the game state.
    -   Arguments:
        -   `gameState`: The game state to set the current state to. Should be a `GameState` object.

#### `Manager`

**Importing it**: `require("rlbot-test").Manager`

**Arguments**:

-   `botClass`: The `BaseAgent` or custom class containing the getOutput method of the bot.
-   `port`: \***DO NOT PROVIDE**\* The port the manager will listen to for the python agent. **This will be read from `rlbot-test\src\pythonAgent\port.cfg`**
-   `ip`: \***DO NOT PROVIDE**\* The ip the manager will run on.

**Methods**:

\***MOST THESE METHODS SHOULD NEVER HAVE TO BE CALLED MANUALLY!!!!! IF THEY SHOULD BE CALLED MANUALLY, THAT WILL BE NOTED!!!!! WRONG USE OF MOST OF THESE FUNCTIONS CAN DISQUALIFY YOUR BOT FROM A TOURNAMENT!!!!!**\*

-   `loadInterface`: Loads the RLBot DLL into the manager.
-   `async waitUntilInitialized`: Waits until the DLL is initialized.
-   `start`: \***SHOULD BE CALLED MANUALLY**\* Starts the manager.
-   `sendInput`: Sends input of the bots to the DLL. \***WRONG USE OF THIS FUNCTION CAN GET YOUR BOT DISQUALIFED**\*
    -   Arguments:
        -   `botIndex`: The bot-to-send-input's index in the `gameTickPacket.players` array.
        -   `botInput`: The `SimpleController` of the bot-to-send-input.
-   `sendQuickChat`: Sends a quick chat. \***WRONG USE OF THIS FUNCTION CAN GET YOUR BOT DISQUALIFED**\*
    -   Arguments
        -   `QuickChatSelection`: A number. Should be one in [`quickChats`](#quickchats).
        -   `index`: The bot-to-send-quick-chat's index in the `gameTickPacket.players` array.
        -   `teamOnly`: A boolean. Whether or not the quick chat should be sent so that only the bot's team can see it
-   `setGameState`: Sets the game state. \***USE OF THIS FUNCTION CAN GET YOUR BOT DISQUALIFED**\*
    -   Arguments:
        -   `gameState`: The game state to set the current state to. Should be a `GameState` object.
-   `updateBots`: Gets the output and sends input to each of the bots it is handling.
-   `getGameTickPacket`: Gets the current game packet. Should be a `GameTickPacket` object.
-   `getBallPrediction`: Gets the current ball prediction. Should be a `BallPrediction` object.
-   `getFieldInfo`: Gets the current field info. Should be a `FieldInfo` object.

#### `SimpleController`

**Importing it**: `require("rlbot-test").SimpleController`

**Variables**:

-   `throttle`: Default is `0`. To go forward, change this to `1`. To go backwards, change this to `-1`. Setting this to `0` will stop the throttle. **NOTE**: Setting this to `0` does not slow the car down very well. If you want to brake, change the throttle to the opposite from what it was. E.g. when this is set to `1`, set this to `-1` until the bot has slowed down.
-   `steer`: Default is `0`. To steer right, change this to `1`. To steer left, change this to `-1`. Setting this to `0` will steer forwards.
-   `pitch`: Default is `0`. To tip the bot forwards, change this to `-1`. To tip the bot backwards, change this to `1`. **NOTE**: Changing this is a way to dodge.
-   `roll`: Default is `0`. To roll clockwise(relative to the back of the car), change this to `-1`. To roll anti-clockwise(relative to the back of the car), change this to `1`. **NOTE**: Changing this is a way to dodge.
-   `yaw`: Default is `0`. To rotate the bot clockwise(relative to the top of the car), change this to `-1`. To rotate the bot clockwise(relative to the top of the car), change this to `1`. **NOTE**: Changing this is a way to dodge.
-   `boost`: Default is `false`. To use boost, change this to `true`. To not use boost, change this to `false`.
-   `jump`: Default is `false`. To jump, change this to `true`. To not jump, change this to `false`
-   `handbrake`: Default is `false`. To powerslide, change this to `true`. To not powerslide, change this to `false`.
-   `useItem`: Default is `false`. To use a rumble item, change this to `true`. To not use a rumble item, change this to `false`.

#### `quickChats`

**Importing it**: `require("rlbot-test").quickChats`

**Variables**:

-   `information`
    -   `IGotIt`: `0` - I got it!
    -   `NeedBoost`: `1` - Need boost!
    -   `TakeTheShot`: `2` - Take the shot!
    -   `Defending`: `3` - Defending!
    -   `GoForIt`: `4` - Gor for it!
    -   `Centering`: `5` - Centering!
    -   `AllYours`: `6` - All yours!
    -   `InPosition`: `7` - In position!
    -   `Incoming`: `8` - Incoming!
    -   `NiceShot`: `9` - Nice shot!
    -   `GreatPass`: `10` - Great pass!
    -   `Thanks`: `11` - Thanks!
    -   `WhatASave`: `12` - What a save!
    -   `NiceOne`: `13` - Nice one!
    -   `WhatAPlay`: `14` - What a play!
    -   `GreatClear`: `15` - Great clear!
    -   `NiceBlock`: `16` - Nice block!
-   `compliments`
    -   `NiceShot`: `9` - Nice shot!
    -   `GreatPass`: `10` - Great pass!
    -   `Thanks`: `11` - Thanks!
    -   `WhatASave`: `12` - What a save!
    -   `NiceOne`: `13` - Nice one!
    -   `WhatAPlay`: `14` - What a play!
    -   `GreatClear`: `15` - Great clear!
    -   `NiceBlock`: `16` - Nice block!
-   `reactions`
    -   `OMG`: `17` - OMG!
    -   `Noooo`: `18` - Noooo!
    -   `Wow`: `19` - Wow!
    -   `CloseOne`: `20` - Close one.
    -   `NoWay`: `21` - No way!
    -   `HolyCow`: `22` - Holy cow!
    -   `Whew`: `23` - Whew.
    -   `Siiiick`: `24` - Siiiick.
    -   `Calculated`: `25` - Calculated.
    -   `Savage`: `26` - Savage!
    -   `Okay`: `27` - Okay.
-   `apologies`
    -   `Cursing`: `28` - [I forgot]
    -   `NoProblem`: `29` - No problem!
    -   `Whoops`: `30` - Whoops.
    -   `Sorry`: `31` - Sorry!
    -   `MyBad`: `32` - My bad.
    -   `Oops`: `33` - Oops.
    -   `MyFault`: `34` - My fault.
-   `postGame`
    -   `Gg`: `35` - GG
    -   `WellPlayed`: `36` - Well played.
    -   `ThatWasFun`: `37` - That was fun!
    -   `Rematch`: `38` - Rematch!
    -   `OneMoreGame`: `39` - One more game!
    -   `WhatAGame`: `40` - What a game!
    -   `NiceMoves`: `41` - Nice moves!
    -   `EverybodyDance`: `42` - Everybody dance!
-   `custom`
    -   `Toxic_WasteCPU`: `44` - Waste of CPU cycles
    -   `Toxic_GitGut`: `45` - Git gud\*
    -   `Toxic_DeAlloc`: `46` - De-Allocate Yourself
    -   `Toxic_404NoSkill`: `47` - 404: Your skill not found
    -   `Toxic_CatchVirus`: `48` - Get a virus
    -   `Useful_Passing`: `49` - Passing!
    -   `Useful_Faking`: `50` - Faking!
    -   `Useful_Demoing`: `51` - Demoing!
    -   `Useful_Bumping`: `52` - BOOPING
    -   `Compliments_TinyChances`: `53` - The chances of that was 47525 to 1\*
    -   `Compliments_SkillLevel`: `54` - Who upped your skill level?
    -   `Compliments_proud`: `55` - Your programmer should be proud
    -   `Compliments_GC`: `56` - You're the GC of Bots
    -   `Compliments_Pro`: `57` - Are you <Insert Pro>Bot? \*
