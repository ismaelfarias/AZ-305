# Three-Tier Web Application Architecture

## Architecture Overview

This diagram represents a typical three-tier web application deployed on Azure with high availability and security.

## Components

### Presentation Tier (Web Tier)
- **Azure Application Gateway** with WAF
- **Web App Service** or **Virtual Machine Scale Set**
- Auto-scaling enabled
- Multiple instances across availability zones

### Application Tier (Business Logic)
- **App Service** or **Azure Kubernetes Service**
- API endpoints
- Business logic processing
- Connection to data tier

### Data Tier
- **Azure SQL Database** with geo-replication
- **Redis Cache** for session state
- **Azure Storage** for blob/file storage

## Architecture Diagram

```
                                Internet
                                   |
                                   ↓
                    [Azure Front Door / Traffic Manager]
                                   |
                                   ↓
                         [Application Gateway + WAF]
                                   |
                    +--------------+---------------+
                    |                              |
                    ↓                              ↓
        [Web Tier - Zone 1]           [Web Tier - Zone 2]
        (App Service / VMSS)          (App Service / VMSS)
                    |                              |
                    +--------------+---------------+
                                   ↓
                         [Azure Load Balancer]
                                   |
                    +--------------+---------------+
                    |                              |
                    ↓                              ↓
        [App Tier - Zone 1]           [App Tier - Zone 2]
        (App Service / AKS)           (App Service / AKS)
                    |                              |
                    +--------------+---------------+
                                   ↓
                    +--------------+---------------+
                    |              |               |
                    ↓              ↓               ↓
            [Azure SQL DB]  [Redis Cache]  [Azure Storage]
            (with geo-rep)   (Premium)      (GRS/RA-GRS)
```

## Network Architecture

```
Virtual Network (10.0.0.0/16)
│
├─ Application Gateway Subnet (10.0.1.0/24)
│  └─ Application Gateway + WAF
│
├─ Web Tier Subnet (10.0.2.0/24)
│  ├─ Network Security Group (Allow 80/443 from App Gateway)
│  └─ Web Servers / App Services
│
├─ App Tier Subnet (10.0.3.0/24)
│  ├─ Network Security Group (Allow traffic from Web Tier only)
│  └─ Application Servers / AKS
│
└─ Data Tier Subnet (10.0.4.0/24)
   ├─ Network Security Group (Allow traffic from App Tier only)
   └─ Azure SQL MI (if using) / VNet endpoints
```

## Security Features

1. **Network Security**
   - Network Security Groups (NSGs) on each subnet
   - Application Gateway with WAF v2
   - Private endpoints for PaaS services
   - Service endpoints for secure access

2. **Identity & Access**
   - Managed Identities for Azure resources
   - Azure AD authentication
   - RBAC for resource access
   - Key Vault for secrets management

3. **Data Protection**
   - TDE for SQL Database
   - Encryption at rest and in transit
   - Geo-redundant backups
   - Point-in-time restore

## High Availability

- **Availability Zones**: Resources deployed across multiple zones
- **Auto-scaling**: Automatic scaling based on metrics
- **Load Balancing**: Traffic distributed across instances
- **Health Probes**: Automatic failover for unhealthy instances
- **Geo-replication**: Database replicated to secondary region

## Disaster Recovery

- **RTO**: Recovery Time Objective < 1 hour
- **RPO**: Recovery Point Objective < 5 minutes
- **Backup Strategy**: 
  - SQL Database: Automated backups with 7-35 day retention
  - Storage: GRS or RA-GRS replication
  - App configuration: Stored in source control

## Monitoring & Logging

- **Azure Monitor**: Metrics and alerts
- **Application Insights**: Application performance monitoring
- **Log Analytics**: Centralized logging
- **Azure Security Center**: Security recommendations
- **Network Watcher**: Network diagnostics

## Cost Optimization

1. **Compute**: Use appropriate SKUs, reserved instances
2. **Storage**: Lifecycle management policies, appropriate redundancy
3. **Database**: Use appropriate tier, scale down in non-prod
4. **Networking**: Minimize data transfer between regions
5. **Auto-scaling**: Scale down during off-peak hours

## Deployment Considerations

### Compute Options

| Requirement | Recommended Service |
|------------|-------------------|
| Simple web app | App Service |
| Containerized microservices | Azure Kubernetes Service |
| Legacy applications | Virtual Machine Scale Sets |
| Event-driven | Azure Functions |

### Database Options

| Requirement | Recommended Service |
|------------|-------------------|
| Relational OLTP | Azure SQL Database |
| NoSQL document | Cosmos DB |
| Data warehouse | Azure Synapse Analytics |
| Caching | Azure Cache for Redis |

## Step-by-Step Deployment

### Phase 1: Network Infrastructure
1. Create Virtual Network
2. Create subnets for each tier
3. Configure Network Security Groups
4. Deploy Application Gateway

### Phase 2: Data Tier
1. Deploy Azure SQL Database
2. Configure geo-replication
3. Set up Redis Cache
4. Create Storage Account

### Phase 3: Application Tier
1. Deploy App Service or AKS
2. Configure managed identities
3. Set up connection strings (use Key Vault)
4. Configure auto-scaling

### Phase 4: Web Tier
1. Deploy Web App Service or VMSS
2. Configure auto-scaling
3. Set up health probes
4. Connect to Application Gateway

### Phase 5: Security & Monitoring
1. Configure WAF policies
2. Set up Azure Monitor alerts
3. Enable Application Insights
4. Configure diagnostic settings
5. Review Security Center recommendations

## Testing Checklist

- [ ] Application accessible via public endpoint
- [ ] SSL/TLS certificate configured
- [ ] WAF blocking malicious requests
- [ ] Auto-scaling working correctly
- [ ] Database connections working
- [ ] Failover tested (zone failure simulation)
- [ ] Monitoring and alerts configured
- [ ] Backup and restore tested

## Related Azure Services

- **Azure Front Door**: Global load balancing and CDN
- **Azure CDN**: Static content caching
- **Azure API Management**: API gateway and management
- **Azure DevOps**: CI/CD pipelines
- **Azure Key Vault**: Secrets management
- **Azure Container Registry**: Container images

## Best Practices

1. Use Infrastructure as Code (ARM templates, Bicep, Terraform)
2. Implement CI/CD pipelines for deployments
3. Use deployment slots for zero-downtime deployments
4. Implement blue-green or canary deployments
5. Regular security reviews and updates
6. Document architecture decisions
7. Test disaster recovery procedures regularly
8. Monitor costs and optimize regularly

## Exam Tips

For the AZ-305 exam, understand:
- How to choose appropriate services for each tier
- When to use PaaS vs IaaS
- How to design for high availability (99.99% SLA)
- Security best practices for each tier
- Cost optimization strategies
- Disaster recovery planning

## References

- [Azure Architecture Center](https://docs.microsoft.com/en-us/azure/architecture/)
- [N-tier architecture](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/n-tier)
- [Web application architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)
