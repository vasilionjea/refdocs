# Secure Shell (SSH)

Secure Shell is a cryptographic protocol, but also a program (_or a set of utilities_) that allows two computers to exchange encrypted messages over an unsecured network (_for example the internet_). This protocol is used to do many things: 

- encrypt a connection
- authenticate a host
- share files
- access a remote terminal
- SSH tunneling

## Connect to remote host

Connect as `<user>` to remote `<host>` on specified `<port>`. If you do not specify a username it uses your current username.

    ssh -p <port> <user>@<host>

## SSH Config

Location of SSH settings and host configuration

    ~/.ssh

If an SSH config file (or the ssh directory) doesn't exist create it at:

    ~/.ssh/config

Add details for a host entry

    Host <nickname>
      HostName <server>
      User <username>
      Port <port>

Use SSH configuration

    ssh <nickname>

# SSH Access

Connect to remote machine using SSH public / private key instead of username and password.

On the remote machine, if there is no existing `~/.ssh` directory or `~/.ssh/authorized_keys` file, create one:

    mkdir ~/.ssh && cd ~/.ssh
    nano authorized_keys

Then past your public key into the file and save.

If an `authorized_keys` file exists, append your key to that file on a new line.
