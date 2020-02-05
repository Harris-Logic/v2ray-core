# ohmyzsh摸鱼



cat /etc/shells

切换bash

chsh -s /bin/bash

切换zsh

chsh -s /bin/zsh

//via curl

sh -c "$\(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh\)"

//via wget

sh -c "$\(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -\)"

ZSH\_THEME="agnoster"

cd ~/.oh-my-zsh/custom/plugins/

git clone https://github.com/zsh-users/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git

vim ~/.zshrc

plugins=\(git zsh-syntax-highlighting zsh-autosuggestions\)

cd ~/.oh-my-zsh/custom/themes

git clone https://github.com/bhilburn/powerlevel9k.git

vim ~/.zshrc

ZSH\_THEME="powerlevel9k/powerlevel9k"

source ~/.zshrc  


