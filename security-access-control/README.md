# Security & Access Control

This section covers learnings related to security and access control in Azure Cosmos DB.

## Topics Covered

- Authentication methods (keys, Azure AD, managed identities)
- Role-Based Access Control (RBAC)
- Network security (VNet, Private Endpoints, Firewall)
- Encryption at rest and in transit
- Customer-managed keys
- Audit logging
- Data masking and compliance
- Resource tokens for fine-grained access

## Key Learnings

### Authentication Best Practices
Document secure authentication patterns and when to use each method.

### Network Security
Learn about implementing network isolation and private connectivity.

### RBAC Configuration
Share insights on configuring role-based access control effectively.

### Compliance
Understand compliance certifications and data residency requirements.

## Best Practices

- Use Azure AD authentication with managed identities when possible
- Rotate access keys regularly
- Implement network restrictions using VNet and Private Endpoints
- Enable audit logging for compliance requirements
- Use resource tokens for client-side applications
- Implement least privilege access principles
- Enable customer-managed keys for sensitive data
- Configure firewall rules to restrict access

## Resources

- [Security in Azure Cosmos DB](https://learn.microsoft.com/azure/cosmos-db/database-security)
- [Azure AD authentication](https://learn.microsoft.com/azure/cosmos-db/how-to-setup-rbac)
- [Network security](https://learn.microsoft.com/azure/cosmos-db/how-to-configure-vnet-service-endpoint)
