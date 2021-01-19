# Install a specific font from nerd-fonts from sources

```sh
# checkout the project
if [ -d ~/build/nerd-fonts ]; then
    mkdir -p ~/build && cd ~/build || exit 1
    git clone --depth 1 https://github.com/ryanoasis/nerd-fonts.git
    cd nerd-fonts || exit 1
fi

# install the font and refresh the font cache
./install.sh Hack
fc-cache -fv ~/.local/share/fonts
```
