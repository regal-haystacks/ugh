# ugh
lazy, scriptable x11 wm tools
## rationale
maybe sometimes you don't need a de.  or a wm.  maybe you're like me and rarely have more open than a few terminals and you get bored.  

while ```ugh``` proper definitely should not be run with another wm/de running, the remaining tools just parse input and issue X server requests for window manipulation.  they're meant to be used in conjunction with shell scripts rather than typing all the chains together every time.

don't like a certain behavior?  these are tiny programs in C99.  just change them and recompile.  it takes a second or two to recompile all of these on my 800MHz laptop.  you don't have any excuse.

## requirements
these being x11 tools means you need to be running x11, but in terms of compiling the source you just need the xlib and stdlib headers.  nothing fancy here.

these were written, tested, and used on *openbsd/i386 6.9*.  given the rather conservative nature of the openbsd project, i'd imagine these tools should compile on any \*nix system supported by x11 with no code modification.  but i also haven't really used linux since before smartphones were a thing so idfk.

## tools provided by ugh
note: most of the supplied tools take a window id as an argument.  all of these tools will either read the id streamed in through stdin, or via the positional arguments of the command when it's run.  they do this automatically so you can use them either way.

this allows the user to pipe commands together with the window id never having to be written out, or if you're doing multiple commands to the same window you can specify it from a shell variable or by hand.

### ugh
```ugh``` holds open the X connection, acting as a *de facto* wm.  there's a few key combinations supported:

```ctrl + mod1 + enter``` - spawns the lancher_cmd

```mod1 + [shift +] tab``` - cycle through visible windows

```ctrl + mod1 + q``` - cleanly quits ugh: destroys remaining windows and releases its connection to the xserver

these are all defined in ```ugh.c```.  they're rather hardcoded in, but there's so few of them it's trivial to change.

the ```launcher_cmd``` variable near the head of the file determines what is launched by ugh.  in the current source, it just launches ```dmenu_run``` with default options.

other than that all it does is lazily adjust the input focus based on window creation and mouse enter events.

### ugh-list
```ugh-list``` simply lists all the children of the root window to stdout in the format: window-id,window title,x,y,w,h

using csv makes it easy to use ```cut``` or ```awk``` to strip information from the results

### ugh-id
```ugh-id <search term>``` takes a word and searches the children of the root window for any window title containing the search term, then outputs the window id to stdout

it's similar to using ```ugh-list``` and ```grep``` together, but only outputs the window-id of the first match, or is silent on no match.

TODO: cycle through all the child windows as well.

### ugh-top
```ugh-top``` takes the top-most window on the stack and outputs the window-id to stdout.  if no windows are visible it is silent.

TODO: cycle through all the child windows as well. 

### ugh-move
```ugh-move [window id] <x> <y>``` takes a window id as well as x,y coordinates on screen to change the window's origin.  if no id or a bad id, silently exits.

note:  this does not raise the window

### ugh-resize
```ugh-resize [window id] <w> <h>``` takes a window id as well as the new width and height to change the window's size.  if no id or a bad id, silently exits.

note:  this does not raise the window

### ugh-mar
```ugh-mar [window id] <x> <y> <w> <h>``` takes a window id as well as the new origin x,y and new width and height , then moves and resizes the window.  on no active window or no supplied id (such as from a previous call being piped in), silently exits.

note:  this does not raise the window

### ugh-hide
```ugh-hide [window id]``` takes a window id and unmaps the window, hiding it.  if no id or a bad id, silently exits.

hidden windows are still searchable via ```ugh-id``` and ```ugh-list```.

### ugh-lower
```ugh-lower [window id]``` takes a window id and moves the window to the bottom of the stack.  if no id or a bad id, silently exits.

### ugh-raise
```ugh-raise [window id]``` takes a window id and moves the window to the top of the stack.  if no id or a bad id, silently exits.

### ugh-focus
```ugh-focus [window id]``` takes a window id and sets keyboard and mouse focus to that window.  if no id or a bad id, silently exits.

### ugh-rotate
```ugh-rotate [<1|0>]``` cycles the window stack up (1) or down (0).  if no input is supplied, it cycles them up.

### ugh-iconify
```ugh-iconify [window id]``` takes a window id and iconifies that window.  if no id or a bad id, silently exits.

this is probably never going to be used.
