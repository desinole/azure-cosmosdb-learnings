# Data Modeling

This section covers learnings related to data modeling in Azure Cosmos DB.

## Topics Covered

- Partition key selection
- Denormalization vs. normalization
- Embedding vs. referencing
- Handling relationships (1:1, 1:N, N:N)
- Schema design patterns
- Document structure optimization
- Hierarchical data modeling
- Time-series data patterns

## Key Learnings

### Partition Key Design
Document strategies for choosing effective partition keys to avoid hot partitions.

### Denormalization Strategies
Learn when to denormalize data for optimal performance.

### Data Relationships
Share patterns for modeling different types of relationships in a NoSQL database.

### Schema Flexibility
Understand how to leverage schema flexibility while maintaining data integrity.

## Best Practices

- Choose partition keys with high cardinality
- Denormalize data to optimize for read performance
- Keep related data together when possible
- Avoid unbounded document growth
- Design for your query patterns, not your data structure
- Use composite partition keys for complex scenarios
- Consider synthetic partition keys for evenly distributed workloads
- Plan for data growth and lifecycle management

## Resources

- [Data modeling](https://docs.microsoft.com/azure/cosmos-db/modeling-data)
- [Partition key strategies](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview)
- [Real-world data modeling examples](https://docs.microsoft.com/azure/cosmos-db/how-to-model-partition-example)
