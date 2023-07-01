---
title: Taskwarrior
date: 30-06-2023
feed: hide
---

> Taskwarrior is a command line todo list manager. It maintains a list of tasks that you want to do, allowing you to add/remove, and otherwise manipulate them.  Taskwarrior has a rich set of subcommands that allow you to do various things with it.[^1]

### Basics {#basics}
The basic structure of a command in Taskwarrior is:
```bash
$ task <filters> <command> [ <mods> | <args> ]
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
 - [reports](#report-commands)
 - [write](#write-commands)
 - [misc](#misc-commands)

##### Report commands {#report-commands}
Reports do not allow the [modification](#modifications) of tasks.

They can be modifiable, which use the standard command format:
```bash
$ task <filters> all        # Show all tasks 
$ task <filters> list       # Show details of tasks 
$ task <filters> active     # Show active tasks 
$ task <filters> overdue    # Show overdue tasks 
$ task <filters> completed  # Show completed tasks 
$ task <filters> recurring  # Show recurring tasks 
$ task <filters> newest     # Show newest tasks 
$ task <filters> oldest     # Show oldest tasks 
$ task <filters> next       # Show most urgent tasks 
$ task <filters> tags       # Show all tags used 
$ task <filters> projects   # Show all projects used
$ task <filters> ids        # Shows IDs of tasks 
$ task <filters> uuids      # Shows UUIDs of tasks
```

Other reports may be static. These have custom code and so take non-standard arguments and may or may not support filters:
```bash
$ task calendar [due|<month> <year>] [y]  # Shows a monthly calendar with the due tasks marked
                                          # 'due' will show the months starting from the earliest due task
                                          # if a month and year is provided, the shown months will start
                                          # from that month and year
                                          # 'y' will show at least one complete year 
$ task <filters> export                   # Exports all tasks in JSON format 
$ task <filters> information              # Shows data and metadata of tasks 
```

### Write commands {#write-commands}
Write commands may be used to alter aspects of tasks.

```bash
$ task add <mods>                 # Adds a new task 
$ task <filters> duplicate <mods> # Duplicates a task and allows for modifications 
$ task <filters> modify <mods>    # Modifies a task
$ task <filters> delete <mods>    # Deletes a task 
$ task <filters> purge            # Permanently removes deleted tasks
$ task <filters> start <mods>     # Marks a task as started
$ task <filters> stop <mods>      # Removes the start time from the specified task
$ task <filters> done <mods>      # Marks a task as done 
$ task <filters> prepend <mods>   # Prepends description text to a task
$ task <filters> append <mods>    # Appends description text to a task 
$ task <filters> annotate <mods>  # Adds an annotation to a task 
$ task <filters> denotate <mods>  # Deletes an annotation from a task
                                  # If the text isn't an exact match, the first partial match will be deleted
$ task import [<file> ...]        # Imports tasks in JSON format
                                  # This may either add or modify tasks 
                                  # STDIN will be read if no file specified 
```

##### Misc commands {#misc-commands}
Miscellaneous subcommands either accept no arguments or non-standard arguments.

```bash
$ task calc <expression>
```

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
[^1]: [Taskwarrior manual page](https://man.archlinux.org/man/task.1)
