# Practical Labs

This folder contains hands-on lab exercises for AZ-305 exam preparation.

## 🧪 Lab Categories

### Identity & Access Management
- Configure Azure AD authentication
- Implement RBAC assignments
- Set up Managed Identities
- Configure Conditional Access policies

### Governance
- Create Azure Policies
- Deploy Azure Blueprints
- Configure Management Groups
- Implement resource tagging strategies

### Compute Solutions
- Deploy and configure Virtual Machines
- Create App Services with deployment slots
- Deploy applications to AKS
- Implement Azure Functions

### Networking
- Create Hub-Spoke network topology
- Configure VPN Gateway
- Implement Application Gateway with WAF
- Set up Azure Firewall

### Data Storage
- Configure Azure SQL Database with geo-replication
- Implement Cosmos DB multi-region writes
- Set up Azure Storage with lifecycle policies
- Design data lake architecture

### Business Continuity
- Configure Azure Backup
- Implement Azure Site Recovery
- Test disaster recovery scenarios
- Design high availability solutions

### Monitoring
- Configure Azure Monitor
- Set up Log Analytics workspace
- Implement Application Insights
- Create custom alerts and dashboards

## 📋 Lab Format

Each lab includes:
1. **Objectives**: What you'll learn
2. **Prerequisites**: Required resources and knowledge
3. **Estimated Time**: Time to complete
4. **Instructions**: Step-by-step guide
5. **Validation**: How to verify success
6. **Cleanup**: Steps to remove resources

## 🚀 Getting Started

### Prerequisites
- Azure subscription (Free tier available)
- Basic understanding of Azure services
- Azure CLI or PowerShell installed (optional)
- Visual Studio Code with Azure extensions (optional)

### Azure Free Account
If you don't have an Azure subscription:
1. Visit [Azure Free Account](https://azure.microsoft.com/en-us/free/)
2. Get $200 credit for 30 days
3. Access to free services for 12 months

### Cost Management
- Use the Azure Pricing Calculator before deploying
- Set up budget alerts
- Delete resources after completing labs
- Use Dev/Test pricing when available
- Leverage Azure Sandbox for free practice

## 📁 Lab Organization

```
Labs/
├── 01-identity-governance/
│   ├── lab-01-azure-ad-setup.md
│   ├── lab-02-rbac-configuration.md
│   └── lab-03-managed-identities.md
├── 02-compute/
│   ├── lab-01-vm-deployment.md
│   ├── lab-02-app-service.md
│   └── lab-03-aks-cluster.md
├── 03-networking/
│   ├── lab-01-vnet-peering.md
│   ├── lab-02-vpn-gateway.md
│   └── lab-03-application-gateway.md
├── 04-storage/
│   ├── lab-01-sql-database.md
│   ├── lab-02-cosmos-db.md
│   └── lab-03-storage-accounts.md
└── 05-monitoring/
    ├── lab-01-azure-monitor.md
    └── lab-02-application-insights.md
```

## 🎯 Recommended Lab Path

### Beginner Path (Weeks 1-2)
1. Azure Portal navigation
2. Create resource groups
3. Deploy a simple VM
4. Configure basic networking
5. Set up storage accounts

### Intermediate Path (Weeks 3-4)
1. Implement RBAC
2. Deploy multi-tier applications
3. Configure VNet peering
4. Set up Azure SQL Database
5. Implement monitoring

### Advanced Path (Weeks 5-6)
1. Design hub-spoke topology
2. Implement disaster recovery
3. Configure AKS clusters
4. Design data pipelines
5. Implement governance at scale

## 💻 Lab Environments

### Azure Portal
- Web-based management interface
- Best for learning and visualization
- No local tools required

### Azure CLI
```bash
# Install Azure CLI
# Windows: Download from Microsoft
# Linux: curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
# macOS: brew install azure-cli

# Login to Azure
az login

# List subscriptions
az account list --output table
```

### Azure PowerShell
```powershell
# Install Azure PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser

# Login to Azure
Connect-AzAccount

# List subscriptions
Get-AzSubscription
```

### ARM Templates & Bicep
- Infrastructure as Code
- Repeatable deployments
- Version control integration

## 📝 Lab Tracking

Keep track of completed labs:
- [ ] Identity and Access Management labs
- [ ] Governance labs
- [ ] Compute solution labs
- [ ] Networking labs
- [ ] Data storage labs
- [ ] Business continuity labs
- [ ] Monitoring labs

## 🔒 Security Best Practices for Labs

1. **Never commit credentials to repositories**
2. Use Azure Key Vault for secrets
3. Implement least privilege access
4. Enable MFA on your account
5. Clean up resources after labs
6. Use separate subscriptions for testing
7. Review and understand cost implications

## 🧹 Cleanup After Labs

Always clean up resources to avoid charges:

```bash
# Delete resource group (deletes all resources within)
az group delete --name MyResourceGroup --yes --no-wait

# Or via Portal
# Navigate to Resource Group → Delete resource group
```

## 💡 Tips for Effective Practice

1. **Read First**: Review lab objectives before starting
2. **Understand Why**: Don't just follow steps, understand the reasoning
3. **Experiment**: Try variations and what-if scenarios
4. **Document**: Take notes on what works and what doesn't
5. **Break Things**: Learn from failures and troubleshooting
6. **Time Yourself**: Practice under time constraints
7. **Repeat**: Do important labs multiple times

## 📚 Additional Practice Resources

- [Microsoft Learn Sandbox](https://learn.microsoft.com/en-us/training/)
- [Azure Quickstart Templates](https://azure.microsoft.com/en-us/resources/templates/)
- [GitHub - Azure Samples](https://github.com/Azure-Samples)
- [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)

## 🎓 Lab Scenarios for Exam Preparation

Focus on scenarios that test:
1. Choosing the right service for requirements
2. Designing for scalability and resilience
3. Implementing security and compliance
4. Optimizing for cost
5. Integrating multiple Azure services
6. Troubleshooting common issues

---

**Important**: Always delete resources after completing labs to avoid unexpected charges. Set up billing alerts to monitor spending.
