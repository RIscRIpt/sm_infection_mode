# Shootmania infection mode

## Conditional gates
Conditional gates are gates in the map which are opened / closed depending on the current server status.
### Syntax
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
- `Z` means conditions which switch default gates status to opposite

#### Condition syntax
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
  - `|` AND (makes the whole expression `True` if the previous expression was `True` AND current expression is `True`);
  
  #### Examples
  ```
  Gate_A:C:S<3
  ```
  This gate is updated during the round (`A`), and it is closed by default (`C`), but when the survivor player count becomes less than `3` the gate is opened (`S<3`).
  
  ```
  Gate_R:C:P>3&P<10
  ```
  This gate is updated only each round start (`R`), and it is closed by default (`C`), but if the player count was between 4 and 9 when the round began than the gate is opened (`P>3&P<10`)
  
  ```
  Gate_A:O:I=1|P<5
  ```
  This gate is update during the round (`A`), and it is opened by default (`O`), but if there is only one infected player OR if there's less than 5 players on the server in total then the gate is closed (`I=1|P<5`).
  
## Credits
- [Mewin](https://mewin.de/) for the original mode;
- [Dommy](https://github.com/domino54/) for helping with undocumented maniascript;
