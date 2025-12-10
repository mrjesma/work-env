# Work environment
This is the current steplist to setup my work environment.

I may soon create an ansible playbook or a bash script to automate this proccess...


# WSL

#### Install WSL distro
- `wsl --install`
- `wsl --update`
- `wsl --version`
- `wsl --install -d Ubuntu`
- Enter username and password when requested

#### Edit WSL distro configuration
- `sudo vim /etc/wsl.conf`

```plaintext
[network]
generateHosts = true
generateResolvConf = false

[boot]
systemd=true
```

#### Restart WSL
```powershell 
exit # to exit WSL
wsl -d Ubuntu --shutdown
wsl -d Ubuntu
```

#### Configure DNS
```bash
sudo resolvectl dns eth0 1.1.1.1
sudo resolvectl domain eth0 br.proj.empros.com
```

#### Update packages
- `sudo apt update && sudo apt upgrade -y`


# Terminal Emulator
I used to use Windows Terminal, then I went to WezTerm and now I'm back to Windows Terminal again.

#### Install Windows Terminal
In windows 11, the windows terminal is the default terminal emulator and it is already installed. In other cases you can install it from [Microsoft Store](https://aka.ms/terminal), or manually from its [repo](https://github.com/microsoft/terminal/releases) installing with the following command: `Add-AppxPackage Microsoft.WindowsTerminal_<versionNumber>.msixbundle`

#### Configure
Copy the content of [windows-terminal.json](windows-terminal.json) to the windows terminal `config.json` file.

#### win32yank
Download win32yank and put it in your path (eg.: C:\Windows\System32) for a seamless clipboard between Windows and WSL. `https://github.com/equalsraf/win32yank`


# OH-MY-ZSH
#### Install dependencies
```bash
sudo apt update && \
sudo apt install -y git curl zsh
```

#### Install oh-my-zsh
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### Configure
```bash
# Install dependencies
sudo apt update && \
sudo apt install -y git stow autojump

git clone --recurse-submodules https://github.com/mrjesma/dotfiles.git ~/repos/dotfiles

cd ~/repos/dotfiles
rm ~/.zshrc
stow -t ~/ zsh
omz reload
```


# TMUX

#### Install dependencies
```bash
sudo apt update && \
sudo apt install -y libevent-dev ncurses-dev gcc make bison pkg-config autoconf automake git
```

#### Clone and build
```bash
git clone https://github.com/tmux/tmux.git /tmp/tmux
cd /tmp/tmux
sh autogen.sh
./configure && make && sudo make install
```

#### Configure
```bash
# Install dependencies
sudo apt update && \
sudo apt install -y git stow fzf

git clone --recurse-submodules https://github.com/mrjesma/dotfiles.git ~/repos/dotfiles

cd ~/repos/dotfiles
stow -t ~/ bin
stow -t ~/ tmux
~/.config/tmux/plugins/tpm/bin/install_plugins
```


# NEOVIM

#### Install dependencies
```bash
sudo apt update && \
sudo apt install -y ninja-build gettext cmake unzip curl build-essential git
```

#### Clone and build
```bash
git clone https://github.com/neovim/neovim /tmp/neovim
cd /tmp/neovim
make CMAKE_BUILD_TYPE=Release && sudo make install
```

#### Configure
```bash
# Install dependencies
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt update && \
sudo apt install -y git stow ripgrep nodejs python3-venv 

git clone --recurse-submodules https://github.com/mrjesma/dotfiles.git ~/repos/dotfiles

cd ~/repos/dotfiles
stow -t ~/ nvim
```


# NAVI

#### Install dependencies
```bash
sudo apt update && \
sudo apt install -y fzf
```

#### Install using script
```bash
# Installation from source required rustc 1.81 or newer, while the currently  rustc version that `apt` was providing was 1.75.0

sudo BIN_DIR=/usr/local/bin bash -c "$(curl -sL https://raw.githubusercontent.com/denisidoro/navi/master/scripts/install)"
```

#### Configure
```bash
# Install dependencies
sudo apt update && \
sudo apt install -y git stow 

git clone --recurse-submodules https://github.com/mrjesma/dotfiles.git ~/repos/dotfiles

cd ~/repos/dotfiles
stow -t ~/ navi
```


# ANSIBLE

#### Install packages
```bash
sudo apt update && \
sudo apt install -y sshpass ansible python3-pyvmomi python3-proxmoxer
```

#### Configure
```bash
# Install dependencies
sudo apt update && \
sudo apt install -y git stow 

git clone --recurse-submodules https://github.com/mrjesma/dotfiles.git ~/repos/dotfiles

cd ~/repos/dotfiles
stow -t ~/ ansible
```


# DOCKER

#### Set up repo
```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
```

#### Install packages
```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 ```

#### Non-root user configuration
```bash
sudo groupadd docker
sudo usermod -aG docker $USER

# Log out and log back in so that your group membership is re-evaluated or run:
newgrp docker
```

#### Configure log
```bash
echo \
'{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}' | sudo tee -a /etc/docker/daemon.json

sudo systemctl restart docker
```


