- [[Tech Fundamentals]]

## Indexing

### Overview
- Data structure that improves query performance
- Similar to book index - points to data location
- Speeds up SELECT queries, slows down INSERT/UPDATE/DELETE

### Types
- **Primary Index** - automatically created on primary key
- **Unique Index** - enforces uniqueness constraint
- **Composite Index** - multiple columns (order matters)
- **Clustered Index** - data physically sorted (one per table)
- **Non-clustered Index** - separate structure, points to data

### How It Works
- B-tree structure (balanced tree)
- O(log n) lookup time vs O(n) full table scan
- Query optimizer uses indexes automatically

### When to Use
- Frequently queried columns
- Foreign keys
- WHERE clause columns
- JOIN columns
- ORDER BY columns

### Trade-offs
- **Pros**: Faster reads, better query performance
- **Cons**: Storage overhead, slower writes, maintenance cost

### Best Practices
- Don't over-index (diminishing returns)
- Index columns used in WHERE, JOIN, ORDER BY
- Monitor index usage
- Consider composite indexes for multi-column queries
- Drop unused indexes

## Execution Plan
- Query optimizer's strategy for executing SQL
- Shows index usage, join methods, scan types
- Use EXPLAIN to analyze query performance

## Databases
- [[oracle]]
- [[my-sql]]
