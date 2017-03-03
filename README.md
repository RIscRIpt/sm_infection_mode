# Shootmania infection mode
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)

This script enables you to make maps with *conditional landmarks* (such as gates, poles, and spawns), which change their statuses according to the current server status.
To make some landmark *conditional*, you must set its **Tag** according to the following syntaxes:

## Conditional gates
Conditional gates are gates in the map which are opened or closed depending on the current server status (for example number of infected players).

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
TODO: add documentation

## Tornado
TODO: add documentation

## Map settings
### Global gates settings
#### Allowing gates to be closed during the round
```
G:C
```
This can be read as `G`ates are allowed to be `C`losed during the round.

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
