# Linux SSH Cheat Sheet

## Basic SSH Commands

### Connecting to a Remote Server
```bash
ssh username@hostname_or_ip
ssh username@hostname_or_ip -p port_number  # Connect to specific port (default: 22)
ssh -i /path/to/private_key username@hostname_or_ip  # Connect using identity file
```

### Authentication Options
```bash
ssh-keygen -t rsa -b 4096  # Generate SSH key pair (RSA, 4096-bit)
ssh-keygen -t ed25519     # Generate SSH key pair (Ed25519, more secure)
ssh-copy-id username@hostname_or_ip  # Copy public key to remote server
```

### File Transfer with SCP
```bash
scp /local/file username@hostname_or_ip:/remote/path  # Upload file
scp username@hostname_or_ip:/remote/file /local/path  # Download file
scp -r /local/folder username@hostname_or_ip:/remote/path  # Upload folder
scp -P port_number username@hostname_or_ip:/remote/file /local/path  # Specify port
```

### File Transfer with SFTP
```bash
sftp username@hostname_or_ip  # Start SFTP session
get remote_file [local_file]  # Download file
put local_file [remote_file]  # Upload file
ls, cd, pwd, mkdir, rm       # Navigate remote filesystem
lls, lcd, lpwd               # Navigate local filesystem
exit                         # Close SFTP session
```

## SSH Configuration

### SSH Config File (~/.ssh/config)
```
# Default settings for all hosts
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 5

# Specific host settings
Host myserver
    HostName 192.168.1.100
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_rsa_server

# Jump host configuration
Host private-server
    HostName 10.0.0.5
    User admin
    ProxyJump jumphost
```

### Common SSH Client Options
```bash
ssh -v username@hostname_or_ip  # Verbose mode (debugging)
ssh -vv or -vvv                 # Increase verbosity
ssh -X username@hostname_or_ip  # Enable X11 forwarding
ssh -Y username@hostname_or_ip  # Enable trusted X11 forwarding
ssh -C username@hostname_or_ip  # Enable compression
ssh -A username@hostname_or_ip  # Enable agent forwarding
ssh -J jump_host username@destination  # Use jump host
```

## SSH Tunneling

### Local Port Forwarding
```bash
ssh -L local_port:remote_host:remote_port username@ssh_server
# Example: Access remote MySQL server locally
ssh -L 3306:database_server:3306 username@ssh_server
```

### Remote Port Forwarding
```bash
ssh -R remote_port:local_host:local_port username@ssh_server
# Example: Expose local web server to remote server
ssh -R 8080:localhost:80 username@ssh_server
```

### Dynamic Port Forwarding (SOCKS Proxy)
```bash
ssh -D local_port username@ssh_server
# Example: Create SOCKS proxy on port 1080
ssh -D 1080 username@ssh_server
```

## SSH Security

### Securing SSH Server (in /etc/ssh/sshd_config)
```
# Disable password authentication
PasswordAuthentication no

# Use specific SSH protocol version
Protocol 2

# Restrict root login
PermitRootLogin no

# Specify allowed users
AllowUsers user1 user2

# Change default port
Port 2222

# Limit authentication attempts
MaxAuthTries 3

# Enable key-based authentication
PubkeyAuthentication yes
```

### After editing sshd_config, restart the SSH service
```bash
sudo systemctl restart sshd  # For systemd
sudo service ssh restart     # For init.d
```

## Advanced SSH Usage

### Run Commands Without Login Shell
```bash
ssh username@hostname_or_ip "command"
# Example: Check disk space
ssh username@hostname_or_ip "df -h"
```

### SSH Agent
```bash
eval $(ssh-agent)            # Start SSH agent
ssh-add ~/.ssh/id_rsa        # Add key to agent
ssh-add -l                   # List keys in agent
ssh-add -D                   # Remove all keys
```

### SSH Escape Characters
```
~.  # Terminate connection
~B  # Send BREAK to the remote system
~C  # Open command line
~R  # Request rekey
~V  # Decrease verbosity
~v  # Increase verbosity
~?  # Display a list of escape characters
```

### SSH Multiplexing
```
# In ~/.ssh/config
Host *
    ControlMaster auto
    ControlPath ~/.ssh/control/%r@%h:%p
    ControlPersist 10m
```

### Generate SSH Key Without Passphrase (Use with Caution)
```bash
ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/automation_key
```

## Troubleshooting

### Check SSH Server Status
```bash
sudo systemctl status sshd  # For systemd
sudo service ssh status     # For init.d
```

### Debug Connection Issues
```bash
ssh -vvv username@hostname_or_ip  # Very verbose output
# Check permissions on ~/.ssh directory
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 644 ~/.ssh/config
```

### Common SSH Errors

- **Permission denied (publickey)**: Key authentication failed
- **Connection refused**: SSH server not running or firewall blocking
- **Host key verification failed**: Server's host key changed or man-in-the-middle attack

### Force Using Password Authentication
```bash
ssh -o PubkeyAuthentication=no username@hostname_or_ip
```
