# printc

With this plugin you can print any color within the rgb color space via an interface that
is as simple as a regular print statement.

This simple tool provides an abstraction layer on top of terminal ANSI rgb escape codes,
making the addition of colorized output to your functions, shell scripts, and/or
interactive terminal in zsh a piece of cake. There is support for any of the colors which
can be achieved via the form R G B, where R, G and B are any numeric value
between 0 and 255, representing the red, green, blue color space values respectively.
Users of this plugin are able to issue a  `printc` statement, followed by the previous
mentioned rgb values, followed by the text to be printed. There is also 36 built in colors
which can be accessed via tab auto-complete. And there is support for bold, italic,
blinking, and underline text.

![screenshot](https://imgur.com/K0FVGzr.png)


#### Table of Contents
 - [Setup](#Setup)
 - [Configuration](#Configuration)
    - [Requirements](#Requirements)
    - [Italics Setup](#Italics-Setup)
 - [Usage](#Usage)
    - [Options](#Options)
    - [General Structure of Command](#General-Structure-of-Command)


## Setup
`git clone https://github.com/philFernandez/printc-.git`

Put the `printc` file in a location that is in your $PATH. Perhaps `~/bin` or `~/.bin`
or any other directory which you have access to, and is in your $PATH. There is an
autocomplete function included called `_printc`. Put that file in any location
which is under your $fpath. You may need to delete the ~/.zcompdump file, and reload
zsh. This will re-build the ~/.zcompdump file, and should pick up the new completion
function as long as it is stored in a directory that is in the $fpath. The autocomplete
is nice because it lets you tab through all of the built in colors included
with this plugin.

## Configuration

### Requirements
Your terminal emulator must support 256 color. If you want to leverage italic text,
depending on your terminal emulator you will likely need to add support for displaying
italic text. The process is very simple, and I'll include instruction on how to do so. As
long as your terminal emulator supports, and is set up for 256 color, which almost all are
now-a-days, you can use the color aspect of the plugin, even without the italic
functionality.

At the least your TERM environment variable must be set as so:
```
export TERM=xterm-256color
```

It is possible that this is all that will be needed for the italic functionality to work
as well. I do recommend trying and seeing if italics work. It it does not, just follow the
simple steps below.

### Italics Setup

---
##### An Aside on Italics
 * It is likely that the italics as implemented in the `printc` plugin
   will work without needing the setup described below.
 * This section is here as reference for people who *do* have problems getting
   italics to work in the terminal.
 * This method of enabling italics may cause problems.
 * The most common problems arise for those who ssh into remote machines.
 * If you do use this method, and end up with ssh problems, because the terminal
   reports a TERM value that the server doesn't understand, it can probably be remedied as
   follows...


 Logging into a remote server with something like `ssh user@remote.host`

 Can be changed to `TERM=xterm-256color ssh user@remote.host`

 Prepending `TERM=xterm-256color` just before your normal ssh command will pass along a
 TERM value that the server understands, without impacting your local TERM value. This can
 be aliased for convenience.

---

 1) Create a directory `~/.terminfo`

 2) Create a file inside `~/.terminfo` called `xterm-256color-italics.terminfo`

 3) Place the following contents inside `xterm-256color-italics.terminfo` *exactly* as
 they are shown:

 ```
 xterm-256color-italic|xterm with 256 colors and italic,
 sitm=\E[3m, ritm=\E[23m,
 use=xterm-256color,
 ```

 4) Inside `~/.terminfo` run the command `tic xterm-256color-italics.terminfo`

 5) Set your TERM environment variable as so:
 ```
 export TERM=xterm-256color-italic
 ```
 You can export TERM in your `~/.zshrc` or `~/.zshenv` or many other ways. If you're an
 iTerm2 user you can do it through the GUI terminal emulator settings there. It doesn't
 matter how you do it, as long as the TERM environment variable is set and exported to
 one of the above mentioned values, either `xterm-256color` or `xterm-256color-italic`.
 Again, I recommend trying things out with `xterm-256color` and seeing if they work for
 you *before* following the italics setup.


 As an aside, I have not tested this inside TMUX, but it should work there as long as the
 environment is set up to properly handle color and italics.

### Blinking Text

Many terminals have no support for blinking text. This may be considered a feature of
those terminals rather than a bug.

# Usage

### Options
  | Option              | Function               |
  | :-----------------: | --------------         |
  | -b                  | Bold                   |
  | -u                  | Underline              |
  | -i                  | Italic                 |
  | -k                  | Blink                  |
  | -C `color`          | Specify built in color |
  | -l                  | List built in colors   |
  | -n                  | No newline             |
  | -h                  | Display help page      |

### General Structure of Command
 * `printc [-b] [-u] [-i] [-k] [-n] <0-255> <0-255> <0-255> "Colorized Text to Display"`
     * This is the structure used when specifying a color with RGB values.
     * Note the absense of quotes around the RGB numbers, this is required.
     * Note the inclusion of quotes around the intended output, also required.
         * As usual double quotes will allow parameter expansion.
     * The RGB values must come after any options, and before the intended output.
 * `printc [-b] [-u] [-i] [-k] [-n] -C <built in color> "Colorized Text to Display"`
     * This is the structure used when specifying a color that is built in to the plugin.
     * Quoting the intended output is not required when using built in color options.
 * Options can be given in any order, and chained together, such as
 `-buinC <built in color>`, so long as `built in color` follows immediately after `-C`,
   and in the case of using RGB values, they must come after any options that are given.

