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

## Text editor - VSCode

The installation of Git bash will provide you with
[nano](https://www.nano-editor.org/) and [vim](https://www.vim.org/). However,
we will also use [VS Code](https://code.visualstudio.com/). Follow these instructions
to get VS Code integrated with gitbash.

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
## Set up your ssh keys

To use git on our [gitlab instance](https://git.automation.ucl.ac.uk/), you will
need to set up ssh keys.

1. Open Git Bash
1. Run the following command:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@ucl.ac.uk"
   ```
   - Click `[Enter]` when asked for where to save the key, unless you know what
     you are doing.
   - Type a secure passphrase when asked for it. You won't see any character
     being typed, but they are! 
   - Type your passphrase again.
1. Add the ssh-keys to your agent
   - Type
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_rsa
   ```
1. Add the public key to gitlab
   - Login into our [gitlab instance](https://git.automation.ucl.ac.uk/)
   - Click on your avatar on the top right corner and select 'Settings'
   - Click on 'SSH Keys' on the left bar.
   - On the git bash execute the following:
     ```bash
     clip < ~/.ssh/id_rsa.pub
     ```
   - Back on gitlab, click `[Ctrl]` + `[v]` to paste the key in the key form.
   - Change the Title by something meaningful if you plan to add more keys. (You
     won't be able to edit it afterwards, but you can deleted and add it again.)
   - If you get an error message from GitLab saying that the form contain errors.
     Then it may have been that the `clip` command didn't work as expected. Open
     the file with a text editor then and copy the whole content of the public key
     with `[Ctrl]` + `[c]` and paste it into the key form.
     ```bash
     code ~/.ssh/id_rsa.pub
     ```
   - Test that the key was added properly:
   ```bash
   ssh -T git@git.automation.ucl.ac.uk
   ```
   confirm writing `yes` that you want to continue. You should get a messsage like:
   ```
   Welcome to GitLab, @<username>!
   ```
