# Linux

## Installing git

Your machine probably includes git installed, but if not then you would have to 
run the appropriated package manager for your distribution.

For example, if Debian based:
```bash
sudo apt-get install git
```
or RedHat based:
```bash
sudo dnf install git
```

## Optional: Text editor - VSCode

The installation of Git bash will provide you with
[nano](https://www.nano-editor.org/). Some of you are familiar with the vi / vim editor and the VS Code application. Where there is time the trainer can assist participants with setting up and use, but Git bash is used for the training.

**Optional**
1. Download [VS Code](https://code.visualstudio.com/) choosing your distro (deb, rpm)
1. Open a terminal
   - browse to where you've downloaded the file
   ```bash
   cd ~/Downloads/
   ```
   - run the installation:
   ```bash
   sudo apt install <file>.deb
   ```
   - If you get into troubles or you are using a non debian based distribution,
     check for more detail on their [installation
     instructions](https://code.visualstudio.com/docs/setup/linux).
1. Git should be integrated without any further settings
1. You should be able to open it from the terminal by:
   ```bash
   code <filename>
   ```
   
## GitHub.com and setting up your ssh keys

“Git” and “Github.com” are not the same thing. You will need your own account on github.com, In addition, you will also need to set up SSH key pair to fully integrate between Git and github.com. Setting up the SSH key pair can throw up problems, but following instructions as closely as possible is high chance this is done in a short time: [https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup](https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup), section 3.

