# Mac Setup
 What I use to setup an Intel CPU, 2019, Mac with

# Table of Contents
- [Install](#install)
    - [Xcode](#xcode)


# Install

## Xcode
- Has code that it installs system wide that many things will need to use on a Mac
- From Apple Mac [App Store][x-code]
- Let fully install, might take several hours
- Open once and accept terms and conditions

## iTerm 2
- Better CLI
    - Some features: Split panes, global search, copy/paste, configurability, 24 bit and
        256 color mode, more
- Download: [https://iterm2.com/][iterm2-homepage]
- After installation, set to have infinite scrollback in settings
- Find a color theme if you'd like: [iTerm2 themes][iterm-themes-github]
- Silence the 'Last Login' line when opening the terminal by adding a file to home
    directory: `touch ~/.hushlogin`
    - See [this Stackoverflow][hush-login-stackoverflow]

## Mac: See Hidden Files
- `$ defaults write com.apple.finder AppleShowAllFiles -bool TRUE;killall Finder`

## VS Code
- IDE
- Download [https://code.visualstudio.com/][vs-code-homepage]
- Configuration:
    - Set `terminal.integrated.scrollback`,
        `terminal.integrated.persistentSessionScrollback`, and
        `terminal.integrated.shellIntegration.history` values to 9999999999999 to
        have basically unlimited history

## Github Desktop
- Git GUI
- Download: [https://desktop.github.com/][github-desktop-homepage]

## Docker Desktop
- Docker GUI
- Download: [https://www.docker.com/products/docker-desktop/][docker-desktop-homepage]

## Homebrew
- Package Manager
- [Install instructions][brew-homepage]
- July 2022 installation was
    `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

## Install Bash with Homebrew
- See this article: [Upgrading Bash on macOS][bash-on-macOS]
- TL;DR of article:
    - Mac has an old version of bash installed from 2007 (As of Aug. 2021)
    - You want to:
        1. Install the latest version of Bash
        1. “Whitelist” new Bash as a login shell
        1. Set new Bash as the default shell
    ```bash
    $ brew install bash
    $ sudo vim /etc/shells

    # Add to bottom of file: 
    /usr/local/bin/bash
    # This will whitelist the Bash installed by Homebrew

    # Set the Homebrew Bash as the default
    $ chsh -s /usr/local/bin/bash
    # Change bash for root user
    $ sudo chsh -s /usr/local/bin/bash
    ```
- Tell scripts to use whatever version of Bash they see first on the PATH. You can
    do this by putting this line at the top of scripts. This tells it to inspect the
    PATH on the local machine and use what ever Bash it finds first:
    ```sh
    #!/usr/bin/env bash
    echo $BASH_VERSION
    ```
- Both System and User Bash will exist in tandem
    - System default Bash: `/bin/bash`
    - User installed Bash using this method: `/usr/local/bin/bash`

## ZSH with Brew
- Install Zsh with Homebrew
- Check system is pointing to Brew installation of ZSH
    - This is going to be like the bash installation, in that, by using brew, it
        will install zsh in /usr/local/bin/zsh and the system version will be in
        /bin/zsh. If you echo PATH, the default path has /usr/local/bin listed
        *before* /bin and the system will run the first one it finds when searching
        locations listed in the PATH, so it will run the brew one first. All this
        meaning, your system should be automatically pointing to the brew version.
        You can double check though, if you want, by checking with `which -a zsh`
        and it should show two versions, then check the PATH and see which is listed
        fist on the path.
    - System ZSH: `/bin/zsh`
    - User installed ZSH using this method: `/usr/local/bin/zsh`
- Do the same thing we did above, for bash, now for Zsh:
    ```bash
    $ brew install zsh
    $ sudo vim /etc/shells

    # Add to bottom of file: 
    /usr/local/bin/zsh
    # This will whitelist the Zsh installed by Homebrew

    # Change default shell for current user
    $ chsh -s /usr/local/bin/zsh
    # Change default shell for root user
    $ sudo chsh -s /usr/local/bin/zsh
    ```
- Set IDE to use the Homebrew ZSH for its integrated terminal, if it has one
    - VS Code: Code -> Preferences -> Settings, open settings, search for
        `terminal.integrated.defaultProfile`, set the value to be the Homebrew ZSH
        for your operating system

## Font
- Many programmer friendly, open source, fonts exist
    - Look at some examples: [NerdFonts Example Page][nerd-fonts-download]
    - Great one with ligatures: [FiraCode][fira-code-github]
1. Find a preferred font
1. With the preferred font, we can patch it to add programmer glyphs using
[Nerd Fonts][nerd-fonts-github]
    - Nerd Fonts has a script that can patch fonts to add the Nerd Fonts glyphs to a
        font
    - Many fonts are already patched ready for download, like [Fira Code pathed with
    Nerd Font][fira-code-nerd-font]
1. Check README on current installation instructions
1. Configure apps to use font (instructions below for Macs):
    - iTerm2: iTerm2 -> Preferences -> Profiles -> Text, set Font to the font and
        tick the checkbox for 'Use ligatures'
    - VS Code: Code -> Preferences -> Settings, open settings
        - Search `editor.fontLigatures`, set `true`
        - Change the base font for VS Code with `editor.fontFamily` which also
            applies to the integrated terminal, or just change the font family for
            the integrated terminal with `terminal.integrated.fontFamily`
            - The value for the default font says something like
                `Menlo, Monaco, 'Courier New', monospace`. Add the font to the front
                of that list, example: 
                `'FiraCode Nerd Font', Menlo, Monaco, 'Courier New', monospace`
            - Evaluates fonts in order and applies the first one it finds installed
        

## [Zinit][zinit-intro]
- Zsh plugin manager, loads fast
    - Allows installation of Oh My Zsh and Prezto plugins
- Install with Homebrew (preferred) or from [Github][zinit-github]
    - Homebrew preferred because it's easy to update with Homebrew
        - Make a .zshrc file at `~/.zshrc` if one doesn't exist yet
        - As of July 2022, the Homebrew installation of Zinit requires this to be
            added to a .zshrc file:

            `source $(brew --prefix)/opt/zinit/zinit.zsh`
            - This file sets up command completion for Zinit
- Every once in a while [update Zinit][zinit-update] and it's plugins
    - `brew update zinit` or `zinit self-update`
    - `zinit update` for all plugins
    - `zinit update <plugin>` for a specific plugin

## ~/.vimrc File
- See [vimrc file][vimrc-file]

## ~/.zshrc File
- On Mac, zsh will load the /etc/zshrc file first and then the ~/.zshrc file.
    Anything in the ~/.zshrc file overrides the /etc/zshrc file.
- See the [zshrc][zshrc-file] file in this repo

## LSD
- `$ ls` command replacement. Has glyphs from [Nerd Fonts][nerd-fonts-github].
- [Github Link][lsd-github]
- `brew install lsd`

## Fun
- Command Line
    - [Pipes Screensaver][pipes-github]
    - [CMatrix Screensaver][cmatrix-github]

## DevUtils
- Useful tool for a variety of small developer situations
- [Website][devutils-website]
- Homebrew or download
- For Windows: https://devtoys.app/

## HTTPie
- Nicer curl for API's and URL's
- [Website][httpie-website]
- Homebrew, have a Desktop UI incoming, but not available as of mid 2022

## pyenv
- Python version management
- [Github][pyenv-github]
- Homebrew
- Add to .zshrc:
    ```bash
    # pyenv
    export PYENV_ROOT="$HOME/.pyenv"
    export PATH="/Users/demon_slayer/.pyenv/shims:${PATH}"
    ```
- Install some python versions and set one globally
    - `$ pyenv install 3.x.x`
    - `$ pyenv global 3.x.x`

## Poetry
- Python package, dependency, environment management
- [Website][poetry-website]
- [Installation][poetry-install]
    - Add Poetry install location to your PATH in .zshrc file:
        - `export PATH="$HOME/.local/bin:${PATH}"`

## Commands
- Clear python pycache:
`$ find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf`

# Other, Not Organized
## Setup AWS credentials and config

## Change Apple Emoji for JoyPixels
- https://apple.stackexchange.com/a/409205

## Packages for Python Projects:
black, isort, pytest, pytest-cov, pre-commit
- Black is an active linter, flake8 is a passive one


## AWS cli command that logs you into all the aws profiles you have:
https://github.com/aidanmelen/awscli_bastion

- make a file at ~/.aws/cli/alias
- put this in your alias file:
=========================
[toplevel]
    whoami = sts get-caller-identity
    bastion =
        !f() {
            if [ $# -eq 0 ]
            then
                bastion get-session-token --write-to-aws-shared-credentials-file
            else
                bastion get-session-token --write-to-aws-shared-credentials-file --mfa-code $1
            fi
            bastion assume-role de-central
            bastion assume-role de-dev
            bastion assume-role de-stage
            bastion assume-role de-prod
            bastion assume-role de-align
            echo "Successfully assumed roles in all AWS accounts!"
        }; f
=========================
- make a directory: `mkdir -p ~/.aws/cli/cache`
- Make your ~/.aws/credentials look like this (backup your old one):
===========================
- then: `$ pip install awscli-bastion`
- It adds a sub command to the aws cli, you use it like this:
`$ aws bastion <mfa token>`
- It will create a token for all the accounts in the credentials file, it will last for the length of time the token is good for (1 hour at the moment), you can set a crontab to run that will refresh this every hour so you don't have to think about it
Crontab: (crontab -l ; echo "0 9-20 * * 1-5 aws bastion") | sort - | uniq - | crontab -

## Install Terraform
- If it's useful to switch terraform versions sometimes, if that's needed, tfswitch is one option: https://tfswitch.warrensbox.com/Install/

## Setup a Github ssh key
https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent
- use this in the ~/.ssh/config file:
 ############ Git Hub #################
 Host github.com
     HostName github.com
     AddKeysToAgent yes
     IdentityFile /Users/<user>/.ssh/git_hub_ps

## NVM, NPM
 install nvm, add a couple lines to zshrc to get it to work
- Use nvm to install npm

## Java
`$ brew install java # will install the jdk`
- - symlink java so system can see it:
```bash
sudo ln -sfn /usr/local/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk
```

[x-code]: https://apps.apple.com/us/app/xcode/id497799835?mt=12
[iterm2-homepage]: https://iterm2.com/
[vs-code-homepage]: https://code.visualstudio.com/
[github-desktop-homepage]: https://desktop.github.com/
[docker-desktop-homepage]: https://www.docker.com/products/docker-desktop/
[brew-homepage]: https://brew.sh/
[zinit-github]: https://github.com/zdharma-continuum/zinit
[bash-on-macos]: https://itnext.io/upgrading-bash-on-macos-7138bd1066ba
[zinit-intro]: https://zdharma-continuum.github.io/zinit/wiki/INTRODUCTION/
[zinit-update]: https://github.com/zdharma-continuum/zinit#updating-zinit-and-plugins
[nerd-fonts-download]: https://www.nerdfonts.com/font-downloads
[fira-code-github]: https://github.com/tonsky/FiraCode
[nerd-fonts-github]: https://github.com/ryanoasis/nerd-fonts
[fira-code-nerd-font]: https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/FiraCode
[vimrc-file]: https://github.com/canada11/mac-setup/blob/main/vimrc
[zshrc-file]: https://github.com/canada11/mac-setup/blob/main/zshrc
[lsd-github]: https://github.com/Peltoche/lsd
[iterm2-themes-github]: https://github.com/mbadolato/iTerm2-Color-Schemes/blob/master/README.md
[pipes-github]: https://github.com/pipeseroni/pipes.sh
[cmatrix-github]: https://github.com/abishekvashok/cmatrix/
[hush-login-stackoverflow]: https://stackoverflow.com/questions/15769615/remove-last-login-message-for-new-tabs-in-terminal
[devutils-website]: https://devutils.com/
[httpie-website]: https://httpie.io/
[pyenv-github]: https://github.com/pyenv/pyenv
[poetry-website]: https://python-poetry.org/
[poetry-install]: https://python-poetry.org/docs/master/#installing-with-the-official-installer