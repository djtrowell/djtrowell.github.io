---
title: Taskwarrior
date: 30-06-2023
feed: show
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

[Attributes](#attributes) may be used to further filter tasks.

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

### Context {#context}
Context is defined by the user, and can be used to automatically [filter](#filters) or [modify](#modifications) tasks.

A context can be created using `task context define <name> <filters>`. For example:
```bash 
$ task context define studies project:studies 
```
To remove a context, `task context delete <name>` is used.

To modify a context, use `task context context.<name>,<read/write> <filters>`.

To switch contexts, use `task context <name>`, and use `task context none` to exit all contexts.

To show the current context, `task context show` can be used. To show all defined contexts, use `task context`.

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
### Attributes {#attributes}
Attributes can be used to [filter](#filters) tasks.
```
project:<project-name>  # Specifies project 
priority:<priority>     # Specifies priority 
due:<date>              # Specifies due date  
recur:<frequency>       # Specifies recurrence frequency 
scheduled:<date>        # Specifies task start 
until:<date>            # Specifies task expiration date 
entry:<date>            # Specifies task creation date 
```

Attributes may be modified by [attribute modifiers](#attribute-modifiers).

### Attribute modifiers {#attribute-modifiers}
Attribute modifiers can be applied to [attributes](#attributes) to improve filters. The `urgency` attribute can also be used with attribute modifiers. For example:
```bash 
$ task due.before:eom priority.not:L list 
```
In this example, all tasks that are due before the end of the month and have a priority other than low will be listed.

```
.before/under/below
.after/over/above
.by
```
These expressions can be used to compare values and will differ depending on their associated attribute. For example `project.before:<project>` will select projects which come alphabetically before the stated project, while `priority.before:<priority>` will select priorities lower than the stated priority. 

Note that `.before` and its synonyms differ from `.by` in that `.by` will also include the stated item also. For example `due.before:eoy` will select tasks due before the end of the year, while `due.by:eoy` will select tasks due as late as the end of the year.

```
.none
.any 
```
`.none` will select tasks which do not have a specific attribute set. For example `task priority.none: list`. `.any` is the opposite- it will select tasks with any value for the specified attribute, but the attribute must be set.

```
.is/equals
.isnt/not 
.has/contains 
.hasnt
```
`.is` requires an exact match while `.has` searches for a substring. For example `task description.is:foo list` requires that the description text is "foo" while `task description.has:foo list` requires only that "foo" is found within the description.

`.isnt` and `.hasnt` are the opposites of `.is` and `.has` respectively.

```
.startswith
.endswith
.word 
.noword
```

`.starswith` and `.endswith` match the start and end of an attribute respectively.

`.word` requires that the attribute contain the whole specified word. For example `description.word:foo` will match a description containing "foo", but not "food".

`.noword` is the opposite of `.word`.

### Expressions {#expressions}
Operators can be used within [filter](#filters) expressions:
```
# Logical operators:
  and 
  or 
  xor 
  !
# Relational operators:
  < <=
  = ==
  != !==
  > >=
```
Parentheses may also be used define precedence, however they must be surrounded by inverted commas to prevent the shell from interpreting them and hiding them from Taskwarrior.

The `=` operator differs from the `==` operator in that `=` tests for approximate equality (i.e. same day, different hour/minute), while `==` requires exact equality. If `=` is used to compare strings, the start of the left operand must equal the right operand. The same applies to `!=` and `!==`.

---
[^1]: [Taskwarrior manual page](https://man.archlinux.org/man/task.1)
