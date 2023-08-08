## Following are the steps to set up vs code and git on Ubuntu

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

### Install git
`sudo apt install git`

#### Configure git
Run the fillowing commands to configure git user name and email address
```
git config --global user.name "<username>"
git config --global user.email "<your email>"
```
### Set up git on VS Code
1. Lauch VS Code
2. Click on the gear button located at the bottem left corner and click `Settings`
3. Type *git:enabled* and tick the checkbox
4. Sign in to GitHub from VS Code
    - Click on *Account* -> *Turn on Settings Sync* -> *Sign in* -> *Sign in with GitHub*
