Steps so far
==================

Testing with Ubuntu 20 LTS
Testing with Debian 11

1. Add `cgroup_enable=memory swapaccount=1` to `GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub`
2. Run `update-grub`
3. Reboot the VM (`sudo reboot`)
4. The `memory cgroup` should be now enabled

See http://serverfault.com/a/762815 for more information about GRUB...

### 2. Dependencies

Install the required dependencies:

```bash
su
apt-get install curl
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt-get install -y nodejs
sudo apt-get update
sudo apt-get install make gcc nodejs git btrfs-progs libcap-dev build-essential lxc lxc-dev
sudo -E npm install -g git+ssh://git@github.io:waywaaard/sourcebox-sandbox
```

**Hint:** Also try to update `npm` itself with `sudo npm install npm --upgrade -g` and maybe you need to link the node binary `sudo ln -s /usr/bin/nodejs /usr/bin/node`.

### 3. Install sourcebox-sandbox

There are multiple ways of install the sourcebox-sandbox. The easiest is using npm and installing it using the `-g` flag:

* Try to install using npm with `npm install https://github.com/ebertmi/sourcebox-sandbox -g`
* Download the `https://github.com/ebertmi/sourcebox-sandbox` as a zip and unzip.
* Use `git clone ...` by using either SSH (requires your key) or HTTPS

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
