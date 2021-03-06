# Shootmania infection mode
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)

This script enables you to make maps with *conditional landmarks* (such as gates, poles, and spawns), which change their statuses according to the current server status.
To make some landmark *conditional*, you must set its **Tag** according to the following syntaxes:

## Conditional gates
Conditional gates are gates in the map which are opened or closed depending on the current server status (for example number of infected players).

Conditional gates are recommended for use with the *conditional spawns*!

### Tag Syntax
```
Gate_X:Y:Z
```
Where
- `X` means when to update gates status, it can be:
  - `A`lways (during the round), or
  - `R`ound (on round start).
- `Y` means default gates status, it can be:
  - `O`pened (condition `Z` must become `True` to close the gates), or
  - `C`losed (condition `Z` must become `True` to open the gates).
- `Z` means conditions which switch default gates status to opposite. (see below)

***Please note***: by default gates cannot be closed during the round, because this makes possible that some players will be locked out from the rest of the map. But if you know what you are doing, you can enable *closing gates during the round* with a special map setting. (see below: *Allowing gates to be closed during the round*)

#### Conditions syntax
```
S?Number...
```
Where
- `S`ource : count source, it can be:
  - `P`layers (total player count on the server), or
  - `S`urvivors (survivor player count), or
  - `I`nfected (infected player count).
- `?` is a comparison sign, it can be:
  - `<` (makes expression `True` if `Source` is less than `Number`)
  - `=` (makes expression `True` if `Source` is equals to `Number`)
  - `>` (makes expression `True` if `Source` is greater than `Number`)
- `Number` which is compared to `Source`
- `...` the next condition which can be added using two following logical operators:
  - `|` OR (makes the whole expression `True` if the previous expression was `True` OR current expression is `True`);
  - `&` AND (makes the whole expression `True` if the previous expression was `True` AND current expression is `True`);
  
#### Examples
```
Gate_A:C:S<3
```
This gate is updated during the round (`A`), and it is closed by default (`C`), but when the survivor player count becomes less than `3` the gate is opened (`S<3`).

```
Gate_R:C:P>3&P<10
```
This gate is updated only each round start (`R`), and it is closed by default (`C`), but if the player count was between 4 and 9 when the round began then the gate is opened (`P>3&P<10`).

```
Gate_A:O:I=1|P<5
```
This gate is updated during the round (`A`), and it is opened by default (`O`), but if there is only one infected player OR if there's less than 5 players on the server in total then the gate is closed (`I=1|P<5`).
*Please note*: without explicitly allowing gates to be closed during the round, such gate will be always opened!

## Conditional spawns
Conditional spawns are spawns which are used by the script depending on the current server status (for example number of infected players).

Conditional spawns are recommended for use with the *conditional gates*!

### Tag syntax
```
Spawn_who:when
```
Where `who` can be:
- `P`layers (survivors and infected)
- `S`urvivors only
- `I`nfected only

And `when` matches the conditional gates syntax. (see above)

#### Examples
```
Spawn
```
All players can be spawned in this spawn landmark.

```
Spawn_I
```
Only infected players can be spawned in this spawn landmark (`I`).

```
Spawn_I:S=1
```
Only infected players can be spawned in this spawn landmark (`I`), ***and*** only if current number of survivor players equals to one (`S=1`).

```
Spawn_P:P>10
```
All players can be spawned in this spawn landmark, only if there are more than 10 players on the server (`P>10`).

## Tornado
To enable tornado in the map, some landmark with the following tag must be placed on the map:
```
Tornado_radius
```
Where `radius` is a radius of the tornado measured in `1/8` of a block.
The center of the tornado matches the center of the landmark with such tag.

*Please note*: you can put no more than one tornado in a map.

#### Example
```
Tornado_80
```
Enables tornado in the map with the radius of 10 blocks.

## Map settings
To edit some map properties, or specify infection mode settings you must place some landmark (a block which can have a tag), and edit its tag according to the following syntax:

The tag of a landmark must begin with:
```
Settings_
```
and after you can specify following settigns delimited with semicolon (`;`):

### Global gates settings
#### Allowing gates to be closed during the round
```
G:C
```
This can be read as `G`ates are allowed to be `C`losed during the round.

### Map water effects
***Please note***: map water effects are applied only if the player is swimming in the water. Touching the water does nothing.
```
W:players=effect
```
Where `players` can be:
  - `S`urvivors;
  - `I`nfected.

And `effect` can be:
- `E`liminate (a player gets eliminated)
- `P`rotect (a player cannot be hit or infected)
- `I`nfect (a survivor player gets infected)
- `H`eal (a player is healed like standing on a healpad)

#### Example
```
Settings_W:S=I;W:I=H;W:I=P
```
Survivors get infected in the water (`S=I`), infected players are healed in the water (`I=H`), infected players cannot be hit in the water (`I=P`).

## How to change landmark tag?
1. Enable block properties mode:

  ![image](https://raw.githubusercontent.com/RIscRIpt/sm_infection_mode/master/documentation/images/open_landmark_settings.png "Enable block properties mode")
2. Click on some landmark.
3. Edit tag

  ![image](https://raw.githubusercontent.com/RIscRIpt/sm_infection_mode/master/documentation/images/edit_landmark_tag.png "Edit tag")
4. Click *OK*

## Credits
- [Mewin](https://mewin.de/) for the original mode;
- [Dommy](https://github.com/domino54/) for helping with undocumented maniascript;
