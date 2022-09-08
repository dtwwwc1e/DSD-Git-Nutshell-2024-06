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
## Set up your ssh keys

To use git on our [gitlab instance](https://git.automation.ucl.ac.uk/), you will
need to set up ssh keys.

1. Open the terminal
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
   pbcopy < ~/.ssh/id_rsa.pub
   ```
   - Back on gitlab, click `[⌘]` + `[v]` to paste the key in the key form.
   - Change the Title by something meaningful if you plan to add more keys. (You
     won't be able to edit it afterwards, but you can deleted and add it again.)
   - Test that the key was added properly:
   ```bash
   ssh -T git@git.automation.ucl.ac.uk
   ```
   confirm writing `yes` that you want to continue. You should get a messsage like:
   ```
   Welcome to GitLab, @<username>!
   ```
