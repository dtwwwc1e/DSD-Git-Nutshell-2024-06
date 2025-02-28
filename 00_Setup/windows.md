# Windows

On this training course We are going to use
[Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)). However, you can after
the course to switch to [PowerShell](https://en.wikipedia.org/wiki/PowerShell)
or the Windows command prompt. During this workshop we won't have time to help
you with these.

## Installing git and bash

You can either follow this [Video
Tutorial](https://www.youtube.com/watch?v=339AEqk9c-8) or follow the
instructions below:

1. Download the [Git for Windows installer](https://git-for-windows.github.io/).
1. Run the installer and follow the steps below:
   - Click on "Next" four times (two times if you've previously installed Git).
     You don't need to change anything in the Information, location, components,
     and start menu screens.
   - Select “Use the nano editor by default” and click on “Next”.
   - Keep "Git from the command line and also from 3rd-party software" selected and click on
     "Next". If you forgot to do this programs that you need for the workshop
     will not work properly. If this happens rerun the installer and select the
     appropriate option.
   - Click on "Next".
   - Keep "Checkout Windows-style, commit Unix-style line endings" selected and click on "Next".
   - Select "Use Windows' default console window" and click on "Next".
   - Click on "Install".
   - Click on "Finish".
1. If your `HOME` environment variable is not set (or you don't know what this is):
   - Open command prompt (Open Start Menu then type `cmd` and press `[Enter]`)
   - Type the following line into the command prompt window exactly as shown:
   ```
   setx HOME "%USERPROFILE%"
   ```
   - Press `[Enter]`, you should see `SUCCESS: Specified value was saved`.
   - Quit command prompt by typing `exit` then pressing `[Enter]`

This will provide you with both Git and Bash in the Git Bash program.

## Optional: Text editor - VSCode

The installation of Git bash will provide you with
[nano](https://www.nano-editor.org/). Some of you are familiar with the vi / vim editor and the VS Code application. Where there is time the trainer can assist participants with setting up and use, but Git bash is used for the training.

**Optional**
1. Download [VS Code](https://code.visualstudio.com/docs/?dv=win64user)
1. Run the installer and follow the steps below:
   - Click on "Next" five times. You don't need to change anything in the
     Information, location, components, and start menu screens. Only make sure you
     accept the license.
   - Click on "Install".
   - Click on "Finish".
1. Git should be integrated without any further settings
1. To set up the bash we need to do the following:
   - Open VS Code.
   - `[Ctrl]` + `[Shift]` + `[']` will open the terminal (which will be a
     powershell).
   - `[Ctrl]` + `[Shift]` + `[p]` opens a menu where you should type: `select
     default shell` and press `[Enter]`
   - Choose 'Git Bash'
   - Now when you open a new terminal (either by the shortcuts keys or via the
     `[+]` key) a git bash terminal should open.
1. To open VS Code from the git bash you can call it as:
   ```bash
   code <filename>
   ```
## GitHub.com and setting up your ssh keys

“Git” and “Github.com” are not the same thing. You will need your own account on github.com, In addition, you will also need to set up SSH key pair to fully integrate between Git and github.com. Setting up the SSH key pair can throw up problems, but following instructions as closely as possible is high chance this is done in a short time: [https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup](https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup), section 3.
