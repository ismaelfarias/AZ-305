# Azure Networking Solutions

## Overview

Networking is a fundamental component of Azure infrastructure design. Understanding virtual networks, connectivity options, and network security is crucial for the AZ-305 exam.

## Virtual Networks (VNets)

### Key Concepts
- **Address Space**: Private IP range (RFC 1918)
- **Subnets**: Logical divisions within a VNet
- **Service Endpoints**: Direct connection to Azure services
- **Private Endpoints**: Private IP for Azure services

### Common Address Spaces
- `10.0.0.0/8` - Large networks
- `172.16.0.0/12` - Medium networks
- `192.168.0.0/16` - Small networks

### Subnet Planning
```
VNet: 10.0.0.0/16
├─ Gateway Subnet: 10.0.0.0/27 (required for VPN/ExpressRoute)
├─ Firewall Subnet: 10.0.1.0/24 (required for Azure Firewall)
├─ Web Tier: 10.0.10.0/24
├─ App Tier: 10.0.20.0/24
└─ Data Tier: 10.0.30.0/24
```

## VNet Connectivity

### VNet Peering
- **Use Case**: Connect VNets in same or different regions
- **Characteristics**:
  - Low latency, high bandwidth
  - No gateway required
  - Private IP connectivity
  - Non-transitive by default

### Hub-Spoke Topology
```
Hub VNet (10.0.0.0/16)
├─ Shared services (Firewall, DNS, VPN Gateway)
├─ Peering ↔ Spoke 1 VNet (10.1.0.0/16) - Production
├─ Peering ↔ Spoke 2 VNet (10.2.0.0/16) - Development
└─ Peering ↔ Spoke 3 VNet (10.3.0.0/16) - Testing
```

**Benefits**:
- Centralized security and management
- Cost optimization (shared services)
- Isolation between workloads
- Simplified governance

## Hybrid Connectivity

### VPN Gateway
- **Use Case**: Secure connection between on-premises and Azure
- **Types**:
  - Site-to-Site (S2S): Branch office to Azure
  - Point-to-Site (P2S): Individual client to Azure
  - VNet-to-VNet: Between Azure VNets

**VPN Gateway SKUs**:
- Basic: Legacy, not recommended
- VpnGw1-5: Performance tiers (100 Mbps - 10 Gbps)
- VpnGw1AZ-5AZ: Zone-redundant versions

### ExpressRoute
- **Use Case**: Dedicated private connection to Azure
- **Characteristics**:
  - High bandwidth (50 Mbps - 100 Gbps)
  - Consistent latency
  - Bypasses public internet
  - SLA available

**Peering Types**:
- Private Peering: Connect to VNets
- Microsoft Peering: Connect to Microsoft 365, Dynamics 365

**ExpressRoute vs VPN**:
| Feature | ExpressRoute | VPN Gateway |
|---------|--------------|-------------|
| Speed | Up to 100 Gbps | Up to 10 Gbps |
| Latency | Consistent | Variable |
| Cost | Higher | Lower |
| Setup Time | Weeks | Minutes |
| Use Case | Production, high-volume | Development, backup |

## Load Balancing

### Azure Load Balancer
- **Layer**: Layer 4 (Transport)
- **Scope**: Regional
- **Use Case**: Distribute traffic to VMs
- **SKUs**: Basic (free), Standard (advanced features)

### Application Gateway
- **Layer**: Layer 7 (Application)
- **Scope**: Regional
- **Use Case**: Web application load balancing
- **Features**:
  - URL-based routing
  - SSL termination
  - Web Application Firewall (WAF)
  - Cookie-based session affinity

### Azure Front Door
- **Layer**: Layer 7 (Application)
- **Scope**: Global
- **Use Case**: Global web application delivery
- **Features**:
  - Global load balancing
  - SSL offload
  - WAF
  - CDN capabilities
  - Automatic failover

### Traffic Manager
- **Layer**: DNS-based
- **Scope**: Global
- **Use Case**: DNS-level traffic routing
- **Routing Methods**:
  - Priority: Failover scenario
  - Weighted: A/B testing, gradual migration
  - Performance: Route to nearest endpoint
  - Geographic: Route based on location

## Network Security

### Network Security Groups (NSGs)
- **Functionality**: Firewall rules for subnets/NICs
- **Rules**: Priority-based (100-4096)
- **Direction**: Inbound and outbound

**Example Rules**:
```
Priority  Name              Source      Destination  Port   Action
100       Allow-Web         Internet    *            80     Allow
110       Allow-SSL         Internet    *            443    Allow
200       Allow-SSH-Admin   10.0.1.0/24 *            22     Allow
65000     Deny-All          *           *            *      Deny
```

### Azure Firewall
- **Type**: Managed, cloud-native firewall
- **Features**:
  - Stateful packet inspection
  - Application-level filtering
  - Threat intelligence
  - High availability
- **Use Case**: Centralized network security in hub VNet

### Application Security Groups (ASGs)
- **Purpose**: Logical grouping of VMs
- **Benefit**: Simplify NSG rules
- **Example**:
  ```
  ASG-WebServers: {VM1, VM2, VM3}
  ASG-AppServers: {VM4, VM5}
  
  NSG Rule: Allow traffic from ASG-WebServers to ASG-AppServers on port 8080
  ```

### DDoS Protection
- **Basic**: Free, automatic protection
- **Standard**: Enhanced protection with telemetry
- **Features**:
  - Always-on traffic monitoring
  - Automatic attack mitigation
  - Cost protection

## DNS Solutions

### Azure DNS
- **Purpose**: Host DNS zones in Azure
- **Benefits**:
  - High availability (100% uptime SLA)
  - Fast global resolution
  - Azure RBAC integration
  - Private DNS zones

### Private DNS Zones
- **Use Case**: Name resolution within VNets
- **Benefits**:
  - Custom domain names for private resources
  - No need for custom DNS servers
  - Automatic registration for VMs

## Content Delivery

### Azure CDN
- **Purpose**: Cache and deliver content globally
- **Providers**: Microsoft, Akamai, Verizon
- **Use Case**: Static content, media streaming
- **Benefits**:
  - Reduced latency
  - Decreased load on origin servers
  - Better user experience

## Network Monitoring

### Network Watcher
- **Features**:
  - IP flow verify
  - Next hop
  - Connection troubleshoot
  - Packet capture
  - NSG flow logs
  - Traffic Analytics

### Connection Monitor
- **Purpose**: Monitor network connectivity
- **Capabilities**:
  - End-to-end connectivity checks
  - Latency measurements
  - Topology visualization

## Design Patterns

### Secure Perimeter Network (DMZ)
```
Internet
   ↓
Azure Firewall / Application Gateway
   ↓
DMZ Subnet (Web servers)
   ↓
Internal Load Balancer
   ↓
Internal Subnet (App servers)
   ↓
Data Subnet (Databases)
```

### Zero Trust Network
- Assume breach
- Verify explicitly
- Least privilege access
- Microsegmentation
- End-to-end encryption

## Best Practices

1. **Address Planning**:
   - Plan IP ranges carefully
   - Avoid overlapping address spaces
   - Reserve space for growth

2. **Subnet Design**:
   - Create subnets based on security zones
   - Don't make subnets too small
   - Use NSGs on subnets

3. **Connectivity**:
   - Use ExpressRoute for production workloads
   - Have VPN as backup for ExpressRoute
   - Implement hub-spoke topology for multiple workloads

4. **Security**:
   - Default deny in NSGs
   - Use Azure Firewall for centralized control
   - Enable DDoS Protection Standard for public IPs
   - Implement network monitoring

5. **High Availability**:
   - Use zone-redundant gateways
   - Implement redundant ExpressRoute circuits
   - Deploy load balancers in standard SKU

## Exam Tips

- Understand when to use VPN vs ExpressRoute
- Know the difference between load balancing options
- Understand NSG rule evaluation order
- Know hub-spoke topology benefits
- Understand private endpoints vs service endpoints

## Common Scenarios

### Scenario 1: Hybrid Connectivity
**Requirement**: Connect on-premises datacenter to Azure
**Solution**: ExpressRoute with VPN backup

### Scenario 2: Multi-Region Web App
**Requirement**: Global web application with failover
**Solution**: Azure Front Door + App Service (multi-region)

### Scenario 3: Secure Internal Application
**Requirement**: Internal app, no public access
**Solution**: Private Endpoint + Private DNS Zone

## References

- [Azure Networking Documentation](https://docs.microsoft.com/en-us/azure/networking/)
- [Hub-spoke topology](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
- [Network security best practices](https://docs.microsoft.com/en-us/azure/security/fundamentals/network-best-practices)
