# Installation of sourcebox on Ubuntu Focal
### 1. Ubuntu Version
Install a VM with Ubuntu 20.04.3 LTS (Focal Fossa). The Desktop and Server Version are available at https://releases.ubuntu.com/20.04/. I used the Desktop Version to have a GUI.
HyperV and VMware were both tested and work for this purpose. Choose Hardware according to your needs. 4 vCPU-cores and 4GB RAM should be the minimum of the VM. Also install the ubuntu on a BTRFS Filesystem (You can choose the filesystem during the ubuntu install). That way you won't have to create a loopmount later on.
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
1. `cd ~`
2. `git clone https://github.com/ebertmi/sourcebox-sandbox.git`
### 6. Compile the sourcebox
1. Go in the cloned github directory `cd ~/sourcebox-sandbox`
2. Compile the sourcebox with the command `npm install -g`
3. After Compilation link the sourcebox aswell `sudo ln -s /home/<username>/.nvm/versions/node/v7.10.1/bin/sourcebox /usr/local/bin/sourcebox`
# Installation of the Sourcebox-LTI_Bridge \[WIP\]
### Dependencies
#### MongoDB: 
1. One of the dependancies of the old mongoDB version the sourcebox_lti-bridge uses is a package called __libssl.so.1.0.0__. In order to install this package you will need to add a source to your ubuntu sources list. Open the sources list with a text editor `sudo nano /etc/apt/sources.list` then add this line `deb http://security.ubuntu.com/ubuntu xenial-security main` to the sources list. Run `sudo apt update` and then install the libssl-package `sudo apt install libssl1.0.0`.
2. Install MongoDB Dependencies: `sudo apt-get install libcurl4 openssl liblzma5`
3. Cd to your home directory `cd ~` and download MongoDB v3.4.6 Archive from the mongoDB Website `wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1604-3.4.6.tgz`. *If somehow the link changed in the future here is a link to the mongoDB archive:* [https://www.mongodb.com/download-center/community/releases/archive]
4. Unpack the archive `tar -zxvf mongodb-linux-*-3.4.6.tgz` and then place it in a folder of your preference. For example `mkdir mongodb-3.4.6`
5. Link the mongoDB binaries to the PATH `sudo ln -s  /home/<username>/mongodb-3.4.6/bin/* /usr/local/bin/`
6. Create Folders for MongoDB and own them: Data: `sudo mkdir -p /var/lib/mongo` Logs: `sudo mkdir -p /var/log/mongodb` `sudo chown <username> /var/lib/mongo` `sudo chown <user> /var/log/mongodb`
7. Run MongoDB: `mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork`
8. Check that MongoDB started successfully: `cat /var/log/mongodb/mongod.log` it should output __\[initandlisten\] waiting for connections on port 27017__ at the end. If something went wrong try deleting the folders you created for mongoDB Logs and Data and repeat the process beginning from step 5 on.
#### Node:
The Node Version 7.* used for compilation of the sourcebox can't be used for the soucebox_lti-bridge.
1. Install the latest 8.* Node version via nvm `nvm install 8.17.0` and activate it `nvm use 8.17.0`.
2. Remove the links for Node 7.* in the /usr/local/bin folder `sudo rm /usr/local/bin/node` `sudo rm /usr/local/bin/npm`.
3. Create links for the 8.* Node version `sudo ln -s /home/<username>/.nvm/versions/node/8.17.0/bin/node /usr/local/bin/` `/home/<username>/.nvm/versions/node/8.17.0/bin/npm /usr/local/bin/`
4. Check your Node version with the command `node --version` and `sudo node --version` you should get __8.17.0__ in both cases.
#### Moodle:
You will need a Moodle instance to test the sourcebox_lti-bridge (*Well if you wouldn't have one you wouldn't be doing all of this anyways...*) In the Moodle you will have to configure the tool as a lti plugin. There are some good tutorials online on doing that i recommend these two from the official Moodle documentation: [https://docs.moodle.org/311/de/Als_LTI-Tool_bereitstellen] [https://docs.moodle.org/311/de/Externes_Tool_konfigurieren]. *If the link might be dead in the future just google for a tutorial.* If you got a Moodle Instance ready and running you can configure the LTI-Plugin just like in the guide and put in a key and password of your liking.
### Compiling the sourcebox_lti-bridge
1. Cd in the sourcebox_lti-bridge directory `cd ~/sourcebox_lti-bridge`
2. Compile the sourcebox_lti-bridge `npm install -g` This will take some time.
3. Alter the config file of the sourcebox_lti-bridge `nano ~/sourcebox_lti-bridge/config/default.json` and put the following content in there: 
   `{
    "serverUrl": "url / ip of your machine",
    "sourcebox": {
        "loopMount": "/home/<username>/sourceboxContent",
        "options": {
            "diskspace": "30MB",
            "memory": "30MB",
            "cpu": 9,
            "processes": 20
        }
    },
    "sourceboxServer": {
        "authTimeout": 10000,
        "sessionTimeout": 300000
    },
    "lti": {
        "consumer_key": "<key>",
        "consumer_secret": "<secret>"
    },
    "database": {
        "url": "mongodb://localhost:27017/sourcebox-lti_bridge",
        "fileLimitKB": "10",
        "maxNumberOfFiles": "5"
    },
    "debug": false
}`
4. create the folder sourceboxContent in your home folder `mkdir ~/sourceboxContent`
5. 
