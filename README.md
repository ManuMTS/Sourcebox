Steps so far
==================
Testing with Ubuntu 20 LTS and Testing with Debian 11 right now...
1. Add `cgroup_enable=memory swapaccount=1` to `GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub`
2. Run `update-grub`
3. Reboot the VM (`sudo reboot`)
### 2. Dependencies
Install the required dependencies:
```bash
su
apt-get install curl
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt-get install -y nodejs
sudo apt-get update
sudo apt-get install make gcc nodejs git btrfs-progs libcap-dev build-essential lxc lxc-dev
```
* Try to install using npm with `npm install https://github.com/ebertmi/sourcebox-sandbox -g` -> doesn't work well with the vanilla install of node
* installation of node via nvm `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash`

Error when building. The sourcebox-lxc library doesnt seem to work properly.
