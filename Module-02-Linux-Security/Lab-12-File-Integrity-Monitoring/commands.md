# Commands Used

```bash
sudo apt-get update
sudo apt-get install -y aide
aide --version
find ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/monitored -maxdepth 1 -type f -printf '%f mode=%m owner=%u:%g size=%s-bytes\n' | sort
aide --config=/home/testlab/cloud-security-labs/lab-11-aide-file-integrity-monitoring/aide/aide.conf --config-check
aide --config=/home/testlab/cloud-security-labs/lab-11-aide-file-integrity-monitoring/aide/aide.conf --init
mv ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/aide/aide.db.new ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/aide/aide.db
aide --config=/home/testlab/cloud-security-labs/lab-11-aide-file-integrity-monitoring/aide/aide.conf --check
aide --config=/home/testlab/cloud-security-labs/lab-11-aide-file-integrity-monitoring/aide/aide.conf --check 2>&1 | tee ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/evidence/aide-change-detection.txt; printf 'AIDE exit status: %s\n' "${PIPESTATUS[0]}" | tee ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/evidence/aide-change-exit-status.txt
cd ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/evidence && sha256sum -c evidence-checksums.sha256
cd ~/cloud-security-labs/lab-11-aide-file-integrity-monitoring/evidence && test -s evidence-checksums.sha256 && echo 'evidence-checksums.sha256: EXISTS AND NON-EMPTY'
```
