# Developer Workspaces Panel
## Introductions and Vagrant Up

Hardy Pottinger

Digital Library Software Developer, UCLA Library

@fa[twitter] @hardy.pottinger

@fa[envelope] hpottinger@library.ucla.edu

Note:
Hi, welcome, thanks for coming. We have a four demos on the slate for you today.
We're going to show you how we have set up and used development environments,
and a few tools we use to manage these environments. We only have about 10
minutes for each of us, so timing will be tight, please hold your questions
for the end, and if you don't get a chance to ask your question today, we're all
quite friendly, stop us in the hall, send us an e-mail or ping us on Slack,
we love talking about this stuff. But first, who are we?

---
# Who?
* *Terry Brady*, Georgetown University Library
* *Liz Krznarich*, ORCID
* *Kate Lynch*, University of Pennsylvania
* *Hardy Pottinger*, UCLA Library

Note:
Will the panelists wave when I read your name? In alphabetical order, we have
Terry Brady from Georgetown, Liz Krznarich from ORCID, Kate Lynch from Penn, and
myself, Hardy Pottinger from UCLA.

When I talk about working with the tools we'll demo today, one word keeps coming
up....

---
# Why?

Note:
And that one word can mean many things:  
Why are we here? Why work this way? Why is this important for repositories?
I hope you don't mind, but I'd like to answer these questions with a picture.

---?image=assets/images/GermanSubmarineControlRoom1918.jpg&size=auto

Note:
Coding is really complicated. Getting your workspace set up correctly is a big
job. Even if you're happy with your current environment, what happens if you
get a new computer? Or a new developer needs to get up to speed? In libraries
we have many student workers, is it the best use of anyone's time to plop them
down in front something like that, with just "Good luck, kid? You'll figure
it out." Is this really how we handle any other task? No! We're developers, we
automate things! Ideally, our development environments should be...

---
# Predictable, Repeatable, Sharable, Disposable
* Vagrant
* Docker
* wrappers for Docker (Codenvy, Docksal, Lando, Stack Car, etc.)

Note:
Predictable, Repeatable, Sharable, Disposable.

TODO: add more here

There are several tools to help with this kind of automation, we'll talk about
a few of them today. First up, I'll talk about a tool I started out with
and keep coming back to, Vagrant.

---
# Vagrant Up
https://www.vagrantup.com/
* "Development Environments Made Easy"
* How to install: download the thing, click on the thing, do what you're told.
* Yes, even Linux users.

Note:
I'm going to start both a real vagrant up of a suspended VM, which takes about a
minute to boot up, and a screencast of a full vagrant up from scratch, sped up
a few times, just to give you an idea what a full vagrant up looks like.

---
# Vagrant is a tool for managing a VM
* _Providers_ - VirtualBox, VMWare, AWS, and more
* _Provisioners_ - Ansible, Chef, Puppet, shell scripts
* _Plugins_ - Hostname/DNS, Caching, Notifications
* _Vagrant Share_ - Tunneling via *ngrok* to your workspace for quick demos

Note:
Vagrant is kind of a wrapper around some sort of virtual machine. In Vagrant
this is achieved by what they call a "provider". There are a few different
providers available, VirtualBox, VMWare, AWS and other cloud providers. These
boxes are then provisioned using some sort of "provisioner," all the major
configuration management software solutions are supported: Ansible, Chef,
Puppet, shell scripts. You can mix and match provisioners, it's pretty
common to see shell scripts handle small tasks while something like Puppet or
Ansible handles the heavy lifting. And the lifting can get pretty heavy,
virtual machines aren't exactly the best-behaved guests to keep on your
workstation. Unfortunately, I have to warn you...

---
# Vagrant is Slow
* Virtual Machines eat up a ton of RAM and CPU
* Watching a machine provision itself is interesting the first few times
* Recommendation: find something to do with your hands

Note:
Vagrant is slow. This isn't Vagrant's fault, it's just that running a virtual
machine on your workstation takes resources away from your main operating
system. Since you'll be doing a bit a waiting, I recommend picking up a hobby,
have something to do with your hands while the text scrolls by. For me, it's
a yo-yo. No, no tricks right now, I mentioned I only have 10 minutes, right?
Ask me later.

What is Vagrant doing while you're waiting? Vagrant provisions virtual
machines, that's its job. If that's what you're after, it's a fantastic tool.
In fact, the reason I keep coming back to Vagrant as a tool is...

---
# Vagrant is a way to communicate
* Show ops how all the pieces work
* Show off your work in progress to your boss or "customer"
* Bounce ideas off other developers
* Impromptu pair programming at at distance

Note:
Vagrant is a way to communicate. As a digital library developer, one of my jobs
is to show our operations folks how a new service works. Vagrant lets you see
and work with all the moving parts. It's also a way to show stakeholders or
customers your work in progress. Or with Vagrant Share, fellow developers can
see your code in action, or even code by your side from wherever they happen
to be, in the same room, or on the opposite side of the world. But you don't
have to take my word for it...

---
# Communication
* Let's pretend you all are my "customers"
* It's demo time!
* And code! On screen! Awesome!

Note:
Let's pretend you all are my customers! Remember I started up a suspended VM?
I think it's ready for us, let's see. Oh, yes, here's my Vagrant-DSpace instance
running on my notebook at http://dspace.vagrant.test:8080/ If we have time at
the end I'll work a bit with this VM, to give you a feel for how I work. For
me the command line is my IDE, so just shelling in with the vagrant ssh
command, running vim and git commands are enough for me, and a good way to
deal with the slowness of running a virtual machine. Some people like to
use shared folders and their own IDE on their workstation. That's too slow
for me, and I like working on the command line anyway. But, save that for the
end, if we have time, let's look at how Vagrant does what it does.

---
## The Vagrant-DSpace project folder
http://github.com/dspace/vagrant-dspace
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
@[20](the Vagrantfile)

Note:
Here's what the Vagrant-DSpace folder looks like. This is all documented on the
project site in the README, it's great. The things I want to point out are the
shell provisioner scripts, the Puppet provisioner, and the Vagrantfile.

---
# Vagrantfile
## pick a base box
```ruby
config.vm.box = "geerlingguy/centos7"
```

Note:
The simplest Vagrantfile just defines a base box to use. This is the starting
point for the rest of your environment.

---
# Vagrantfile
## a simple shell provisioner
```ruby
config.vm.provision :shell, :name => "running puppet-bootstrap",
  :path => "puppet-bootstrap-ubuntu.sh"
```

Note:
Here's an example of a simple shell provisioner being called. Pretty
straightforward: please run this script.

---
# Vagrantfile
## a shell provisioner with an override
```ruby
if File.exists?("config/apt-spy-2-bootstrap.sh")
    config.vm.provision :shell, :name => "apt-spy-2, running custom
      apt-spy-2-bootstrap", :path => "config/apt-spy-2-bootstrap.sh"
else
    config.vm.provision :shell, :name => "apt-spy2, running default
      apt-spy-2-bootstrap", :path => "apt-spy-2-bootstrap.sh"
end
```
@[1](if there is a matching shell provisioner in the config folder)
@[2-3](run it)
@[4](otherwise...)
@[5-6](run the default shell provisioner)

Note:
Here's a more complicated pattern, one which we use a few times for
Vagrant-DSpace. Doing this allows you optionally override and customize
the shell provisioners by copying the default one to the config folder
and then modifying it to better suit your needs.

---
# Vagrantfile
## a Puppet provisioner
```ruby
config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "."
    puppet.manifest_file = "setup.pp"
    puppet.options = "--verbose"
end
```

Note:
Here's the call we make to run the Puppet provisioner, which is the main
provisioner we use for Vagrant-DSpace. It roughly says: using Puppet, run
this Puppet manifest, and be really wordy about the output of the script.

---
# Vagrant
## lots of room to play
 * github.com/dspace/vagrant-dspace
 * github.com/Islandora-Labs/islandora_vagrant
 * github.com/fcrepo4-exts/fcrepo4-vagrant
 * github.com/samvera-labs/samvera-vagrant
 * github.com/geerlingguy/ansible-vagrant-examples

Note:
There are many Vagrant workspace environments in existence, created and
supported by the repository community. If you're interested, I recommend playing
with a few of these. It's a fun way to get your feet wet, and see how all the
parts fit together. Or, if you want to build your own environment, there are
plenty of good ideas to swipe from all of these existing projects. Seriously,
geerlingguy's examples are like the kitchen sink, do look there before you
start on something new.

---
# MOAR Demo?
Note:
I hope we have time for a bit more of a demo here. Just vagrant ssh, change
something, prove it works.

---
# Thanks!

Slides: [github.com/hardyoyo/talk-or18-dev-workspace-panel](https://github.com/hardyoyo/talk-or18-dev-workspace-panel)

Image Credit: *German Submarine, UB-110.  
Photo of Control room looking aft, starboard side*  
Tyne & Wear Archives & Museums
https://www.flickr.com/photos/29295370@N07/8441846768

Former Panelists: *Erin Fahy*, Stanford &  
*Anusha Ranganathan*, Cottage Labs

Note:
Thanks for coming, please hold questions for the end after all the panelists
have had a chance to talk. In addition to the panelists here with us today,
I also want to thank Erin Fahy from Stanford and Anusha Ranganathan from
Cottage Labs, who both were scheduled to be panelists but things didn't work
out and they could not make it. They contributed to the discussion that led up
to this panel, and have helped ensure the quality of the presentations are
high. I appreciate all the panelists' continued guidance and hope to work with
them again.
