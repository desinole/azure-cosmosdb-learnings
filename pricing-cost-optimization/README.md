# Pricing & Cost Optimization

This section covers learnings related to pricing and cost optimization in Azure Cosmos DB.

## Topics Covered

- Request Units (RUs) understanding
- Provisioned vs. Serverless vs. Autoscale
- Storage costs
- Multi-region cost implications
- Cost optimization strategies
- Reserved capacity pricing
- Backup and restore costs
- Analytical store costs

## Key Learnings

### Throughput Modes
Document when to use each throughput mode for optimal cost-efficiency.

| Mode        | Best For                 | Cost Pattern                  |
|-------------|--------------------------|-------------------------------|
| Provisioned | Predictable workloads    | Pay for provisioned RU/s      |
| Autoscale   | Variable workloads       | Pay for actual usage (within limits) |
| Serverless  | Sporadic/unpredictable   | Pay per request               |

### RU Optimization
Learn techniques to reduce RU consumption without sacrificing functionality.

### Cost Monitoring
Share strategies for tracking and managing costs effectively.

## Best Practices

- Right-size your throughput based on actual usage patterns
- Use autoscale for variable workloads
- Consider serverless for dev/test and low-traffic scenarios
- Optimize queries to reduce RU consumption
- Review and optimize indexing policies
- Use shared throughput at database level when appropriate
- Monitor and adjust partition key strategy to avoid hot partitions
- Consider reserved capacity for predictable long-term workloads
- Implement data lifecycle policies (TTL) to manage storage costs
- Use analytical store for reporting instead of operational queries

## Cost Analysis Tools

- Azure Cost Management
- Azure Cosmos DB Capacity Calculator
- Request Unit Calculator
- Built-in metrics for RU consumption

## Resources

- [Pricing model](https://learn.microsoft.com/azure/cosmos-db/understand-your-bill)
- [Cost optimization](https://learn.microsoft.com/azure/cosmos-db/plan-manage-costs)
- [Capacity calculator](https://cosmos.azure.com/capacitycalculator/)
- [Reserved capacity](https://learn.microsoft.com/azure/cosmos-db/cosmos-db-reserved-capacity)
