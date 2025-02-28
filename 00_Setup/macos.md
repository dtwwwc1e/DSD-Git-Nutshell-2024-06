# MacOS

## Installing git

The easier way to install Git is to use Xcode Command Line Tools. If you are using
Maverics (10.9) or above this is all you need to do:

1. Open a terminal on your Mac
1. Type the following command:
   ```bash
   git --version
   ```
   If you are given a version then that's it, if not you should get a pop-up window
   asking you to install XCode.
   - Click on 'Install' and that's it.
## Text editor - VSCode

[nano](https://www.nano-editor.org/) and [vim](https://www.vim.org/) should be already
available on your Mac. However,
we will also use [VS Code](https://code.visualstudio.com/). Follow these instructions
to get VS Code on your mac.

1. Download [VS Code](https://code.visualstudio.com/docs/?dv=osx)
1. Double-click on the downloaded archive to open
   - Drag VSCode.app to the Applications folder.
1. Git should be integrated without any further settings
1. You should be able to open it from the terminal by:
   ```bash
   code <filename>
   ```
## GitHub.com and setting up your ssh keys

“Git” and “Github.com” are not the same thing. You will need your own account on github.com, In addition, you will also need to set up SSH key pair to fully integrate between Git and github.com. Setting up the SSH key pair can throw up problems, but following instructions as closely as possible is high chance this is done in a short time: [https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup](https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup), section 3.

