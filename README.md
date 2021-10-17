# Devcontainers on Windows without Docker Desktop demo

This repo demonstrates how to use VS Code Development Containers on Windows 10 without using Docker Desktop. It accompanies my blog post available [here](https://ajf58.github.io/).

# Getting started

This project assumes that you already have VS Code and WSL2 running on your Windows machine (the Host). We'll use an Ubuntu 20.04 based distro when we're working in WSL2. If you don't have it already, install it from the command line.

```powershell
PS C:\Users\Andrew> wsl.exe --install --distribution Ubuntu-20.04
Ubuntu 20.04 LTS is already installed.
Launching Ubuntu 20.04 LTS...
```
Once the distro is installed, opened a shell in it. 
```powershell
PS C:\Users\Andrew> wsl.exe -d Ubuntu-20.04
andrew@win10-laptop:/mnt/c/Users/Andrew$
```
For best filesystem performance it's recommended that you change into the WSL filesystem. Clone this repo.
```sh
andrew@win10-laptop:/mnt/c/Users/Andrew$ cd ~/
andrew@win10-laptop:~$ git clone https://github.com/ajf58/devcontainer-demo.git
```

Checkout this code to your Windows machine. It's recommend that you checkout the code into the WSL2 filesystem and open the folder in VS Code.
TODO
