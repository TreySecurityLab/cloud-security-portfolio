# Security Baseline Commands

## Ubuntu Server

### Display the hostname

```bash
hostname
```

- `hostname` — Displays the system's configured hostname.

### Display the current user

```bash
whoami
```

- `whoami` — Displays the username associated with the current shell.

### Display IP addresses

```bash
ip address show
```

- `ip` — Linux networking utility.
- `address` — Selects IP address information.
- `show` — Displays the current configuration.

### Display listening TCP and UDP sockets

```bash
sudo ss -tulnp
```

- `sudo` — Runs the command with administrative privileges.
- `ss` — Displays socket information.
- `-t` — Shows TCP sockets.
- `-u` — Shows UDP sockets.
- `-l` — Shows listening sockets.
- `-n` — Shows numerical addresses and ports.
- `-p` — Shows the process using each socket.

### Review Apache

```bash
sudo systemctl status apache2 --no-pager
```

- `systemctl` — Manages and inspects systemd services.
- `status` — Shows the current service state.
- `apache2` — Ubuntu's Apache service.
- `--no-pager` — Prints output directly.

### Review SSH

```bash
sudo systemctl status ssh --no-pager
```

- `ssh` — Ubuntu's OpenSSH service.

### Review UFW

```bash
sudo ufw status verbose
```

- `ufw` — Ubuntu's Uncomplicated Firewall utility.
- `status` — Displays current status and rules.
- `verbose` — Shows additional details.

### Review authentication activity

```bash
sudo grep -Ei "accepted|failed|invalid" /var/log/auth.log
```

- `grep` — Searches text.
- `-E` — Enables extended regular expressions.
- `-i` — Ignores letter case.
- `accepted|failed|invalid` — Matches any listed pattern.
- `/var/log/auth.log` — Ubuntu authentication log.

## Kali Linux

### Test connectivity

```bash
ping -c 4 192.168.50.129
```

- `ping` — Sends ICMP Echo Requests.
- `-c 4` — Sends four packets.
- `192.168.50.129` — Ubuntu Server target.

### Perform a default scan

```bash
nmap 192.168.50.129
```

- `nmap` — Network Mapper.
- `192.168.50.129` — Ubuntu target address.

### Detect service versions

```bash
nmap -sV 192.168.50.129
```

- `-sV` — Probes open ports to identify services and versions.

### Scan expected ports

```bash
nmap -p 22,80 192.168.50.129
```

- `-p` — Specifies ports.
- `22,80` — SSH and HTTP ports.

### Save the Nmap scan

```bash
nmap -sV 192.168.50.129 -oN nmap-service-scan.txt
```

- `-oN` — Saves normal human-readable output.
- `nmap-service-scan.txt` — Destination file.
