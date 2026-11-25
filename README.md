# How I Set Up My Own DetectionLab

A practical SOC analyst homelab I built and documented end to end, using Windows 11, VirtualBox, Vagrant, Git, Active Directory, Windows Event Forwarding (WEF), a Splunk Logger, and a Windows 10 endpoint.

The full 35-page guide — including every command, every error I hit and how I fixed it, and 94 evidence screenshots — is in [DetectionLab_Complete_Setup_Guide.pdf](DetectionLab_Complete_Setup_Guide.pdf).

## Project objective

Build a local DetectionLab environment that behaves like a mini enterprise network for SOC analyst practice: Windows logging, Active Directory, event forwarding, SIEM/log analysis, and incident investigation in a safe environment on a personal computer.

## Host specification

| Item | Value |
| --- | --- |
| Host OS | Windows 11 |
| RAM | 16 GB |
| Free storage during setup | ~155 GB |
| Virtualization provider | Oracle VirtualBox |
| Automation tool | Vagrant |
| Repository tool | Git |
| Final domain | windomain.local |

## Lab architecture

`vagrant up` builds four virtual machines inside VirtualBox:

- **logger** — Ubuntu server running Splunk
- **dc** — Active Directory domain controller
- **wef** — Windows Event Forwarding server
- **win10** — Windows 10 workstation joined to the domain

## Setup journey (15 steps)

1. Install VirtualBox, Vagrant and Git on the Windows host
2. Open Administrator PowerShell
3. Verify VirtualBox is installed (VBoxManage was not in PATH — located it manually in `C:\Program Files\Oracle\VirtualBox`)
4. Clone the DetectionLab repository to `C:\DetectionLab`
5. Install the missing `vagrant-reload` plugin after a "reload provisioner could not be found" error
6. Start the VMs one by one (staged approach because of 16 GB RAM)
7. Build the Logger (Splunk provisioning completed successfully)
8. Build the Domain Controller (fixed osquery/Wireshark provisioning issues by re-running `vagrant provision dc`)
9. Build the WEF server (worked through a .NET troubleshooting loop)
10. Build the Windows 10 endpoint (initially showed WORKGROUP — `vagrant provision win10` completed the domain join)
11. Verify all VMs are running with `vagrant status`
12. Verify domain membership on DC and Win10
13. Verify domain authentication (`windomain\vagrant`)
14. Verify Windows security logs (Event ID 4624 on win10.windomain.local)
15. Document everything and update my portfolio

## Final validation results

- logger, dc.windomain.local, wef.windomain.local and win10.windomain.local all running
- Domain membership and domain authentication confirmed
- Security event logs flowing (Event ID 4624 verified)

## What I practised next

- PowerShell logging and suspicious command detection
- Windows Event Forwarding validation
- Threat hunting and incident response exercises

## Evidence

The appendix of the PDF contains contact sheets with 94 actual screenshots captured during the setup.

---

Built and documented by [Pelumi Lekan Ayeloja](https://www.linkedin.com/in/pelumi-ayeloja-a08247240). More of my practice work: [home-lab-setup](https://github.com/pelumilekanayeloja/home-lab-setup).
