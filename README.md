Installation of sourcebox on Ubuntu Focal
==================
### 1. Ubuntu Version
Install a VM with Ubuntu 20.04.3 LTS (Focal Fossa) Desktop Version available at https://releases.ubuntu.com/20.04/.
HyperV and VMware were both tested and work for this purpose. Choose Hardware according to your needs. 4 vCPU-cores and 4GB RAM should be the minimum of the VM.
### 2. Dependencies
Install the required dependencies:
`sudo apt install make gcc nodejs git btrfs-progs libcap-dev build-essential lxc lxc-dev git curl`
### 3. Node.js
The sourcebox needs node module version 51 which can be found in __7.*__ node releases. I recommend the latest one __7.10.1__. In order to easily install the old node version the node version manager nvm is used. It can be found on github https://github.com/nvm-sh/nvm. 
1. Run this command __NOT AS SUDO__ to install nvm on your system. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash`
2. Close the terminal and open a new terminal in order for nvm to work
3. install node 7.10.1 `nvm install 7.10.1`
4. check the installed node version `node --version` you should get a reply __7.10.1__
