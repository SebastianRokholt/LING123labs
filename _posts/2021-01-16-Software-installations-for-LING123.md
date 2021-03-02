---
title: Software installations for LING123
date: 2021-01-16 15:00:00 +0100
categories: [Guides]
tags: [wsl, bash, shell, linux, setup, ubuntu, MSYS2, python, atom, emacs, spyder, pycharm, visualstudio, jflap, rstudio, r ]
pin: false
---

<style>
  ul {
    list-style: none;
  }
</style>

**LING123: Language and Computers** contains a significant amount of shell-scripting in Linux. If you are using MacOS (or a Linux OS for that matter), you already have access to a compatible shell. Just open the terminal window and you're ready!

Those of you who are on Windows, however, will need to install a compatible Linux terminal environment (a.k.a "shell"). This can be a bit tricky for some, so I've provided you all with a short guide. If you run into any problems you can't solve through Googling, I will be available to help you in the first few lab sessions.

You will also be required to install [Python 3](https://www.python.org/downloads/), [R](https://www.r-project.org/) + [RStudio](https://rstudio.com/products/rstudio/download/) and [jflap](http://www.jflap.org/jflaptmp/) through the extent of this course.

You may also feel free to install and get familiar with a text editor or IDE, such as [Atom](https://atom.io/), [Emacs](https://www.gnu.org/software/emacs/download.html), [Spyder](https://www.spyder-ide.org/), [Visual Studio Code](https://code.visualstudio.com/Download) or [PyCharm](https://www.jetbrains.com/pycharm/download/#section=windows). They aren't strictly necessary to complete this course, but they'll make your life a little (or a lot!) easier when writing longer scripts later in the semester.


# Installing a compatible Linux terminal environment (“shell”) on a Windows computer

## Setting up the shell
Windows does not provide a (ba)sh-compatible shell, and one must install one.

### For those using Windows 10:
<ul>
<li> For Windows 10, the easiest solution is to enable Windows Subsystem for Linux (WSL) and install a Linux distribution (e.g. Ubuntu 20.04 LTS) from the Microsoft Store. This provides you with both a terminal, the needed commands, and a full (terminal) Linux environment within Windows.</li>

<li> <b>Some very important things to note during installation are: </b> </li>
    <li> You must <b>remember the password</b> you gave during installation. You will need this to update the system or to install packages.
PS: When you are prompted by the terminal to enter your password, it doesn't show you what you are typing.
So try not to make any typos... :) </li>
    <li> The home directory in the Linux terminal and your Windows system <b>are not the same</b>, as the Linux filesystem is separated from the Windows one. </li>
</ul>

We will go through the commands for navigating the Linux filesystem in the first few labs.

To install WSL, please follow Microsoft’s own installation guide [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10). As a part of the installation, you will be asked to install a Linux distribution of your choice (step 6). I recommend Ubuntu 20.04 LTS (Long Term Support), but you can also choose something else if you already have a preference.


## For those using an older version of Windows:
WSL is only available for Windows 10, and as such, those using older versions must install other programs to be able to follow the course. There are multiple things to choose from (installing all packages manually, Cygwin, MSYS2), but I recommend using MSYS2. It is available for download [here](https://www.msys2.org/).

**MSYS2 Post-install:**
After the installation, open the “MSYS2 MSYS” application. <br>
         Run this command to update MSYS: `pacman -Syu` <br>
         Ensure that the following commands are available: `awk`, `grep`, `wc` <br>
         If they aren't, you can install them: `pacman -S coreutils gawk grep`
