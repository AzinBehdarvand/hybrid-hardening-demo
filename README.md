# Hybrid Hardening Demo

## Overview
This repository provides a demonstration of scripted hardening and automated patching for a hybrid environment:
- A Linux VM (RedHat-like distribution running on Google Cloud Platform)  
- A Windows Server instance (accessible via RDP)  

The purpose is to showcase how repeatable scripts improve configuration consistency, reduce manual errors, and enforce security baselines in both Linux and Windows systems.

## Repository Structure
```
hybrid-hardening-demo/
├── linux/
│   ├── hardening.sh          # Linux hardening script
│   ├── patching.sh           # Linux patching script
├── windows/
│   ├── hardening.ps1         # Windows hardening script
│   ├── patching.ps1          # Windows patching script
├── docs/
│   ├── demo-steps.md         # Runbook for demo
│   └── images/               # Screenshots and diagrams
└── README.md
```

## Usage

### Linux VM (on GCP)
1. Create a lightweight VM (e2-micro or f1-micro) with AlmaLinux / Rocky / CentOS Stream.  
2. Copy the scripts to the VM.  
3. Run the hardening script:
   ```bash
   sudo ./linux/hardening.sh
   ```
4. Run the patching script:
   ```bash
   sudo ./linux/patching.sh
   ```
5. Verify key changes:
   ```bash
   sudo sshd -T | grep permitrootlogin
   sudo firewall-cmd --state
   ```

### Windows Server
1. Copy the PowerShell scripts to the server.  
2. Run in PowerShell as Administrator:
   ```powershell
   .\hardening.ps1
   .\patching.ps1
   ```
3. Use Task Scheduler to run `patching.ps1` daily for automated updates.

## Automation Evidence
- **Linux**: Configure a systemd timer or cron job to run `patching.sh` daily.  
- **Windows**: Configure Task Scheduler to run `patching.ps1` daily.  
- Logs can be collected for auditing and verification.

## Recommended Screenshots
Save all images under `docs/images/`:
- `architecture-diagram.png` – Diagram of Linux VM, Windows Server, and GitHub repo.  
- `linux-sshd-before-after.png` – Output showing SSH configuration changes.  
- `firewall-state.png` – Firewall state after applying hardening.  
- `systemd-timer.png` – Timer showing scheduled patching.  
- `windows-task-scheduler.png` – Task Scheduler configuration.  
- `windows-update-history.png` – Recent updates shown in PowerShell (`Get-HotFix`).  
- `patch-log.png` – Linux patch log output.  
- `repo-file-tree.png` – GitHub repository file tree.

## Safety Notes
- Always ensure you have valid SSH key access before disabling password authentication on Linux.  
- Patching may require reboots. Run during maintenance windows.  
- Scripts are intended as proof-of-concept, not full production hardening. For enterprise use, adapt to CIS or STIG standards.

## Future Enhancements
- Migrate scripts into Ansible or Puppet for idempotent configuration management.  
- Add CI checks (linting and syntax validation) using GitHub Actions.  
- Forward logs to centralized monitoring (ELK, Prometheus, Grafana).  
- Implement remediation workflows for failed updates.

## Author
Azin Behdarvand  
Repository: [hybrid-hardening-demo](https://github.com/username/hybrid-hardening-demo)
