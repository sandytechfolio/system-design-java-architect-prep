# CAP Theorem (Brewer’s Theorem)

The CAP Theorem is a fundamental principle in distributed systems that explains the trade-offs between **Consistency**, **Availability**, and **Partition Tolerance**.

---

## The Three Properties

### Consistency (C)

> Every read receives the most recent write (all nodes see the same data at the same time).

**Example:**  
In a banking system, after you transfer money, your new balance must be immediately visible everywhere.

---

### Availability (A)

> Every request receives a non-error response — the system is always available to respond.

**Example:**  
Even if a node is down, you can still get a response from another node.

---

### Partition Tolerance (P)

> The system continues to operate despite network failures (message loss, delays, or nodes being cut off).

**Example:**  
If nodes in Europe can’t talk to nodes in the US due to a network split, both sides should still function.

---

## The Trade-off

In a distributed system, **network partitions (P) are inevitable**.

So, in the presence of a partition, you must choose **Consistency (C)** or **Availability (A)**.

This means:

---

- **CP systems:** Prioritize **Consistency + Partition tolerance**.  
  May sacrifice availability during network issues.  
  _Example:_ HBase, MongoDB (when configured with majority writes).

- **AP systems:** Prioritize **Availability + Partition tolerance**.  
  May return stale/approximate data but always respond.  
  _Example:_ Cassandra, DynamoDB.

- **CA systems:** Prioritize **Consistency + Availability**  
  Not possible under partitions (only possible in a single-node or perfectly reliable network).  
  _Example:_ A traditional relational database (not distributed).
