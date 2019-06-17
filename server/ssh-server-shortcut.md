# SSH server shortcut

Edit file ~/.ssh/config
```
Host iot-bus-dev
	Hostname 192.168.1.100
	User root
	Port 4444
```

- Host: Any name you want, easy to remember (ex: project name)
- Hostname: IP server
- User: username to access server
- IdentityFile: SSH public key, default is `~/.ssh/id_rsa`
- Port: port ssh of server, default is 22
- ServerAliveInterval: server connect timeout
- ProxyCommand: special command when connect to server