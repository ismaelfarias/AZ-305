# Lab 01: Deploy and Configure a Virtual Machine

## 🎯 Objectives

In this lab, you will:
- Create a resource group
- Deploy a Windows/Linux Virtual Machine
- Configure Network Security Group (NSG)
- Connect to the VM
- Implement basic monitoring

## ⏱️ Estimated Time

45-60 minutes

## 📋 Prerequisites

- Azure subscription (Free tier works)
- Basic understanding of VMs and networking
- RDP client (for Windows) or SSH client (for Linux)

## 🏗️ Architecture

```
Internet
   ↓
Network Security Group
   ↓
Virtual Machine
   ├─ OS Disk (Managed)
   ├─ Network Interface
   └─ Public IP (optional)
```

## 📝 Instructions

### Task 1: Create a Resource Group

#### Using Azure Portal

1. Sign in to the [Azure Portal](https://portal.azure.com)
2. Click **Resource groups** in the left menu
3. Click **+ Create**
4. Fill in the details:
   - **Subscription**: Select your subscription
   - **Resource group**: `rg-az305-lab01`
   - **Region**: `East US` (or your preferred region)
5. Click **Review + create**, then **Create**

#### Using Azure CLI

```bash
# Login to Azure
az login

# Create resource group
az group create \
  --name rg-az305-lab01 \
  --location eastus
```

#### Using PowerShell

```powershell
# Login to Azure
Connect-AzAccount

# Create resource group
New-AzResourceGroup `
  -Name rg-az305-lab01 `
  -Location eastus
```

### Task 2: Deploy a Virtual Machine

#### Using Azure Portal

1. In the Azure Portal, click **Virtual machines**
2. Click **+ Create** → **Azure virtual machine**
3. Fill in the **Basics** tab:
   - **Resource group**: `rg-az305-lab01`
   - **Virtual machine name**: `vm-lab01`
   - **Region**: Same as resource group
   - **Image**: `Ubuntu Server 20.04 LTS` or `Windows Server 2022`
   - **Size**: `Standard_B2s` (2 vCPUs, 4 GB RAM)
   - **Authentication type**: SSH public key (Linux) or Password (Windows)
   - **Username**: `azureadmin`
   - **Password/SSH key**: Create a secure password or generate SSH key

4. **Disks** tab:
   - **OS disk type**: `Standard SSD`
   - Leave other settings as default

5. **Networking** tab:
   - **Virtual network**: Create new or use default
   - **Subnet**: Use default
   - **Public IP**: Create new
   - **NIC network security group**: `Basic`
   - **Public inbound ports**: 
     - Linux: Allow SSH (22)
     - Windows: Allow RDP (3389)

6. **Management** tab:
   - Enable **Azure Monitoring** → Enable for basic monitoring
   - **Boot diagnostics**: Managed storage account

7. **Review + create** → **Create**

8. Wait for deployment to complete (3-5 minutes)

#### Using Azure CLI (Linux VM)

```bash
# Create a Linux VM
az vm create \
  --resource-group rg-az305-lab01 \
  --name vm-lab01 \
  --image UbuntuLTS \
  --size Standard_B2s \
  --admin-username azureadmin \
  --generate-ssh-keys \
  --public-ip-sku Standard

# Open port 22 for SSH
az vm open-port \
  --resource-group rg-az305-lab01 \
  --name vm-lab01 \
  --port 22
```

### Task 3: Configure Network Security Group

1. Navigate to your VM in the Azure Portal
2. Click **Networking** in the left menu
3. Review the **Inbound port rules**
4. Click **Add inbound port rule**
5. Configure a new rule:
   - **Source**: `IP Addresses`
   - **Source IP addresses**: Your public IP (find at https://whatismyip.com)
   - **Destination port ranges**: `22` (SSH) or `3389` (RDP)
   - **Protocol**: `TCP`
   - **Priority**: `300`
   - **Name**: `Allow-SSH-MyIP` or `Allow-RDP-MyIP`
6. Click **Add**

**Security Best Practice**: Always restrict access to specific IP addresses rather than allowing from anywhere (0.0.0.0/0).

### Task 4: Connect to the Virtual Machine

#### For Linux VM (SSH)

```bash
# Get the public IP address
az vm show \
  --resource-group rg-az305-lab01 \
  --name vm-lab01 \
  --show-details \
  --query publicIps \
  --output tsv

# Connect via SSH
ssh azureadmin@<PUBLIC_IP_ADDRESS>
```

#### For Windows VM (RDP)

1. In the Azure Portal, navigate to your VM
2. Click **Connect** → **RDP**
3. Download the RDP file
4. Open the RDP file
5. Enter your username and password
6. Click **Yes** to accept the certificate

### Task 5: Verify VM Configuration

Once connected to the VM:

#### Linux Commands
```bash
# Check OS version
cat /etc/os-release

# Check CPU and memory
lscpu
free -h

# Check disk space
df -h

# Check network configuration
ip addr show
```

#### Windows Commands (PowerShell)
```powershell
# Check OS version
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion

# Check CPU and memory
Get-ComputerInfo | Select-Object CsProcessors, CsTotalPhysicalMemory

# Check disk space
Get-Volume

# Check network configuration
Get-NetIPAddress
```

### Task 6: Configure Monitoring

1. In the Azure Portal, navigate to your VM
2. Click **Insights** in the left menu
3. Click **Enable** to enable VM Insights
4. Wait for monitoring to be configured (5-10 minutes)
5. Once enabled, explore:
   - Performance metrics (CPU, Memory, Disk, Network)
   - Map view (dependencies)
   - Logs

### Task 7: Test VM Restart and Monitoring

1. In the Azure Portal, navigate to your VM
2. Click **Overview**
3. Click **Restart**
4. Confirm the restart
5. Monitor the VM status and metrics during restart

## ✅ Validation

Verify that you have successfully:
- [ ] Created a resource group
- [ ] Deployed a Virtual Machine
- [ ] Configured NSG with appropriate rules
- [ ] Connected to the VM via SSH/RDP
- [ ] Verified VM configuration
- [ ] Enabled monitoring

## 📊 Review Questions

1. What is the difference between Standard and Premium SSD?
2. When should you use Availability Sets vs Availability Zones?
3. What is the purpose of a Network Security Group?
4. How does Azure Managed Disks improve reliability?
5. What monitoring metrics are most important for VMs?

## 🧹 Cleanup

To avoid charges, delete all resources:

#### Using Azure Portal
1. Navigate to **Resource groups**
2. Select `rg-az305-lab01`
3. Click **Delete resource group**
4. Type the resource group name to confirm
5. Click **Delete**

#### Using Azure CLI
```bash
az group delete --name rg-az305-lab01 --yes --no-wait
```

#### Using PowerShell
```powershell
Remove-AzResourceGroup -Name rg-az305-lab01 -Force
```

## 💡 Key Takeaways

1. **Resource Groups** organize related Azure resources
2. **VM Sizes** should be chosen based on workload requirements
3. **Network Security Groups** act as a firewall for VMs
4. **Managed Disks** provide better reliability and management
5. **Azure Monitor** provides insights into VM performance
6. **Always clean up** lab resources to avoid unnecessary costs

## 🔗 Additional Resources

- [Virtual Machines Documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/)
- [VM Sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes)
- [Network Security Groups](https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)
- [Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/)

## 📈 Next Steps

- Lab 02: Implement VM High Availability with Availability Sets
- Lab 03: Configure VM Auto-scaling with Scale Sets
- Lab 04: Implement Azure Backup for VMs
