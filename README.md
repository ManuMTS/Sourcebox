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


### 4. Create a sourcebox sandbox template container

Once the sandbox is installed it is quite easy to create and manage sourcebox instances. You can either use a loop mount or a pure BTRFS partition for the sourcebox container, though loop mounts work really good.

Create a new sandbox with the following interactive command. For ease of use select `debian` and `jessie` and same platform as host. Choose a sufficent template container size if you plan to install something like Java or other bigger dependencies.

```bash
sudo sourcebox create --interactive /path
```

See `README.md` for a description of the individual commands to create and manage sourcebox sandbox instances.

### Next steps

In order to make the sandboxes accessible for users, you need to either use our [sandbox-server](https://github.com/waywaaard/sandbox-server) with configurable limits and authentication and a test page. Or build your own using the low-level [sourcebox-web](https://github.com/waywaaard/sourcebox-web) libraries for clients and servers using the sandbox. 

The libarires provide:

* `RemoteStream` implementation for bidirectional dataflow between clients and sandboxes
* Server library for `nodejs` that uses `sourcebox-sandbox` for starting and terminating user sandboxes, remote file commands and compile and execution functions
* Client library that allows to execute commands and a process interface for binding IO-Streams from the sandbox
