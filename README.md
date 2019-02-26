
# MacBook Setup

## Introduction

First thing you need to do, on any OS actually, is update the system! For that: **Apple menu () > About This Mac > Software Update.**

## System Preferences

Run the defaults script to configure most defaults for the OS and native applications.

```bash
$ ./set-defaults.sh
```

### Security & Privacy

- Enable FileVault
- Enable Firewall
	- Uncheck everything except for **Enable Stealth Mode**

### Users & Groups
- Set up Password, Apple ID, Picture, etc.

### Internet Accounts

- Add iCloud account for `iCloud Drive`, `Photos`, `Reminders`, `Safari`, `Notes`, `Keychain`, `Find My Mac`
- Add Google account for `Contacts`, `Mail` and `Calendar`
- Add Facebook account for `Notifications` and `Share Menu`

### Finder

- Toolbar
	- Add "New Folder" icon
- Sidebar
	- Add `Home` and `Projects` directories
	- Remove tags

### Time Machine

- Select `Orion` server as backup destination
- Backup automatically

### Screensaver

Install the [Aerial](https://github.com/JohnCoates/Aerial/releases/download/v1.2/Aerial.zip) screensaver for Mac here.
Enable it in System Preferences.

## Xcode

Install [Xcode](https://developer.apple.com/xcode/) from the App store or the Apple developer website.

For installing Xcode command line tools run the command

```bash
$ xcode-select --install
```

It'll prompt you to install the command line tools. Follow the instructions and now you have Xcode and Xcode command line tools both installed and running.

## Homebrew

In the terminal paste the following line (without the $), hit Enter, and follow the steps on the screen:

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install brew casks:

```bash
$ brew tap caskroom/cask
```

One thing we need to do is tell the system to use programs installed by Hombrew (in `/usr/local/bin`) rather than the OS default if it exists. We do this by adding `/usr/local/bin` to your `$PATH` environment variable:

```bash
$ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
```

Open an new terminal tab with Cmd+T (you should also close the old one), then run the following command to make sure everything works:

```bash
$ brew doctor
```

## Quicklook Plugins

Some [plugins](https://github.com/sindresorhus/quick-look-plugins) to enable different files to work with Mac Quicklook. Includes features like syntax highlighting, markdown rendering, preview of jsons, patch files, csv, zip files etc.

```bash
QUICK_LOOK_PLUGINS=(
	qlcolorcode
	qlstephen
	qlmarkdown
	quicklook-json
	quicklook-csv
	betterzipql
	webpquicklook
)

echo "Installing Quick Look plugins..."
brew cask install ${QUICK_LOOK_PLUGINS[@]}
```

## Applications

Most applications can be installed with brew:

```bash
CASKS=(
	arq
	atom
	docker-toolbox
	dropbox
	flux
	google-chrome
	google-drive
	google-hangouts
	grammarly
	iterm2
	keyfinder
	macdown
	postico
	pycharm
	remote-play
	skype
	slack
	sublime-text
	transmission
	virtualbox
	vlc
	whatsapp
)
echo "Installing cask apps..."
brew cask install ${CASKS[@]}
```

## Tools

### iTerm2

#### Colors and Font Settings

- Download the Solarized Dark iTerm2 color scheme from [here](https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Solarized%20Dark.itermcolors). And then set these to your default profile colors.
- Download the patched Menlo Powerline font from [here](https://github.com/powerline/fonts/raw/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf). And set to 14pt for regular text, 15pt for non-ASCII text.

#### Enable word jumps

By default, word jumps (option + → or ←) do not work. To enable these, go to "iTerm -> Preferences -> Profiles -> Keys". Press the + sign under the list of key mappings and add the following sequences:

**Option + right**

```
⌥→
Send Escape Sequence
f
```

**Option + left**

```
⌥←
Send Escape Sequence
b
```

### ZSH

Install zsh, zsh completions and zsh-syntax-highlighting using homebrew:

```bash
$ brew install zsh zsh-completions zsh-syntax-highlighting
$ chsh -s /usr/local/bin/zsh
```

Install oh-my-zsh on top of zsh to get additional functionality

```bash
$ curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```

Set up `~/.zshrc`:

```bash
ZSH_THEME="agnoster"
plugins=(brew colored-man-pages docker git git-flow-avh pip pyenv extract urltools)
DEFAULT_USER=`whoami`

# Add env.sh
source ~/env.sh

source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

Create `~/env.sh`:

```bash
#!/bin/zsh

# PATH
export PATH="/usr/local/bin:$$PATH"  # FIX: the double "$$"
export EDITOR='subl -w'

# mkdir and cd
function mkcd() { mkdir -p "$@" && cd "$_"; }

# Aliases
alias ll="ls -lh"
	
	
# Use sublimetext for editing config files
alias zshconfig="subl ~/.zshrc"
alias envconfig="subl ~/env.sh"
```

### Git

To install, simply run:

```bash
$ brew install git
```

Next, define the git user:

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "your.email@gmail.com"
$ git config --global credential.helper osxkeychain
```

### SSH

First, we need to check for existing SSH keys on your computer. Open up your Terminal and type:

```bash
$ cd ~/.ssh
$ ls -al
```

Use a strong passphrase for keys.

```bash
$ ssh-keygen -t rsa -b 4096 -C "Your Name (MacBook) <your.email@gmail.com>"
```

Add the public key to GitHub and BitBucket.

### Python

Install `pyenv` using homebrew:

```bash
$ brew install pyenv
```

After installing, add pyenv init to your shell to enable shims and autocompletion.

```bash
$ echo 'eval "$(pyenv init -)"' >> ~/env.sh
```

Open a new terminal and enter these commands:

```bash
$ pyenv install 3.6.1
$ pyenv global 3.6.1
```

Open another new terminal, install Jupyter and iPython:

```bash
pip3 install --upgrade pip
pip3 install jupyter ipython
```

To install `pipenv`, enter this command:

```bash
$ pip install pipenv
```

### Node.js

To install or update `nvm`, you can use the install script using cURL:

```bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

Open a new terminal window and enter this command:

```bash
$ nvm install --lts
```

To upgrade the copy of npm that is provided with Node.js, run this command in a new terminal:

```bash
$ npm -g upgrade npm
```

### AWS CLI

Install the CLI:

```bash
$ brew install awscli
```

Configure profiles:

```bash
$ aws configure --profile personal
AWS Access Key ID [None]: **REDACTED**
AWS Secret Access Key [None]: **REDACTED**
Default region name [None]: ap-southeast-2
Default output format [None]: text

$ aws configure --profile porter
AWS Access Key ID [None]: **REDACTED**
AWS Secret Access Key [None]: **REDACTED**
Default region name [None]: ap-southeast-2
Default output format [None]: text
```

## Folder Structure

Create some basic folder structure:

```bash
echo "Creating Projects folder..."
cd ~
[[ ! -d Projects ]] && mkdir Projects

echo "Creating Downloads folder structure..."
cd ~/Downloads
[[ ! -d Applications ]] && mkdir Applications
[[ ! -d Misc ]] && mkdir Misc
[[ ! -d Movies ]] && mkdir Movies
[[ ! -d Music ]] && mkdir Music
[[ ! -d "TV Shows" ]] && mkdir "TV Shows"

echo "Creating Pictures folder structure..."
cd ~/Pictures
[[ ! -d Wallpapers ]] && mkdir Wallpapers

cd ~
echo "Done!"
```

## Format and Reinstall MacOS on other device

<https://support.apple.com/en-au/HT201065>

## References

1. <http://sourabhbajaj.com/mac-setup/index.html>
2. <http://www.stuartellis.name/articles/mac-setup/>
3. <https://github.com/michaelzoidl/setup-new-macbook/>
4. <https://github.com/danguita/osx-for-developers/>
