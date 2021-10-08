Installation of sourcebox on Ubuntu Focal
==================
### 1. Ubuntu Version
Install a VM with Ubuntu 20.04.3 LTS (Focal Fossa). The Desktop and Server Version are available at https://releases.ubuntu.com/20.04/. I used the Desktop Version to have a GUI.
HyperV and VMware were both tested and work for this purpose. Choose Hardware according to your needs. 4 vCPU-cores and 4GB RAM should be the minimum of the VM.
### 2. Dependencies
Install the required dependencies:
`sudo apt install make gcc nodejs git btrfs-progs libcap-dev build-essential lxc lxc-dev git curl python2.7`
### 3. Node.js
The sourcebox needs node module version 51 which can be found in __7.*__ node releases. I recommend the latest one __7.10.1__. In order to easily install the old node version the node version manager nvm is used. It can be found on github https://github.com/nvm-sh/nvm. 
1. Run this command __NOT AS SUDO__ to install nvm on your system. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash`
2. Close the terminal and open a new terminal in order for nvm to work
3. install node 7.10.1 `nvm install 7.10.1`
4. check the installed node version `node --version` you should get a reply __7.10.1__
### 4. Create Links
Sourcebox will need to be run as sudo. You need to create a link to npm and node.
1. `sudo ln -s /home/<username>/.nvm/versions/node/v7.10.1/bin/node /usr/local/bin/node`
2. `sudo ln -s /home/<username>/.nvm/versions/node/v7.10.1/bin/npm /usr/local/bin/npm`
Python also needs a link.
1. `sudo ln -s /usr/bin/python2.7 /usr/bin/python`
### 5. Clone Git Repository
Clone the git repository in a folder of your choice for example in the home folder.
1. `cd /home/<username>`
2. `git clone https://github.com/ebertmi/sourcebox-sandbox.git`
### 6. Compile the sourcebox
1. Go in the cloned github directory `cd home/<username>/sourcebox-sandbox`
2. Compile the sourcebox with the command `npm install -g`
3. After Compilation link the sourcebox aswell `sudo ln -s /home/<username>/.nvm/versions/node/v7.10.1/bin/sourcebox /usr/local/bin/sourcebox`
