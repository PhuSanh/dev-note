# Install Oh My Zsh for iterm

- `curl` or `wget` should be installed
- `git` should be installed

Install zsh
```
brew install zsh zsh-completions
```
Set zsh as a default shell
```
chsh -s /bin/zsh
```

### Install Oh My Zsh
#### via curl
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
#### via wget
```
sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Config
open and edit `~/.zshrc`\
Add this line at the beginning to detect plugins
```
source /Users/sanhdp/.bash_profile (macOS)
source /home/YOUUSERNAME/.bash_profile (linux)
```
Edit theme you want
```
ZSH_THEME="agnoster"
```

### Update font
Go to `https://github.com/powerline/fonts/tree/master/SourceCodePro`\
Download some fonts and install





