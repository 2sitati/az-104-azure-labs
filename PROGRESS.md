# Progress Tracker

Tracking weekly AZ-104 hands-on labs from start to finish.

**Status legend:** ✅ Done · 🔄 In Progress · ⏳ Planned

| Week | Topic | Status | Key Concepts | LinkedIn | Folder |
|------|-------|--------|--------------|----------|--------|
| 01 | Resource Management | ✅ Done | Resource Groups, Tags, IAM, Resource Locks, Activity Log | [post](#) | [link](week-01-resource-management/) |
| 02 | Virtual Machines | ✅ Done | VM sizing, Windows Server, RDP, OS disks, Networking, NSG, Monitoring, Activity Log, Cost management | [post](#) | [link](week-02-virtual-machines/) |
| 03 | *TBD* | ⏳ Planned | | | |

## Week 2 Notes
- **Challenge:** RDP could not establish a working session on the initial Standard_B1s size (1 GiB RAM insufficient for a full Windows Server desktop session).
- **Resolution:** Resized the VM to Standard_B1ms (2 GiB RAM), which resolved the RDP issue — confirmed via a successful session and in-VM validation (`hostname`, `ipconfig`, `systeminfo`, test file creation).
- **Cleanup status:** VM stopped, deallocated, and deleted, and the Resource Group `AZ104-Week02-VirtualMachines` was also deleted — cleanup fully completed.

## Notes

- Update the table each time a lab is started and completed.
- Replace `[post](#)` with the actual LinkedIn post URL once published.
- Keep "Key Concepts" short (3–5 items) — this doubles as a quick skills index for anyone skimming the repo.
- All lab resources are deleted after each week; this repo documents the work, not live infrastructure.
