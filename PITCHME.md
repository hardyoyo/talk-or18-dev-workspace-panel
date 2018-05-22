# Developer Workspaces Panel
## Introductions and Vagrant Up

Hardy Pottinger

Digital Library Software Developer, UCLA Library

@fa[twitter] @hardy.pottinger

@fa[envelope] hpottinger@library.ucla.edu

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
"Development Environments Made Easy"
* How to install: download the thing, click on the thing, do what you're told.
* Yes, even you Linux users.

---
# Vagrant is a wrapper for managing a VM
* "Providers": VirtualBox, VMWare, AWS, and more
* "Provisioners": Ansible, Chef, Puppet, and more
* "Plugins": Hostname/DNS, Caching, Notifications
* Tunneling via *ngrok* to your workspace for quick demos

---
# Vagrant is Slow
* Virtual Machines eat up a ton of RAM and CPU
* Watching a machine provision itself is interesting the first few times
* But it gets old

---
# Vagrant is a way to communicate
* Show ops how all the pieces work
* Show off your work in progress to your boss or "customer"
* Bounce ideas off other developers
* Impromptu pair programming at at distance

---
# Why I keep coming back to Vagrant: communication

 a. link to a page on localhost to prove DSpace is running
 b. look at how the Vagrantfile works
 c. point out customization points
 d. shoutout Vagrant plugins
 e. shoutout Vagrant Share
 f. shoutout to the DSpace Developer Show & Tell series


---
# The rest is just details

@fa[twitter] @hardy.pottinger

@fa[envelope] hpottinger@library.ucla.edu

slides: [github.com/hardyoyo/talk-or18-dev-workspace-panel](https://github.com/hardyoyo/talk-or18-dev-workspace-panel)
