---
title : Vim
date : 30-06-2023
feed : hide
---

> Vim (a contraction of Vi IMproved) is a free and open-source, screen-based text editor program. It is an improved clone of Bill Joy's vi. Vim's author, Bram Moolenaar, derived Vim from a port of the Stevie editor for Amiga and released a version to the public in 1991. Vim is designed for use both from a command-line interface and as a standalone application in a graphical user interface.[^1]

### Normal mode {#normal-mode}
Vim will be in normal mode by default. To switch to it, press `escape`. 

##### Deleting text
Note that each of the following actions will copy the deleted text into register x.
| shortcut    | description                                                              |
| ---         | ---                                                                      |
| `x`         | delete character under cursor                                            |
| `X`         | delete character before cursor                                           |
| `d[motion]` | delete characters between initial position and new position              |
| `dd`        | delete line                                                              |
| `[count]J`  | delete newline and whitespace between current line and [count] - 1 lines |


---

[^1]: https://en.wikipedia.org/wiki/Vim_(text_editor)?wprov=sfti1
