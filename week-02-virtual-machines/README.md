# AZ-104 Week 2 — Azure Virtual Machines

## Overview
This week's lab focused on deploying and managing an Azure Virtual Machine — provisioning a Windows Server VM, connecting to it, examining its compute, storage, and networking components, and practicing basic validation, monitoring, and cost-conscious sizing decisions.

## Objectives
- Deploy a Windows Server VM into a dedicated Resource Group.
- Connect to the VM via Remote Desktop (RDP) and verify the operating system was functioning.
- Examine the VM's OS disk, networking components, and Network Security Group.
- Use VM monitoring and the Activity Log to observe management operations.
- Make a deliberate, cost-conscious sizing decision appropriate for a temporary lab in a shared environment.

## Scenario
As the second lab in this AZ-104 hands-on series, this exercise simulates a common early administrator task: standing up a VM for a specific purpose, understanding what's deployed around it (disks, networking, NSG), confirming it works as expected, and keeping cost and environment hygiene in mind throughout — since this lab ran inside a shared Azure environment.

## Azure Services and Concepts
- Resource Groups
- Azure Virtual Machines
- VM sizing (Standard_B1s vs. Standard_D2s_v3)
- OS Disks
- Virtual Network, Subnet, Network Interface, Public IP (auto-created by VM deployment)
- Network Security Groups (NSG)
- Remote Desktop Protocol (RDP)
- VM Metrics / Monitoring
- Azure Activity Log
- Cost management

## Architecture
![Architecture Diagram](architecture-diagram.png)

The diagram shows only the resources actually deployed for this lab. Management and operational activities (RDP access, VM Metrics, Activity Log) are shown separately since they are not deployed resources themselves.

## Implementation

### 1. Resource Group
I created a dedicated Resource Group, `AZ104-Week02-VirtualMachines`, to contain all resources for this lab and keep it isolated within the shared Azure environment.

### 2. Virtual Machine Deployment
I deployed a Windows Server VM named `AZ104-Week02-VM` using the **Windows Server 2025 Datacenter: Azure Edition** image, with username/password authentication and RDP enabled for lab access.

*[Screenshot: VM Overview — placeholder, pending upload]*

### 3. VM Configuration
The VM was initially shown at **Standard_D2s_v3** (~$0.73/hour). Since this was a temporary learning lab, I deliberately resized it to **Standard_B1s** (~$0.104/hour, 1 vCPU / 1 GiB RAM) to keep compute cost down, with a **Standard HDD, 127 GiB** OS disk and no additional data disks.

![VM Size and Source Image](screenshots/03-vm-size.jpeg)
*VM size and source image details at initial configuration.*

### 4. Remote Desktop Access
I connected to the VM using the Azure Portal's native RDP option (Virtual Machine → Connect → RDP) to verify the operating system was reachable and functioning. After resolving the sizing issue described in **Troubleshooting** below, I was able to establish a working RDP session.

![RDP Session Connected](screenshots/04-rdp-connection.jpeg)
*Successful RDP session into the Windows Server desktop.*

### 5. VM Validation
Inside the VM, I used Command Prompt to run `hostname`, `ipconfig`, and `systeminfo` to confirm the OS was responding correctly. I also created a test folder (`C:\AZ104-Lab`) and test file (`C:\AZ104-Lab\week02-test.txt`) to confirm I could interact with local storage.

![Command Prompt Validation](screenshots/05-vm-validation.png)
*`hostname`, `ipconfig`, and `systeminfo` output confirming the VM was reachable and correctly configured.*

![Test File Created](screenshots/06-test-file.jpeg)
*Test folder and file created inside the VM to confirm local storage access.*

### 6. Disk Exploration
I reviewed the VM's Disks blade and confirmed the OS disk configuration: Standard HDD, 127 GiB, with no additional data disks attached.

![VM Disks](screenshots/07-vm-disks.jpeg)
*OS disk configuration — Standard HDD, 127 GiB.*

### 7. Networking Exploration
I reviewed the networking resources Azure automatically created alongside the VM — Network Interface, Virtual Network, Subnet, and Public IP — to understand how a VM connects into Azure networking. (A dedicated networking lab will cover this area in more depth in a future week.)

![VM Networking](screenshots/08-vm-networking.png)
*Network interface, virtual network/subnet, and public IP associated with the VM.*

### 8. Network Security Group
I reviewed the NSG associated with the VM and confirmed the relevant inbound rule: **RDP / TCP 3389, allowed from any source**. This was to understand how RDP access is controlled at the network level, not to design a custom NSG configuration.

![Network Security Group Rules](screenshots/09-network-security-group.png)
*NSG inbound rules, including the RDP (TCP 3389) allow rule.*

### 9. Monitoring and Activity Log
I explored the VM's available metrics (including CPU and availability) to see the performance visibility Azure provides, and reviewed the Activity Log to see how management operations against the VM are recorded.

![VM Metrics](screenshots/11-vm-metrics.jpeg)
*CPU utilization and availability metrics for the VM.*

![Activity Log](screenshots/10-activity-log.png)
*Activity Log showing management operations performed against the VM, including the resize and later deallocation/deletion.*

### 10. Cost Management and Cleanup
To manage cost in a shared environment, I avoided unnecessary add-ons (extra data disks, backup, load balancer, Bastion, availability infrastructure, extra monitoring services) and used a small B-series VM. At the end of the lab, the VM was stopped/deallocated.

![VM Deallocated](screenshots/12-vm-deallocated.png)
*VM shown in a Stopped (deallocated) state at the end of the lab.*

## Troubleshooting
While connecting to the VM via RDP on the initial **Standard_B1s** size (1 vCPU / 1 GiB RAM), the RDP session would not load correctly. A full Windows Server desktop session is too heavy a workload for 1 GiB of RAM to support reliably.

**Resolution:** I resized the VM from **Standard_B1s → Standard_B1ms** (1 vCPU / 2 GiB RAM). This gave the VM enough memory to support an interactive RDP session, and the subsequent connection succeeded, confirmed by the validation steps above.

## Key Concepts Learned
- How VM size directly affects both cost and the VM's ability to handle workloads like a full Windows Server desktop session — confirmed firsthand when 1 GiB of RAM (B1s) couldn't reliably support an RDP session, and 2 GiB (B1ms) resolved it.
- How an OS disk is provisioned and configured as part of VM deployment.
- How Azure automatically provisions supporting networking resources (NIC, VNet, Subnet, Public IP) alongside a VM.
- How an NSG controls inbound access (in this case, RDP over TCP 3389).
- How VM Metrics and the Activity Log provide two different kinds of visibility — performance data vs. an audit trail of management operations.
- The importance of matching VM size to workload, and of cost-conscious decisions in a shared/temporary environment.

## Validation
- Confirmed the VM appeared correctly inside its dedicated Resource Group.
- Confirmed the OS disk configuration (Standard HDD, 127 GiB) via the Disks blade.
- Confirmed supporting networking resources were created and associated with the VM.
- Confirmed the NSG's inbound RDP rule.
- Ran validation commands (`hostname`, `ipconfig`, `systeminfo`) and created a test file inside the VM to confirm the OS was functioning and locally accessible.

## Cleanup
The VM was stopped/deallocated at the end of the lab, and the Activity Log confirms the VM was subsequently deleted. The dedicated Resource Group, `AZ104-Week02-VirtualMachines`, was intended to be removed as well to avoid ongoing charges in the shared environment. *(Resource Group deletion pending confirmation.)*

## Key Takeaways
- VM sizing is a real trade-off between cost and workload suitability, not just a cost line item — this became concrete when the B1s size affected RDP usability.
- Azure creates several supporting resources automatically during VM deployment, which is useful to understand for both cost and security review.
- The Activity Log and VM Metrics together give a fuller picture of a VM's operational history than either alone.
- Working in a shared environment reinforces good habits: appropriately sized resources, minimal unnecessary add-ons, and prompt cleanup.

---
*This lab was completed as part of a self-directed, hands-on AZ-104 study portfolio. It reflects a learning environment, not production experience.*
