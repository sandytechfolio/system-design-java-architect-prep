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

## 🔹 CAP Triangle
markdown
Copy
Edit
      Consistency (C)
         /\
        /  \
       /    \
Availability ---- Partition Tolerance
(A) (P)

yaml
Copy
Edit

- In distributed systems, **network partitions (P) are inevitable**.  
- Real choice = **Consistency (C) vs Availability (A)**.  

---

## 🔹 CAP System Types & Examples
| Type | Trade-off | Examples |
|------|-----------|----------|
| **CA** (Consistency + Availability) | Works only when no partitions. Not realistic for large distributed systems. | Traditional RDBMS (MySQL, PostgreSQL, Oracle in single-node mode) |
| **CP** (Consistency + Partition tolerance) | Always consistent, but may sacrifice availability under partition. | MongoDB (majority writes), HBase, Zookeeper, Spanner |
| **AP** (Availability + Partition tolerance) | Always available, but may return stale/inconsistent data (eventual consistency). | DynamoDB, Cassandra, CouchDB, Riak, Cosmos DB |

---

## 🔹 Real-World Scenarios
| Use Case | CAP Choice | Why |
|----------|------------|-----|
| **Banking / Money Transfers** | **CP** | Accuracy > availability (better to fail than show wrong balance). |
| **E-commerce Shopping Cart** | **AP** | Must accept writes (cart updates) even if inventory is slightly stale. |
| **Social Media Feed** | **AP** | Better to see slightly old posts than see downtime. |
| **Stock Trading** | **CP** | Trades must be consistent, downtime is better than incorrect trades. |
| **DNS / CDN** | **AP** | Must always resolve requests, stale data is acceptable. |

---

## 🔹 Quick Interview Tips
- **Always say:** Partition Tolerance (P) is **non-negotiable** in distributed systems.  
- Real choice: **C vs A**.  
- **Consistency (CP)** = Banking, Trading.  
- **Availability (AP)** = Social Media, Shopping Carts, DNS/CDN.  
- **CA** = Only in single-node or ideal networks.  
