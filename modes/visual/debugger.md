---
description: Static and Dynamic Analysis in Visual Mode.
---

# Visual Debugger Mode

## Navigation

Visual Debugger Mode offers a few ways to navigate the binary. Below are the standard navigation commands for Visual Debugger Mode.  These commands directly influence the current seek address.

* $$\uparrow$$ / `j` - Move the seek address forward one instruction.
* $$\downarrow$$ / `k` - Move the seek address back one instruction.
* $$\leftarrow$$ / `h` - Decrease the seek address by one byte.
* $$\rightarrow$$ / `l` - Increase the seek address by one byte.

{% hint style="info" %}
The most common way to navigate is to use `j` and `k` to move between instructions.  Moving one byte at a time isn't very useful.
{% endhint %}


* $$.$$ - Seek to `$eip`

## Entering Commands
