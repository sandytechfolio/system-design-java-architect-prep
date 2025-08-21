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

---


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

# 🔹 Types of Consistency in Distributed Systems

## 1. Strong Consistency
- After a write, **all reads** return the most recent value.
- Users always see the latest data, no matter which replica they read from.
- ✅ Guarantees correctness  
- ❌ Slower, higher latency (requires coordination/consensus)  
- **Examples**: Google Spanner, HBase (with quorum), traditional RDBMS.  

---

## 2. Eventual Consistency
- Given enough time (and no new writes), all replicas **converge** to the same value.
- Reads may return **stale data** for some time.
- ✅ High availability, low latency  
- ❌ Inconsistency possible temporarily  
- **Examples**: DynamoDB, Cassandra, S3, DNS systems.  

---

## 3. Causal Consistency
- If **operation B is caused by A**, then every node must see A before B.
- Independent operations may be seen in different orders.
- ✅ Preserves “cause-effect” relationships  
- **Example**: Social media — if you see a post, you should also see the “like” on it afterwards.  

---

## 4. Read-Your-Writes Consistency
- After a user writes data, their own reads will **always reflect that write**.
- Other users may still see stale values temporarily.
- ✅ Good for user-facing apps (e.g., updating your profile picture).  
- **Examples**: Many mobile apps, caching layers with session stickiness.  

---

## 5. Monotonic Reads
- If a user reads a value, **future reads will never return older data**.
- Guarantees "time doesn’t go backward" for a single user.
- ✅ Avoids confusion (e.g., seeing “100 likes” first and later “80 likes”).  

---

## 6. Monotonic Writes
- A user’s writes are **seen in the order they were made**.
- Ensures no write reordering (e.g., updates to account balance).  

---

## 7. Session Consistency
- Guarantees **read-your-writes + monotonic reads** within a session.
- Common in **mobile and web apps** where a user session must behave consistently.
- **Example**: Azure Cosmos DB offers session consistency.  

---

## 8. Linearizability (Single-copy Consistency)
- Strongest form: Every read/write appears to occur **instantly at a single point in time**.
- Equivalent to having **one copy of data**.
- ✅ Simplest to reason about  
- ❌ Most expensive (requires synchronization/consensus like Paxos/Raft).  
- **Examples**: Zookeeper, Chubby (Google).  

---

# 🔹 Quick Interview Mapping
- **Strong Consistency** → Banking, stock trading.  
- **Eventual Consistency** → Social media feeds, DNS, shopping carts.  
- **Causal Consistency** → Social networks, chat apps.  
- **Session Consistency** → User sessions (profile updates, cart).  
- **Linearizability** → Leader election, distributed locks.

---

# 🔹 Consistency Types Comparison Table

| Consistency Type       | Guarantees / Behavior                                                                 | Example Use Case                           |
|------------------------|--------------------------------------------------------------------------------------|-------------------------------------------|
| **Strong Consistency** | All reads return the most recent write.                                               | Banking, stock trading                     |
| **Eventual Consistency** | Replicas converge over time; reads may return stale data temporarily.               | Social media feeds, DNS, shopping carts   |
| **Causal Consistency** | Operations that are causally related are seen in order; independent ops may be reordered. | Social media posts & likes, chat apps     |
| **Read-Your-Writes**   | A user always sees their own writes in subsequent reads.                              | User profile updates, session data        |
| **Monotonic Reads**    | Once a value is read, future reads will never return older data.                      | Likes count, read counters                 |
| **Monotonic Writes**   | Writes from a user are seen in the order they were issued.                             | Account balance updates                     |
| **Session Consistency**| Guarantees read-your-writes + monotonic reads within a session.                       | Mobile/web apps with user sessions         |
| **Linearizability**    | Each operation appears to occur instantaneously at a single point in time (single-copy consistency). | Leader election, distributed locks         |
