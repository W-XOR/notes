## Following are the steps to set up vs code and git

### Install VS Code
VS Code could be installed via either of the following ways:
1. .deb package
- Download `.deb` package from [VS Code Download Page](https://code.visualstudio.com/Download)
- Run `sudo install ./file.deb`

2. Microsoft Repository
- Add Microsoft repo and key 
```
sudo apt-get install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
```
-  Update package cache and install the `code` package
```
sudo apt install apt-transport-https
sudo apt update
sudo apt install code
```