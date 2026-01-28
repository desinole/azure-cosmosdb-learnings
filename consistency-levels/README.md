# Consistency Levels

This section covers learnings related to consistency levels in Azure Cosmos DB.

## Topics Covered

- The five consistency levels (Strong, Bounded Staleness, Session, Consistent Prefix, Eventual)
- Consistency vs. performance tradeoffs
- Consistency guarantees across regions
- Session token management
- Choosing the right consistency level
- Impact on latency and availability

## Key Learnings

### Consistency Level Comparison
Document the characteristics and use cases for each consistency level.

| Consistency Level | Latency | Throughput | Use Cases |
|-------------------|---------|------------|-----------|
| Strong | Highest | Lowest | Financial transactions, inventory |
| Bounded Staleness | High | Low | Time-sensitive reads with acceptable lag |
| Session | Medium | Medium | User sessions, shopping carts |
| Consistent Prefix | Low | High | Social feeds, comments |
| Eventual | Lowest | Highest | Telemetry, analytics |

### Session Consistency
Learn about managing session tokens for session consistency across clients.

### Multi-Region Consistency
Understand consistency guarantees in globally distributed deployments.

## Best Practices

- Use Session consistency as the default for most applications
- Choose Strong consistency only when absolutely necessary
- Understand the latency-consistency tradeoff
- Test your application with different consistency levels
- Use per-request consistency overrides when needed
- Consider Bounded Staleness for multi-region strong consistency
- Document consistency requirements in your design

## Resources

- [Consistency levels](https://learn.microsoft.com/azure/cosmos-db/consistency-levels)
- [Consistency tradeoffs](https://learn.microsoft.com/azure/cosmos-db/consistency-levels-tradeoffs)
- [Choose the right consistency level](https://learn.microsoft.com/azure/cosmos-db/consistency-levels-choosing)
