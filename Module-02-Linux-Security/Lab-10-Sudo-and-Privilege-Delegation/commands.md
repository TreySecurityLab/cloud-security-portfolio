# Commands Used

```bash
id
sudo -l
sudo useradd -M -s /usr/sbin/nologin lab10-operator
getent passwd lab10-operator
printf '%s\n' 'Identity: lab10-operator' 'Role: SSH service status operator' 'Required privilege: View SSH service status only' 'Approved command: /usr/bin/systemctl status ssh' 'Unrestricted root commands: NOT AUTHORIZED' > ~/cloud-security-labs/lab-10-sudo-and-privilege-delegation/scenario/authorization-requirement.txt
sudo chmod 440 /etc/sudoers.d/lab10-operator
sudo visudo -cf /etc/sudoers.d/lab10-operator
sudo -l -U lab10-operator
sudo -u lab10-operator sudo -n /usr/bin/id
sudo -u lab10-operator sudo -n /usr/bin/systemctl status ssh
sha256sum -c evidence-checksums.sha256
test -s evidence-checksums.sha256 && echo 'evidence-checksums.sha256: EXISTS AND NON-EMPTY'
```
