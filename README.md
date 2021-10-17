# Devcontainers on Windows without Docker Desktop demo

This repo demonstrates how to use VS Code Development Containers on Windows 10 without using Docker Desktop. It accompanies my blog post available [here](https://ajf58.github.io/).

# Getting started

This project assumes that you already have VS Code and WSL2 running on your Windows machine (the Host). We'll use an Ubuntu 20.04 based distro when we're working in WSL2. If you don't have it already, install it from the command line.

```powershell
PS C:\Users\Andrew> wsl.exe --install --distribution Ubuntu-20.04
Ubuntu 20.04 LTS is already installed.
Launching Ubuntu 20.04 LTS...
```
Once the distro is installed, open a shell in it. 
```powershell
PS C:\Users\Andrew> wsl.exe -d Ubuntu-20.04
andrew@win10-laptop:/mnt/c/Users/Andrew$
```

## Setup Podman in the Ubuntu-20.04 distro

The [official documentation](https://code.visualstudio.com/docs/remote/containers#_getting-started) for working with Dev Container on Windows recommends using [Docker Desktop](https://www.docker.com/products/docker-desktop) on the Local OS when using Windows. In this demo, instead we're using the Ubuntu-20.04 WSL environment as the "Local OS", and then use Podman running within WSL to manage the Dev Container.

The [Podman installation instructions](https://podman.io/getting-started/installation) include a link to separate instructions for installing Podman in WSL2, but I found the [instructions for Ubuntu](https://podman.io/getting-started/installation#linux-distributions) to work as-is. Since we're using an Ubuntu 20.04 based distro, use the instructions for using the [Kubic project](https://build.opensuse.org/package/show/devel:kubic:libcontainers:stable/podman), repeated here:

```bash
. /etc/os-release
echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
curl -L "https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/Release.key" | sudo apt-key add -
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get -y install podman
```
Verify that Podman has installed successfully:
```bash
andrew@win10-laptop:~$ which podman
/usr/bin/podman
```

The Ubuntu 20.04 distro is now ready to use with Dev Containers in VS Code.

## Configure VS Code 

Install the [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension. You can do this manually or by installing the recommended extensions in this Workspace.

### Configure VS Code to use Podman

By default, VS Code tries to execute `docker` when creating Dev Containers. The user configuration `remote.containers.dockerPath` can be used to change this, e.g. e.g. using `Ctrl+,` to bring up the settings view. Change this setting to the Podman path in the WSL, i.e.
```json
{
    "remote.containers.dockerPath": "/usr/bin/podman",
}
```

### Using workspaces in WSL

If the code is stored in the WSL filesystem, install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension, which allows VS Code to work with code in the WSL filesytem. Using this repo, for example, first clone the repo into the filesystem.
```bash
andrew@win10-laptop:/mnt/c/Users/Andrew$ cd ~/
andrew@win10-laptop:~$ git clone https://github.com/ajf58/devcontainer-demo.git
```
and then open the folder in VS Code:
```
andrew@win10-laptop:~$ code ./devcontainer-demo
```

### Using the Dev Container

Use the Command Palette (`F1`) to run the __Remote-Containers: Open Folder in Container...__ command or any of the other approaches in the [official documentation](https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container). After VS Code has created and configured the container, open a terminal in VS Code and you'll be inside the Dev Container, ready to do development in an isolated, controlled environment.
```bash
vscode ➜ /workspaces/devcontainer-demo (main) $ pwd
/workspaces/devcontainer-demo
vscode ➜ /workspaces/devcontainer-demo (main) $ cat /etc/lsb-release 
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=21.04
DISTRIB_CODENAME=hirsute
DISTRIB_DESCRIPTION="Ubuntu 21.04"
```

## TODO rearrange content for working with Windows filesystem.

### Configure VS Code to work with WSL


Depending on whether VS Code is open in workspace on Windows or within a folder in WSL the command may execute in a different context. As of VS Code 1.61 users can enable the new [execute in WSL setting](https://github.com/microsoft/vscode-docs/blob/main/remote-release-notes/v1_61.md#execute-in-wsl-setting). Both of these settings are user-specific and need to be changed to work with this setup. Either adjust these through the UI (e.g. using `Ctrl+,` to bring up the settings view), or by adding the JSON fields below to the user's config file.
```json
{
    "remote.containers.dockerPath": "/usr/bin/podman",
    "remote.containers.executeInWSL": true
}
```

Checkout this code to your Windows machine. It's recommend that you checkout the code into the WSL2 filesystem and open the folder in VS Code.


VS Code is now ready to use with Dev Containers.

## Using the Dev Container 

For best filesystem performance it's recommended that you change into the WSL filesystem. Clone this repo.
```sh
andrew@win10-laptop:/mnt/c/Users/Andrew$ cd ~/
andrew@win10-laptop:~$ git clone https://github.com/ajf58/devcontainer-demo.git
```
Open the folder in VS Code:
```
andrew@win10-laptop:~$ code ./devcontainer-demo
```

