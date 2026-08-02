---
title: Adding line numbers to the target assembly
description: 
published: true
date: 2026-08-02T12:57:37.973Z
tags: 
editor: markdown
dateCreated: 2026-08-02T12:39:44.535Z
---

# Adding line numbers to the target assembly

Debug information for games can contain data on what assembly corresponds to which line in the original source code. It is possible to annotate the target assembly with this.

## The .loc directive

The easiest way is to use the .loc directive.

### Syntax

`.loc <file_record> <line_number>`

`<file_record>` refers to an identifier defined by the .file directive. If you're using decomp.me, it'll automatically define one for you at the start of the target assembly:

`.file 1 "file.c"`

In this case, `<file_record>` would be 1.

### How to use

Add the .loc directive before the assembly that corresponds to the line number. It'll match all of the assembly between that and the next .loc directive.

### Example

```
glabel bgEmu77N
.loc 1 29
    /* stuff */
.loc 1 15
    /* more stuff */
.size bgEmu77N, . - bgEmu77N
```

## The STABS format

Another option is to use the STABS format.

### Syntax

`.stabn 68,0,<line_number>,<relocatable offset>`

`relocatable offset` refers to a label in the assembly.

### How to use

To use STABS, you'll need to place a .stabs directive at the start that looks like this:

`.stabs    "file.C",100,0,4,.Ltext0`

Next, to define the line numbers, place a label with the name of your choice before the assembly that corresponds to the line number. Then place a .stabn directive that refers to it somewhere before it. It'll match all of the assembly between that label and the next label that defines the start of a line.

### Example

```
glabel bgEmu77N
.stabn 68,0,29,.LM800C84E8-bgEmu77N
.LM800C84E8:
    /* stuff */
.stabn 68,0,15,.LM800C84F8-bgEmu77N
.LM800C84F8:
    /* more stuff */
.size bgEmu77N, . - bgEmu77N
```