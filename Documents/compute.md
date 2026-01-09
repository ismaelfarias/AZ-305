# Azure Compute Solutions

## Overview

Azure provides multiple compute options to host applications and services. Understanding when to use each option is critical for the AZ-305 exam.

## Compute Options

### Virtual Machines (VMs)
- **Use Case**: Full control over OS, legacy applications, specific software requirements
- **Considerations**: 
  - Choose appropriate VM sizes (General purpose, Compute optimized, Memory optimized, etc.)
  - Use Availability Sets or Availability Zones for high availability
  - Consider Reserved Instances for cost optimization
  - Use Managed Disks for better reliability

### Azure App Service
- **Use Case**: Web apps, REST APIs, mobile backends
- **Considerations**:
  - PaaS solution with built-in scaling
  - Multiple deployment slots for testing
  - Support for .NET, Java, Node.js, Python, PHP
  - Integrated with Azure DevOps and GitHub

### Azure Container Instances (ACI)
- **Use Case**: Simple containerized applications, burst scenarios
- **Considerations**:
  - Fast startup
  - Per-second billing
  - No orchestration overhead
  - Good for short-lived workloads

### Azure Kubernetes Service (AKS)
- **Use Case**: Microservices, container orchestration at scale
- **Considerations**:
  - Managed Kubernetes cluster
  - Automatic upgrades and patching
  - Integration with Azure AD, Monitor, and Container Registry
  - Scaling with KEDA or HPA

### Azure Functions
- **Use Case**: Event-driven, serverless compute
- **Considerations**:
  - Consumption plan for automatic scaling
  - Premium plan for VNet integration
  - Multiple trigger types (HTTP, Timer, Queue, etc.)
  - Stateful workflows with Durable Functions

### Azure Batch
- **Use Case**: Large-scale parallel and HPC workloads
- **Considerations**:
  - Automatic scaling
  - Job scheduling
  - Good for data processing and rendering

## Decision Tree

```
Need full OS control? → Virtual Machines
Web application? → App Service
Microservices architecture? → AKS
Event-driven tasks? → Azure Functions
Short-lived containers? → Container Instances
Parallel processing? → Azure Batch
```

## Key Concepts

### Scaling
- **Vertical Scaling**: Increase VM size (scale up/down)
- **Horizontal Scaling**: Add more instances (scale out/in)
- **Auto-scaling**: Automatically adjust based on metrics

### Availability
- **Availability Sets**: Protect against hardware failures (99.95% SLA)
- **Availability Zones**: Protect against datacenter failures (99.99% SLA)
- **Load Balancers**: Distribute traffic across instances

### Cost Optimization
- **Reserved Instances**: 1 or 3 year commitments for VMs
- **Spot Instances**: Use excess capacity at reduced cost
- **Azure Hybrid Benefit**: Use existing Windows Server licenses
- **Consumption-based**: Pay only for what you use (Functions, ACI)

## Best Practices

1. Choose the right compute service based on requirements
2. Implement proper monitoring and diagnostics
3. Use managed services when possible (less operational overhead)
4. Design for scalability and resilience
5. Implement proper security (NSGs, managed identities)
6. Consider multi-region deployments for critical workloads

## Related Services

- Azure Container Registry: Store and manage container images
- Azure DevOps: CI/CD pipelines
- Azure Monitor: Performance monitoring and diagnostics
- Application Insights: Application performance management

## References

- [Azure Compute Documentation](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)
- [Compute Comparison](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-comparison)
