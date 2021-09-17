# ugh.
a collection of silly tools for x11 written in c99, requiring only stdlib and xlib.

## rationale
a dumb experiment in more unix-like tools for x11.  turning you and your shell into the window manager.  because why let the machines do all the work for you?  :angry::angry::angry:

i didn't bother writing a hotkey daemon or launcher because those exist in spades and maybe you don't need one.  maybe you just need a few terminals and a browser open and full screen.  besides, start a terminal before you run ```ugh-fm``` in your .xsession/.xinitrc and you can make the rest happen yourself.

#### what ugh does not do
aside from the focus manager, *ugh* does not manage your windows.  there are no decorations or buttons or menus or keymappings to learn or configuration files.  hell, it doesn't even hog up memory.  ```ugh-fm``` consumes a few kilobytes of RAM and only a few xlib calls are made.  it will spend most of it's time blocked waiting for a few different mouse-related x events.

by extension, none of these utilize the Substructure or Structure masks with the x server, so other utilities could be written that do use them to control behavior as well as draw decorations around the windows or something.

## provided tools
note: most of the supplied tools take a window id as an argument. all of them will either read the id streamed in through stdin, or via the positional arguments of the command when it's run. they do this automatically so you can use them either way without needing to change any flags or commands.

this allows the user to pipe commands together with the window id never having to be written out, or if you're doing multiple commands to the same window you can specify it from a shell variable.

in addition, those same utilities will output the input window id back to stdout to allow further chaining of commands.  they will all accept the optional ```-q``` flag to silence the output.

#### ugh-fm 
```usage: ugh-fm```

the ugh focus manager.  run this in place of a wm in your .xsession/.xinitrc
provides the following revolutionary and paradigm-shifting features:
- a focus-follows-mouse model 
- infinitely loops so your X connection stays open even with no applications running
- "cleanly" closes all windows and exits when it receives SIGHUP

#### ugh-list
```usage: ugh-list```

lists to stdout all top-level windows in the following format (1 entry per line):

```<window id>,<window title>,<x geometry string>```

#### ugh-id
```usage: ugh-id <search term>```

searches the top-level windows for the first window whose title contains the search term, then outputs the window id to stdout.

#### ugh-circ
```usage: ugh-circ [{1,0}]```

circulates the top-level windows up(1) or down(0).  if no direction is provided, it circulates down.

#### ugh-raise
```usage: ugh-raise [-q] [<window id>]```

raises \<window id> to the top of the stack.

#### ugh-lower
```usage: ugh-lower [-q] [<window id>]```

lowers \<window id> to the bottom of the stack.

#### ugh-move
```usage: ugh-move [-q] [<window id>] <x geometry string>```

moves and/or resizes \<window id> according to \<x geometry sting>.

#### ugh-max
```usage: ugh-max [-q] [<window id>]```

raises \<window id> to the top of the stack and resizes it to the size of the display.

#### ugh-hide
```usage: ugh-hide [-q] [<window id>]```

unmaps \<window id>.  still visible by ugh-list and ugh-id

#### ugh-unhide
```usage: ugh-unhide [-q] [<window id>]```

remaps \<window id> and brings it to the top of the stack.

## license
***"intellectual property is robbery."*** -- proudhon, probably.
