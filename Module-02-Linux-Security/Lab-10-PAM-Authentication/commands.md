# Lab 10 — PAM Authentication Command Reference

## Inspect PAM authentication policy

```bash
sudo awk 'NF && $1 !~ /^#/ {printf "%3d  %s\n", NR, $0}' /etc/pam.d/common-auth
```

## Inspect PAM account policy

```bash
sudo awk 'NF && $1 !~ /^#/ {printf "%3d  %s\n", NR, $0}' /etc/pam.d/common-account
```

## Verify pamtester

```bash
command -v pamtester && dpkg-query -W -f='${Status} ${Version}\n' pamtester
```

## Create the controlled user

```bash
sudo useradd -m -s /bin/bash lab10-pam-user && getent passwd lab10-pam-user
```

## Verify password status

```bash
sudo passwd -S lab10-pam-user
```

## Create the deliberately vulnerable isolated PAM service

```bash
printf '%s\n' 'auth sufficient pam_permit.so' 'auth required pam_unix.so' 'account required pam_permit.so' | sudo tee /etc/pam.d/lab10-auth >/dev/null
```

## Test the vulnerable authentication path

```bash
sudo pamtester lab10-auth lab10-pam-user authenticate
```

## Replace the vulnerable policy with required Unix authentication

```bash
printf '%s\n' 'auth required pam_unix.so' 'account required pam_unix.so' | sudo tee /etc/pam.d/lab10-auth >/dev/null
```

## Test valid credentials

```bash
sudo pamtester lab10-auth lab10-pam-user authenticate
```

## Test invalid credentials

```bash
sudo pamtester lab10-auth lab10-pam-user authenticate
```

## Test PAM account management

```bash
sudo pamtester lab10-auth lab10-pam-user acct_mgmt
```

## Remove the isolated lab objects

```bash
sudo rm -f /etc/pam.d/lab10-auth && sudo userdel -r lab10-pam-user
```

## Verify evidence checksums

```bash
cd "$HOME/cloud-security-labs/lab-10-pam-authentication/evidence" && sha256sum -c evidence-checksums.sha256
```
