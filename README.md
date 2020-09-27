# ansible-playbooks

This is an attempt to get some order into the way I manage my small
but growing army of embedded computers - mostly Raspberry Pi variants.

The goal is to be able to grab the latest Raspbian image, burn it onto
a card (using Etcher), and then easliy run a few playbooks to get the
Pi configured to suit my needs. It's way easier than writing down a
number of step by step instructions

# First Steps

Install Ansible on your host machine - later we will make this a 
Raspberry Pi as well :-)

Now run the inital playbook that permanently enables ssh, configures
the local wifi, changes the hostname, and then powers down the rpi:

```
ansible-playbook -v -i ./hosts -u pi -k fresh-rpi.yml --extra-vars '{"hostname":"some-new-hostname"}' 
```

To create an ansible-vault encrypted variable, such as a password:

mkpasswd --method=sha-512

ansible-vault encrypt_string '$6$t5eSXI8UxtBoU7JV$9CkhiIIT5UOGfRK7vlLRtPBxfZM7IMxkSlkBwSPduldwj/VsKAKBzbHUlUPDEWkwBJfbZHSfFvXZNNMaoN7ym1' --name pi_password

Next we run another playbook that changes the default pi user password
to something from this Ansible vault, and then adds me as a new user
with sudo, adds me to all the standard pi groups and adds my public
ssh key from GitHub so that I can log into this pi without ever having
to enter the password again!

```
ansible-playbook -v -i ./hosts -u pi -k my-rpi.yml --ask-vault-pass --extra-vars '{"hostname":"zwischenwagen"}' 

```

# Next Steps

From this point forward, we can use Ansible playbooks without a lot of extra parameters
so let's get started with a few basic roles - roles are the way we can easily build up
playbooks using known-good methods.

- Update to latest packages

```
ansible-playbook -v -i ./hosts rpi-jekyll.yml 
```

This playbook can take a loooong time to run as it updates the apt cache, installs a few
bigger packages, and builds jekyll and the bundler. opens up prot 4000 etc...

# Starting jekyll

The jekyll server can start like this:

bundle exec jekyll serve --host=0.0.0.0 --incremental

# Getting Started With Jekyll

Use Gem based themes to make life easier, start with a new folder and
create a git repo inside it:

mkdir newsite
cd newsite
git init

Now follow the instructions in the MinimalMistakes QuickStart Guide:
https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/

In particular, we are following the "Fresh Start" section

Create a blank Gemfile and put this inside:

gem "minimal-mistakes-jekyll"

And create a blank "\_config.yml" and copy the default contents into it from
the MinimalMistakes repo

Now get things installed by running `bundle` once to get everything installed



# Acknowledgment

Much of the information used to assemble these playbooks comes from:

- https://docs.ansible.com/ansible/latest/user_guide
- https://github.com/chesterbr/chester-ansible-configs
- https://github.com/glennklockwood/rpi-ansible
- https://github.com/giuaig/ansible-raspi-config
- https://github.com/jonashackt/raspberry-ansible
- https://github.com/motdotla/ansible-pi
- https://github.com/davidderus/ansible-rpi
