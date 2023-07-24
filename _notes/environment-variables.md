---
title: Environment variables
date: 02-07-2023
feed: hide
---

> An environment variable is a named object that contains data used by on or more applications. In simple terms, it is a variable with a name and a value. The value of an environment variable can for example be the location of all executable files in the file system, the default editor  that should be used, or the system locale settings.[^1]

### Utilities {#utilities}
In Arch Linux, the `coreutils` package contains the programs `printenv` and `env`.

The `printenv` program may be used to show current environment variables and their respective values:
```bash
$ printenv 
```
It should be noted that some environment variables are user-specific.

The `env` program may be used to set environment variables for a specific program. For example:
```bash
$ env EDITOR=vim echo $EDITOR
vim 
```
In this example, the `EDITOR` environment variable will be set to `vim` only for the process of `echo $EDITOR` (i.e. without changing the `EDITOR` environment variable).

Each process stores its environment variables in its `/proc/<PID>/environ` file, listing each variable delimited by a null character.

### Definition {#definition}
Environment variables can be defined either [globally](#global-definition) or [per user](#user-definition).

---
[^1]: [ArchWiki - Environment variables](https://wiki.archlinux.org/title/environment_variables)
