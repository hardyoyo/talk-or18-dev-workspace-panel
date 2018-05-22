# Developer Workspaces Panel
## Introductions and Vagrant Up

Hardy Pottinger

Digital Library Software Developer, UCLA Library

@fa[twitter] @hardy.pottinger

@fa[envelope] hpottinger@library.ucla.edu

---
# Who?

---
# Why?

---?image=assets/images/GermanSubmarineControlRoom1918.jpg&size=auto

---
# Predictable, Repeatable, Sharable
* Vagrant
* Docker
* wrappers for Docker (Codenvy, Docksal, Lando etc.)

---
# Vagrant Up
https://www.vagrantup.com/
* "Development Environments Made Easy"
* How to install: download the thing, click on the thing, do what you're told.
* Yes, even Linux users.

---
# Vagrant is a tool for managing a VM
* _Providers_: VirtualBox, VMWare, AWS, and more
* _Provisioners_: Ansible, Chef, Puppet, and more
* _Plugins_: Hostname/DNS, Caching, Notifications
* _Vagrant Share_: Tunneling via *ngrok* to your workspace for quick demos

---
# Vagrant is Slow
* Virtual Machines eat up a ton of RAM and CPU
* Watching a machine provision itself is interesting the first few times
* Recommendation: find something to do with your hands

---
# Vagrant is a way to communicate
* Show ops how all the pieces work
* Show off your work in progress to your boss or "customer"
* Bounce ideas off other developers
* Impromptu pair programming at at distance

---
# Why I keep coming back to Vagrant: communication
* Let's pretend you all are my "customers"
* It's demo time!
* And code! On screen! Awesome!

---
# The Vagrant-DSpace project folder
```
├── apt-spy-2-bootstrap.sh
├── config
│   ├── dotfiles
│   │   ├── inputrc
│   │   ├── maven_settings.xml
│   ├── local-bootstrap.sh
│   ├── local.yaml
├── content
│   ├── aips
│   ├── random_metadata_1K.csv
│   ├── random_metadata.csv
├── dspace-src
├── hiera.yaml
├── increase-swap.sh
├── puppet-bootstrap-ubuntu.sh
├── puppet.conf
├── Puppetfile
├── README.md
├── setup.pp
└── Vagrantfile
```
@[1,6,14,15](shell provisioner scripts)
@[19](Puppet provisioner)
@[20]
---
# Vagrantfile: pick a base box
```ruby
config.vm.box = "geerlingguy/centos7"
```
---
# Vagrantfile: a simple shell provisioner
```ruby
config.vm.provision :shell, :name => "running puppet-bootstrap", :path => "puppet-bootstrap-ubuntu.sh"
```
---
# Vagrantfile: a shell provisioner with an override
```ruby
if File.exists?("config/apt-spy-2-bootstrap.sh")
    config.vm.provision :shell, :name => "apt-spy-2, locating a nearby mirror", :inline => "echo '   > > > running local apt-spy2 to locate a nearby mirror (for quicker installs). Do not worry if it shows an error, it will be OK, there is a fallback.'"
    config.vm.provision :shell, :name => "apt-spy-2, running custom apt-spy-2-bootstrap", :path => "config/apt-spy-2-bootstrap.sh"
else
    config.vm.provision :shell, :name => "apt-spy2, locating a nearby mirror", :inline => "echo '   > > > running default apt-spy2 to locate a nearby mirror (for quicker installs). Do not worry if it shows an error, it will be OK, there is a fallback.'"
    config.vm.provision :shell, :name => "apt-spy2, running default apt-spy-2-bootstrap", :path => "apt-spy-2-bootstrap.sh"
end
```
---
#Vagrantfile: a Puppet provisioner
```ruby
config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "."
    puppet.manifest_file = "setup.pp"
    # NOTE: If you are hitting issues/errors with Puppet provisioning,
    # adding "--debug" to the 'puppet.options' will provide debug logs.
    puppet.options = "--verbose"
end
```
---
#Vagrant: lots of room to play
 * github.com/dspace/vagrant-dspace
 * github.com/Islandora-Labs/islandora_vagrant
 * github.com/fcrepo4-exts/fcrepo4-vagrant
 * github.com/samvera-labs/samvera-vagrant
 * https://github.com/geerlingguy/ansible-vagrant-examples

---
# Thanks!

@fa[twitter] @hardy.pottinger

@fa[envelope] hpottinger@library.ucla.edu

slides: [github.com/hardyoyo/talk-or18-dev-workspace-panel](https://github.com/hardyoyo/talk-or18-dev-workspace-panel)
