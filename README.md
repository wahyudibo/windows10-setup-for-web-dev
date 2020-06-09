# My Journey on Configuring Windows 10 for Web Dev

This is my journal about configuring Windows 10 to become my primary machine for web development. Not all of these steps necessary, you can choose whatever step that suits your need.

SideNote: 

- i'm using Windows 10 Pro 64bit
- i'm using Ubuntu 20.04 as default WSL distro

### Contents
- How-Tos
  - [Install and Configure Colemak as My Default Keyboard Layout](#install-and-configure-colemak-as-my-default-keyboard-layout)
  - [Change Machine Name](#change-machine-name)
  - [Turn on Disk Encryption (Windows 10 Pro only)](#turn-on-disk-encryption-windows-10-pro-only)
  - [Enable Built in Clipboard](#enable-built-in-clipboard)
  - [Enable Night Light](#enable-night-light)
  - [Change to Small Taskbar](#change-to-small-taskbar)
  - [Enable Fingerprint](#enable-fingerprint)
  - [Enable WSL and Upgrade to WSL 2](#enable-wsl-and-upgrade-to-wsl-2)
  - [Install Chocolatey (Package Manager for Windows)](#install-chocolatey-package-manager-for-windows)
  - [Install and Configure Terminal for Windows from Microsoft Store](#install-and-configure-terminal-for-windows-from-microsoft-store)
  - [Install Ubuntu Tools and Utility in WSL](#install-ubuntu-tools-and-utility-in-wsl)
  - [Make WSL Terminal Pretty and Powerful](#make--wsl-terminal-pretty-and-powerful)
  - [Sharing SSH between Windows and WSL and Configure Correct Permission for SSH in WSL](#sharing-ssh-between-windows-and-wsl-and-configure-correct-permission-for-ssh-in-wsl)
  - [Fix Folder and Files Default Permission in Windows (From WSL Perspective)](#fix-folder-and-files-default-permission-in-windows-from-wsl-perspective)
  - [Install and Configure Git](#install-and-configure-git)
  - [Sharing Git Credentials Between Windows and WSL](#sharing-git-credentials-between-windows-and-wsl)
  - [Install and Configured Docker for Windows](#install-and-configured-docker-for-windows)
  - [Install and Configure VSCode + Remote Development Pack](#install-and-configure-vscode--remote-development-pack)
- Known Bugs
  - [WSL 2 Consumes Too Much CPU / Memory](#wsl-2-consumes-too-much-cpu--memory)

## How-Tos

### Install and Configure Colemak as My Default Keyboard Layout

I'm using colemak as my keyboard layout for more than 5 years now. Unfortunately, Windows not shipped with colemak out of the box so i need to install it myself (c'mon guys, colemak even included by default in linux).

- Colemak for Windows have 2 version, one with Capslock mapped to Backspace and one that doesn't. I'm using the former. You can download both version [here](https://colemak.com/Windows). For the one i use, download it from [here](https://forum.colemak.com/topic/1621-colemak-for-windows-with-capslock-to-backspace/).
- Go to Settings > Time & Language > Preferred Language and Choose the primary language. Mine is English (US). Then click Options button. Make sure that Colemak already registered there.
- To make Colemak as primary keyboard layout, Go to Settings and type `Advanced Keyboard Settings` and choose Colemak from the dropdown.
- To disable (annoying) Ctrl + Shift combo that change the keyboard layout accidentally, still in `Advanced Keyboard Settings`, go to `Input language hot keys` choose `Between input languages` then press `Change Key Sequence` button. Choose `Not Assigned` options for both of settings.

### Change Machine Name

- Go to Settings > System > About > Rename this PC

### Turn on Disk Encryption (Windows 10 Pro only)

Since i have many confidential files, i need to encrypt my harddisk. We can use Bitlocker which is come with Windows 10 Pro by default.

- Open explorer (Win + E)
- Right click on C:\ (i only have single partition for everything), then choose Turn on Bitlocker

### Enable Built in Clipboard

- Go to Settings > System > Clipboard > Clipboard History > On
- Access clipboard using Win + V

### Enable Night Light

- Go to Settings > System > Display > Night light > On

### Enable Dark Mode

- Go to Settings > Personalization > Colors > Choose your color > Dark

### Change to Small Taskbar

- Go to Settings > Personalization > Taskbar > Use small taskbar buttons > On

### Enable Fingerprint

If your machine support Windows Hello, you can setup fingerprint mode for login which very convenient IMO.

- Setup your account with password first
- Go to Settings > Account > Sign in options > Windows Hello Fingerprint

### Enable WSL and Upgrade to WSL 2 

- To enable WSL, go to Settings > Apps > Program and Features (on the top right side) > Turn off Windows features on or off > Check Hyper-V, Virtual Machine Platform, and Windows Subsystem for Linux

- Follow instruction in this [page](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to upgrade to WSL 2. Your Windows version must be in version 2004 to use WSL 2.

- Included in that page, there are steps for Ubuntu 20.04 installation from Microsoft Store after done installing WSL 2. We also will set WSL 2 as default WSL (since we have WSL 1 and WSL 2).

- Set default distro to Ubuntu 20.04 by using 

  ```
  wsl -s <DistributionName>, wsl --setdefault <DistributionName>
  ```

### Install Chocolatey (Package Manager for Windows)

- Follow this instruction from [Chocolatey install guide](https://chocolatey.org/install)

### Install and Configure Terminal for Windows from Microsoft Store

- Install `Terminal` from Microsoft Store

- Make Ubuntu 20.04 as default by setting the value of `defaultProfile` to Ubuntu guid. You can find it under list, still in settings.json file

- Change starting directory to linux home directory instead of /mnt/c/User by adding this properties under Ubuntu 20.04 profile

  ```json
  "startingDirectory": "\\\\wsl$\\Ubuntu-20.04\\home\\[linux-username]"
  ```

- We can also reorder the list and move Ubuntu 20.04 to the top so by default Terminal app will choose Ubuntu when we click `+` button. 

### Install Ubuntu Tools and Utility in WSL

```
sudo apt install curl git net-tools xsel acpi htop
```

### Make  WSL Terminal Pretty and Powerful

I already use several tools not only to make my terminal more powerful, but also pleasing to the eye.  The tools are:

- Homebrew. Homebrew, which is known as package manager in MacOS, also available in linux. Homebrew provide many ready to use scripts and tools so we don't need to built from source manually. To install, follow instruction [here](https://brew.sh/). To backup and restore, you can use `brew bundle dump` for backup and `brew bundle` to restore. More on those, you can read it [here](https://tomlankhorst.nl/brew-bundle-restore-backup/)

- ZSH + Oh-My-ZSH + antigen

- Tmux

- Powerline fonts. We can install the font in Windows automatically without having to install them one by one using font viewer.

  - Clone the font from https://github.com/powerline/powerline.git in directory that you have chosen.

  - Open Powershell, then run `Get-ExecutionPolicy`. If it returns `Restricted`, then run 

    ```
    Set-ExecutionPolicy Bypass -Scope Process
    ```

  - Change directory to where powerline cloned (get inside the folder called `fonts-master`),  and run `install.ps1`

- Change font in Terminal to powerline font by adding this line to Ubuntu profile in settings.json

  ```
  "fontFace":  "DejaVu Sans Mono for Powerline"
  ```

- To open wsl directory on windows explorer, simply open wsl and run `explorer.exe .`. If you come previously from ubuntu, you can copy your configuration file here (.zshrc, .tmux.conf, .gitconfig, ssh folder etc)

My WSL console appearance:

![My WSL Console](https://i.imgur.com/KP03Ww9.png)

### Sharing SSH between Windows and WSL and Configure Correct Permission for SSH in WSL

- Follow instruction [here](https://devblogs.microsoft.com/commandline/sharing-ssh-keys-between-windows-and-wsl-2/)

- Don't forget to set the permission (chmod) of the SSH folder and files so we can use it normally.

  .ssh directory: 700 (drwx------)  
  public key (.pub file) & known host: 644 (-rw-r--r--)  
  private key (id_rsa): 600 (-rw-------)  
  lastly your home directory should not be writeable by the group or others (at most 755 (drwxr-xr-x)).

### Fix Folder and Files Default Permission in Windows (From WSL Perspective)

Windows by default assign 777 if the file is located outside WSL. We can fix this by 

- adding this setting in `.profile` inside WSL home directory
  ```
  if [[ "$(umask)" = "0000" ]]; then
      umask 0022
  fi
  ```

- create file (if not exists) `/etc/wsl.conf` and put this inside the file content
  ```
  [automount]
  enabled=true
  options=metadata,uid=1000,gid=1000,umask=022
  ```

### Install and Configure Git

Git can be a pain for a cross platform project. This is due to difference in how line ending being treated in Windows and *nix OS. Windows using CRLF, *nix using LF

- Install git in WSL with `sudo apt install git` if you haven't done this before. This git limited to WSL console only. If you want to use git in your IDE, you must install Git for Windows

- If you see all your existing project suddenly become modified, don't panic. Resolve it by using this setting in your global git config

  ```
  git config --global core.autocrlf true
  git config --global core.filemode false
  ```

  Basically, `core.autocrlf true` will use this conversion rule `Checkout -> CRLF, Commit -> LF`. This is suitable for windows, since any project from remote repo's line ending will automatically converted to CRLF in checkout. On commit, it will be automatically converted to LF. Meanwhile `core.filemode false` will ignore the difference in permission between folder/file checked out in WSL or Windows storage.

  Still see the files modified in git status ? check your entire config by using `git config --list --show-origin` to make sure that the global config is not overridden by project-scoped config. 

  By the way, **if you have project that meant to be run in Linux, put them inside WSL folder**. If you put the project outside WSL, the access will be significantly slow and some watch feature (create react app hot reload, webpack watch) will not work. More on that, see the comment about WSL 2 limitation [here](https://github.com/microsoft/WSL/issues/4739#issuecomment-582374769)

### Sharing Git Credentials Between Windows and WSL

- Follow instructions [here](https://code.visualstudio.com/docs/remote/troubleshooting#_sharing-git-credentials-between-windows-and-wsl)

### Install and Configured Docker for Windows

- Download the exe [here](https://docs.docker.com/docker-for-windows/install/)
- If WSL 2 is installed, Docker will ask to use WSL 2 by default.
- After installation finished, go to docker icon in taskbar, then choose Settings > Resources > WSL Integration > turn on Ubuntu 20.04
- Check the integration by opening WSL console, then type `docker version`. It should return with installed docker information.

### Install and Configure VSCode + Remote Development Pack

- Install VSCode, then download Remote Development extension from Microsoft

- To set default terminal in VSCode to WSL, put this in VSCode settings.json

  ```
  "terminal.integrated.shell.windows": "C:\\Windows\\System32\\wsl.exe"
  ```

- To open your project using VSCode in WSL context, the easy way is just open WSL console, go to your project root, then type `code .` and VSCode will be open and automatically connected with WSL.

- See [here](https://code.visualstudio.com/docs/remote/wsl) for more complete instruction on setting VSCode and WSL  

## Known Bugs

### WSL 2 Consumes Too Much CPU / Memory

Open the Task Manager and see if `Vmmem` is causing your CPU / memory usage to 100%. Don't worry, that's not because your machine is lack of memory. In the following github issue page, someone reported that his machine (which having 64GB RAM) also encountered this problem too. Basically, it's a known bug when we use WSL 2 and try to do heavy operation. Apply the workaround from [here](https://github.com/microsoft/WSL/issues/4166#issuecomment-628493643) or you can follow my steps to implement those workaround in cron job which are :

- open WSL console

- create .drop_cache.sh file in home directory and give execute access with `chmod ug+x .drop_cache.sh`

- inside the file, paste this command

  ```bash
  #!/usr/bin/env /bin/bash
  
  sudo sh -c "echo 3 >'/proc/sys/vm/drop_caches' && swapoff -a && swapon -a && printf '\n%s\n' 'Ram-cache and Swap Cleared'"
  ```

- Create a cron job that will run the script for every minute.

  ```
  crontab -l | { cat; echo "* * * * * root /home/[linux-username]/.drop_cache.sh"; } | crontab -
  ```

- Check whether the cron is created or not by running `crontab -l` command

  ```
  $ crontab -l
  * * * * * root /home/[linux-username]/.drop_cache.sh
  ```

  