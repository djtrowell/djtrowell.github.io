---
title : Taskwarrior
date : 30-06-2023
feed : hide
---

> Taskwarrior  is  a  command  line  todo list manager. It maintains a list of tasks that you want to do, allowing you to add/remove, and otherwise manipulate them.  Taskwarrior has a rich set of subcommands that  allow  you  to  do  various things with it.[^1]

### Basics {#basics}
The basic structure of a command in Taskwarrior is:
```bash
task <filters> <command> [ <mods> | <args> ]
```

### Filters {#filters}
Filters can be used to apply search criteria when selecting tasks. Multiple may be used in any one command. For example:
```bash
$ task project:personal +studies physics list
```
In this example, all tasks within the `personal` project with the tag `studies` and the word "physics" in the description or annotation on the task. Note that `physics` is translated to `description.contains:physics`. `contains` is an example of an [attribute modifier](#attribute-modifiers).

Unless otherwise specified, filters will be combined with an `and` operator, but you can use `or` or `xor` if parentheses are used. Parentheses are needed to isolate the logically combined filters from other filters and/or command words. Within parentheses, regular expressions may also be used.
```bash
$ task +studies '( physics or chemistry )' <command> <mods>
$ task +exercise '( /[Bb]icep|[Tt]ricep/ )' <command> <mods>
```
Some characters may need to be escaped to avoid their special meanings within the shell in use.

Filters may be used to select using a task's ID or UUID. Multiple can be specified using space-separated lists of IDs, UUIDs or ID ranges.
```bash
$ task 1 2 3  <command> <mods>
$ task 1-3    <command> <mods>
$ task 1 3-5  <command> <mods> 
$ task [UUID] <command> <mods>
```

### Commands {#commands}
Taskwarrior supports different types of command:
 - [read](#read-commands)
 - [write](#write-commands)
 - [misc](#misc-commands)
 - [helper](#helper-commands)

##### Read commands {#read-commands}
Read commands (reports) do not allow the [modification](#modifications) of tasks. 

| command                   | action                                                                                                                                                   |                      |
| ---                       | ---                                                                                                                                                      |                      |
| `task --version`          | Displays version of Taskwarrior                                                                                                                          |                      |
| `task <filters> all`      | Shows **all** tasks matching the filter                                                                                                                  |                      |
| `task <filters> active`   | Shows tasks matching the filter that are **started but not completed**                                                                                   |                      |
| `task <filters> blocked`  | Shows tasks matching the filter that are **blocked by other tasks**                                                                                      |                      |
| `task <filters> blocking` | Shows tasks matching the filer that are **blocking other tasks**                                                                                         |                      |
| `task <filters> burndown` | Shows a **graphical burndown chart** by week. `burndown.daily`, `burndown.weekly` or `burndown.monthly` can also be used. **It is affected by context.**                                                                                      |                      |
|                           | `task <filters> calendar [due\|<month> <year>] [y]`                                                                                                                                                                                               |                      |
### Modifications {#modifications}
Modifications come after the command and define the changes to apply to the selected task(s). For example:
```bash
$ task <filters> <command> project:personal
$ task <filters> <command> +homework due:tomorrow
$ task <filters> <command> description [text]
$ task <filters> <command> annotation [text]
$ task <filters> <command> /from/to/  # replace first match only
$ task <filters> <command> /from/to/g # replace all matches
```

### Attribute modifiers {#attribute-modifiers}

---
[^1]: Taskwarrior manual page
