# Security Baseline — Selected Commands

This concise reference contains the commands that best represent the Lab 01 baseline. Run Ubuntu commands on `ubuntu-server` and Kali commands on `kali-attacker`. Every terminal command is displayed on one line.

## Confirm the Ubuntu System

Displays the configured hostname:

```bash
hostname
```

Displays the user associated with the current shell:

```bash
whoami
```

Displays the server's IP-address configuration:

```bash
ip address show
```

## Review Local Exposure and Controls

Displays listening TCP and UDP sockets with owning-process information:

```bash
sudo ss -tulnp
```

Reviews Apache service state without opening a pager:

```bash
sudo systemctl status apache2 --no-pager
```

Reviews SSH service state without opening a pager:

```bash
sudo systemctl status ssh --no-pager
```

Reviews the UFW firewall status and configured rules:

```bash
sudo ufw status verbose
```

Searches the Ubuntu authentication log for accepted, failed, and invalid-user events:

```bash
sudo grep -Ei "accepted|failed|invalid" /var/log/auth.log
```

## Validate Remote Exposure from Kali

Tests basic reachability to the Ubuntu Server:

```bash
ping -c 4 192.168.50.129
```

Performs service-version detection against the Ubuntu Server:

```bash
nmap -sV 192.168.50.129
```

Scans the expected SSH and HTTP ports:

```bash
nmap -p 22,80 192.168.50.129
```

Saves a human-readable service scan:

```bash
nmap -sV 192.168.50.129 -oN nmap-service-scan.txt
```

## Verify Evidence

Verifies the final evidence checksum manifest:

```bash
cd evidence && sha256sum -c evidence-checksums.sha256
```
