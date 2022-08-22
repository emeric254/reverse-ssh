# reverse-ssh
This explain how to setup a reverse ssh


## What is a reverse ssh

The goal is to use one SSH connection to open a second one inside in the opposite direction.

This make computers reachable from outside their network.

This require one computer that can be reached by them.


## Example

### Situation

There is two computer: "A" and "B".

Computer "A" has a public ip address and can be reached from the Internet.

Computer "B" can reach the computer "A".

Computer "A" can not reach computer "B" by any mean.

### Steps

Computer "B" will open a SSH connection to the computer "A".

Inside this connection, computer "B" will redirect its own SSH port to one of the computer "A" port (exposing it publicly or keeping it internal).

Computer "A" is now able to connect to computer "B" through this _tunnel_.

## Commands

### Computer "A"

```shell

# Make sure ssh server is active
systemctl enable --now sshd

# Now go to computer "B" commands

# Paste the content of the computer "B" public key here
$EDITOR .ssh/authorized_keys

# Now go back to computer "B" commands

# Try to connect to computer "B" using the tunnel, this will ask the password of computer "B" user
ssh -p 30000 my_user_B@localhost
```

### Computer "B"

```shell

# Make sure ssh server is active
systemctl enable --now sshd

# Generate a SSH key
ssh-keygen

# Copy the public key to computer "A"
cat .ssh/id_rsa.pub

# Now go to computer "A" commands

# Try to connect to computer "A", this should not ask for any password as the key will be used
ssh my_user@my_domain.com

# Put autossh.service here
mkdir -p .config/systemd/user
$EDITOR .config/systemd/user/autossh.service

# Enable and start this service
systemctl --user enable --now autossh

# Verify the status of the service
systemctl --user status autossh

# Now go back to Computer "A" commands
```
