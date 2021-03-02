---
title: Notes on terminology and shell environments
date: 2021-02-15 16:34:00 +0100
categories: [Guides]
tags: [shell, scripting, zsh, bash, OS, operating system, terminal, interpreter, iPython, command line, CLI, change shell, awk, AWK]
pin: false
---

Now that you've got a general idea about how shell scripting works, I want to expand on a few concepts that might
still be somewhat unclear. This is not strictly a part of the curriculum, but will very likely prove useful
for the lab exercises and the exam. However, feel free to skim through most of what's written below and come back later
if you need to. You can always use this page as a reference for any unfamiliar terms you encounter.
> Tip: Most browsers support `ctrl + F` / `cmd + F` to search for a keyword in a web page. <br>
> You can also navigate using the contents table on the lower right of this page.

---

Before I get into the differences between different shells and how to switch between them, I need to clear up a few
concepts so that I know we're all on the same page.

> Everytime a new concept is mentioned below, I'll put alternative names and acronyms for the same thing in parentheses, and I'll also
> provide some links for you to investigate if you should want to learn more.
<br>


### Explaining a few technical terms
An **[operating system](https://en.wikipedia.org/wiki/Operating_system)** (OS) is software that runs on your computer
(machine/console) which manages access to things such as the computer's
[memory](https://en.wikipedia.org/wiki/Computer_memory) (storage/RAM/SSD/HDD) and other computer hardware and software
resources. Some examples of different operating systems are:
- Windows
- MacOS
- Ubuntu
- Debian
- Fedora
- Android (mobile)
- iOS (mobile)

<br>

A **[terminal emulator](https://en.wikipedia.org/wiki/Terminal_emulator)** (terminal, terminal application)
is a computer program that provides an interface into which users can type commands and that can print text.
The terminal is also the computer program which runs the shell.

> Side note: It is called a "terminal emulator" because it is a software representation of the physical terminals for
> the old computers way back when computers were these huge multi-user things that took up the space of a large room.
> Users wouldn't interact directly with computers, because doing so was a pain when they were so large, expensive
> and securely stored. Instead, they would log on to a terminal and work with the computer that way.

Some examples of different terminal emulators are:
- Windows terminal / command prompt / cmd.exe
- Terminal (MacOS and Linux distributables such as Ubuntu)

<br>

A **[shell environment](https://en.wikipedia.org/wiki/Shell_(computing))** (shell, interpreter, command-line shell)
is basically just a computer program that runs other programs (commands). More specifically, a shell exposes the
operating system's features/services to a human user or other programs. This means that instead of having to interact
with a complicated graphical user interface or some other high-level program where we would select what we
wanted to do from a list, we can write commands directly to the operating system instead.
This is much more efficient and simple, though as you
know, it also requires some training. <br>

It is important to know that each shell environment comes with its own
[interpreter](https://en.wikipedia.org/wiki/Interpreter_(computing)) which interprets the code which is given to
the shell as input. This is why there are syntactic differences between different shells
(and generally, between different programming languages). Sometimes we use the terms
*interpreter* and *shell* interchangeably, because the real difference
between different shell is largely a result of the difference in interpreters.

Some examples of different shell environments/interpreters are:
- Bash
- Zsh
- Dash
- Windows command processor shell (The Windows command shell)
- Windows Powershell
- Fish
- IPython
- and a gazillion others....

<br>

A **[command line interface](https://en.wikipedia.org/wiki/Command-line_interface)** (CLI) is what you write you code
into. The CLI is implemented into the shell, and it processes the text input (code) that you give so that the shell
environment can understand it.

<br>

Keep in mind that most of the explanations here are
oversimplifications, mainly because you shouldn't need a full introduction to Computer Science just to write some
shell scripts for LING123.
To make things even clearer, I've created the visualization below of how these concepts relate to each other.

![The OS vs Shell vs Terminal](/assets/img/setting-shell-envs/OS%20vs%20Terminal%20vs%20Shell%20vs%20CLI.svg){: width="500"}
_The OS vs the shell vs the terminal, oversimplified_

<br>

### Differences between *bash* and *zsh*

Since there are differences in syntax for different shell environments, a script written for one
shell might not work in another. The default shell in most Linux operating systems (such as Ubuntu) is the *Bourne
Again Shell*, also known as *Bash*. The default shell on MacOS is the *Z Shell*, also known as *zsh*. The default shell
for Windows is the Command Processor Shell, also known as *cmd*, *cmd.exe* or the *Command Prompt*.

In the general sense, *zsh* is an extension of *bash*. This means that *zsh* will accept (almost) all *bash* syntax,
but *bash* will often not be able to accept *zsh* syntax. Therefore, we often experience that a *zsh* script won't work
when we run it in *bash*.

The main differences between *bash* and *zsh* are:
- Slight differences in syntax. For example, *zsh* supports floating-point (decimal) arithmetic operations and
  scientific notation.
- *Zsh* has many additional "quality-of-life" features such as spelling correction, `cd` autocomplete and much more.
- *Zsh* is much more customizable than *bash*, with many options for configuration, terminal themes and
  plugin support.
-  *Bash* has a lot more users than *zsh*, since it is the default installation on many systems.

Read more about the differences between *zsh* and *bash*
**[here](https://apple.stackexchange.com/questions/361870/what-are-the-practical-differences-between-bash-and-zsh)**. <br>
You can also read more about the differences from a more
[historical perspective](https://www.howtogeek.com/68563/htg-explains-what-are-the-differences-between-linux-shells/).

<br>

### How to quickly switch between *bash* and *zsh* in the terminal <br>
Sometimes it can be really useful to quickly switch between different shells on the command line (CLI).

To check which shell you are using in the terminal, enter the command `echo $SHELL`.
The output will probably be something like `/bin/bash` or `usr/bin/bash` for *bash* or `usr/bin/zsh` for *zsh*.

If you want to swap between shells, or set a different default shell, you first need to check to see if the other shell
you want to use is installed. Write `zsh --version` to check if *zsh* is installed and `bash --version` for *bash*.

To install *zsh* on Ubuntu, use `sudo apt-get install zsh`.
zsh is already installed on MacOS. To install *bash* on MacOS, use `brew install bash` (make sure you have the [Homebrew](https://brew.sh/)
package manager installed). *Bash* is already installed on Ubuntu.


If you want to execute a single script with a different shell, just use `shellname scriptname script_argument_1`.
Here's an example. Notice that I first tried to execute the script in my current shell (*bash*) with the
`source` command, but that didn't work (because the script uses a floating-point operation).

![Swapping to the zsh to execute a single script](/assets/img/setting-shell-envs/swapping-to-zsh.png){: .normal}

<br>


### Specifying a script's interpreter/shell environment <br>
Here's a *zsh* script. When I try to run this from the command line, it won't work, because I've set *bash*
as my default shell.

![a zsh script with no interpreter specified](/assets/img/setting-shell-envs/charwordratio.sh%20original.png){: .normal}
_A zsh script that won't work if you have bash as your default shell_

However, when we write a shell script we can also choose to specify what should be used to run (interpret) it.
For example, if I write a *zsh* script, I also want it to work for users who don't have *zsh* set as their default
shell environment. I can do that by declaring that the script should only be run in *zsh* through using a
"hashbang" ("shebang") `#!` at the top of the file, like so:

![Specifying zsh as interpreter for a script](/assets/img/setting-shell-envs/charwordratio.sh%20zsh%20specified.png){: .normal}
_Specifying zsh as the interpreter for a script_

When we use the `source scriptname.sh` command, we're saying that we want to run a script in the current shell environment.
The same goes for `. scriptname.sh` (only works for some shells, not all).

However, when we use `./scriptname.sh` (`./` is the "relative path" to the file), we can use the shell which is
specified in the hasbang part of the script. Here's an example with the script I just modified to use `zsh` as its
interpreter:

![Executing a zsh script with relative path](/assets/img/setting-shell-envs/executing-script-relative-path.png){: .normal}
_Executing a zsh-script using the relative path_

It is important to note that the user running the *zsh* script still needs to have *zsh* installed for this to work.
You might also have to set the permissions for the file with `chmod u+x filename` in order to execute it with
a relative path.
<br>

Here's a final example with `awk`. When writing a pure AWK script, we can also use `#!/usr/bin/env awk -f` at the
beginning of the file to specify that we want to run the script using the `awk` command.

![A very simple AWK script](/assets/img/setting-shell-envs/a-simple-AWK-script.png){: .normal}
_A very simple AWK script_


![Executing an AWK script](/assets/img/setting-shell-envs/executing-a-simple-AWK-script.png){: .normal}
_Executing a simple AWK script from the command line_
<br>



### Changing the default shell to *zsh* or *bash*
Some of you might be thinking that you want to change which shell to use as default.
If you want to change the shell from *bash* to *zsh*, you first need to run *zsh* for the first time and set the
different configurations. Just enter `zsh`, then navigate the configuration menu by typing one of the number keys,
`1`, `2` etc. I recommend that you just type `2` straight away and use the default settings. You can always
come back and change things later.

When you've completed the first time setup, run the command `chsh -s $(which zsh)`.
Type in your password, then restart the terminal.

Similarly, if you want to switch from *zsh* to *bash*, run the command `chsh -s $(which zsh)`.
Type in your password and restart the terminal.
> If `chsh -s $(which zsh)` doesn't work, try: `chsh -s /bin/bash` instead.

