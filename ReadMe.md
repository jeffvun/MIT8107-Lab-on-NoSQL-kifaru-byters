Here is the corrected and formatted Lab Manual in Markdown. I have fixed the spacing issues in the commands (e.g., changing `d ock er` to `docker`) and structured it for a professional GitHub README.

-----

# Redis Key-Value Database Lab Manual

## 1\. Introduction

### What is a Key-Value Database?

A key-value database is the simplest type of NoSQL database that stores data as a collection of **key-value pairs**. Each key is unique and acts as an identifier to retrieve its associated value. Think of it like a dictionary or hash map.

**Key Characteristics:**

  * **Simple structure:** Key → Value
  * **Fast lookups:** O(1) time complexity
  * **Flexible values:** Can store strings, numbers, lists, sets, hashes, etc.
  * **High performance:** Optimized for speed and scalability

### Why Redis?

Redis (**Re**mote **Di**ctionary **S**erver) is an open-source, in-memory key-value store known for:

  * Extremely fast read/write operations (sub-millisecond latency)
  * Rich data types (strings, hashes, lists, sets, sorted sets)
  * Built-in persistence options
  * Wide industry adoption (Twitter, GitHub, Stack Overflow)

### When to Use Key-Value Stores?

Key-value DBs are ideal for:

  * Session management and caching
  * Real-time analytics and leaderboards
  * Rate limiting and API throttling
  * Shopping carts
  * User preferences and profiles
  * Message queues

**Note:** They are not best suited for complex queries with multiple conditions, relationships between entities, or data requiring frequent aggregations.

-----

## 2\. Setup Instructions

### Prerequisites

1.  **VS Code:** [Link]
2.  **Git and Git Bash:** [Link]
3.  **GitHub Desktop:** [Link]
4.  **WSL (for Windows OS):** [Link]
5.  **Docker:** [Link]
6.  **VS Code extensions:** Docker and YAML

### Step 1: Pull Redis Docker Image

Enter the following command to pull the Redis Docker image:

```bash
docker pull redis:7.2-alpine
```

### Step 2: Run Redis Container

Enter the following command to start the Redis container with persistent storage:

```bash
docker run -d \
--name redis-lab \
-p 6379:6379 \
-v redis-data:/data \
redis:7.2-alpine redis-server --appendonly yes
```

**Command Breakdown:**

  * `-d`: Run in detached mode (background)
  * `--name redis-lab`: Name the container for easy reference
  * `-p 6379:6379`: Map port 6379 (host:container)
  * `-v redis-data:/data`: Create volume for data persistence
  * `--appendonly yes`: Enable AOF (Append Only File) persistence

### Step 3: Verify Installation

Enter the following command to check if the container is running:

```bash
docker ps
```

**Expected Output:**

### Step 4: Connect to Redis CLI

Enter the following command to connect to the Redis command-line interface:

```bash
docker exec -it redis-lab redis-cli
```

You should see the Redis prompt:
`127.0.0.1:6379>`

### Step 5: Test Connection

Inside Redis CLI, test with the PING command:

```redis
PING
```

**Expected Output:**

-----

## 3\. Basic Operations (CRUD)

### Understanding Redis Data Types

Before we dive into CRUD operations, let's understand Redis's core data types:

1.  **String:** Simple key-value pairs
2.  **Hash:** Key with multiple field-value pairs (like objects)
3.  **List:** Ordered collection of strings
4.  **Set:** Unordered collection of unique strings
5.  **Sorted Set:** Set ordered by score

### CREATE Operations

#### A. String Operations

Set a simple key-value pair:

```redis
SET user:1001:name "Alice Johnson"
```

Set with expiration (EX = seconds, PX = milliseconds):

```redis
SET session:abc123 "user_data" EX 3600
```

Set only if key doesn't exist (NX = Not eXists):

```redis
SET user:1001:created_at "2025-11-28" NX
```

Set multiple key-value pairs at once:

```redis
MSET user:1001:email "alice@email.com" user:1001:age "28" user:1001:country "Kenya"
```

**Expected Output:**

#### B. Hash Operations (Storing Objects)

Create a hash with multiple fields:

```redis
HSET user:1002 name "Bob Mwangi" email "bob@email.com" age 32 country "Kenya"
```

Add individual fields to hash:

```redis
HSET user:1002 role "developer"
HSET user:1002 department "Engineering"
```

**Expected Output:**

#### C. List Operations

Create a list (push from right/left):

```redis
RPUSH user:1001:login_history "2025-11-28 09:00:00"
RPUSH user:1001:login_history "2025-11-28 14:30:00"
LPUSH user:1001:notifications "Welcome to the platform!"
```

#### D. Set Operations

Add members to a set:

```redis
SADD user:1001:skills "Python" "Java" "Docker" "Redis"
SADD user:1002:skills "JavaScript" "Python" "React"
```

### READ Operations

#### A. String Operations

Get a single value:

```redis
GET user:1001:name
```

Get multiple values at once:

```redis
MGET user:1001:name user:1001:email user:1001:age
```

Check if key exists:

```redis
EXISTS user:1001:name
```

Get all keys matching a pattern (use carefully in production\!):

```redis
KEYS user:1001:*
```

**Expected Output:**

#### B. Hash Operations

Get all fields and values from a hash:

```redis
HGETALL user:1002
```

Get specific field from hash:

```redis
HGET user:1002 name
```

Get multiple fields from hash:

```redis
HMGET user:1002 name email role
```

Get all field names:

```redis
HKEYS user:1002
```

Get all values:

```redis
HVALS user:1002
```

Check if field exists:

```redis
HEXISTS user:1002 age
```

**Expected Output for HGETALL:**

#### C. List Operations

Get all elements from a list:

```redis
LRANGE user:1001:login_history 0 -1
```

Get list length:

```redis
LLEN user:1001:login_history
```

Get element at specific index:

```redis
LINDEX user:1001:login_history 0
```

#### D. Set Operations

Get all members of a set:

```redis
SMEMBERS user:1001:skills
```

Check if member exists in set:

```redis
SISMEMBER user:1001:skills "Python"
```

Get number of members:

```redis
SCARD user:1001:skills
```

Find intersection of sets (common skills):

```redis
SINTER user:1001:skills user:1002:skills
```

**Expected Output:**

### UPDATE Operations

#### A. String Operations

Update existing value:

```redis
SET user:1001:name "Alice Wanjiku"
```

Increment numeric value:

```redis
SET user:1001:login_count 5
INCR user:1001:login_count
INCRBY user:1001:login_count 10
```

Decrement numeric value:

```redis
DECR user:1001:login_count
DECRBY user:1001:login_count 2
```

Append to string value:

```redis
SET user:1001:bio "Data Scientist"
APPEND user:1001:bio " | AI Enthusiast"
GET user:1001:bio
```

**Expected Output:**

#### B. Hash Operations

Update field in hash:

```redis
HSET user:1002 age 33
```

Increment numeric field:

```redis
HINCRBY user:1002 age 1
```

Update multiple fields:

```redis
HMSET user:1002 role "senior_developer" department "Platform Engineering"
```

#### C. List Operations

Update element at specific index:

```redis
LSET user:1001:login_history 0 "2025-11-28 09:15:00 (Updated)"
```

#### D. Set Operations

Add new members to set:

```redis
SADD user:1001:skills "Kubernetes" "AWS"
```

Remove and add in one operation (not atomic, but sequential):

```redis
SREM user:1001:skills "Java"
SADD user:1001:skills "Golang"
```

### DELETE Operations

#### A. Delete Keys

Delete a single key:

```redis
DEL user:1001:created_at
```

Delete multiple keys:

```redis
DEL user:1001:email user:1001:age
```

Delete key with expiration check and set a key with TTL:

```redis
SET temp:key "temporary data" EX 10
TTL temp:key
# Check time to live (Wait 10 seconds and it auto-deletes)
```

**Expected Output:**

#### B. Hash Operations

Delete specific fields from hash:

```redis
HDEL user:1002 department
```

Delete entire hash:

```redis
DEL user:1002
```

#### C. List Operations

Remove elements by value:

```redis
LREM user:1001:notifications 1 "Welcome to the platform!"
```

Delete entire list:

```redis
DEL user:1001:login_history
```

#### D. Set Operations

Remove member from set:

```redis
SREM user:1001:skills "Docker"
```

Remove random member:

```redis
SPOP user:1001:skills
```

Delete entire set:

```redis
DEL user:1001:skills
```

-----

## 4\. Applied Scenario: Real-Time Session Management System

### Problem Statement

You're building a high-traffic web application (e.g., an e-learning platform) that needs to:

  * Manage user sessions across multiple servers
  * Track active users in real-time
  * Implement rate limiting for API endpoints
  * Store temporary shopping cart data
  * Maintain real-time leaderboards for gamification

**Traditional relational databases struggle with:** High write throughput for session updates, sub-millisecond read latency, automatic expiration, and horizontal scaling.

### Solution: Redis Key-Value Store

Redis excels here because of in-memory storage, built-in TTL, atomic operations, horizontal scalability, and complex data structures.

### Implementation

#### Step 1: User Authentication & Session Creation

When a user logs in, create a session with a session ID and store it as a hash with 30-minute expiration:

```redis
HSET sess:550e8400-e29b-41d4-a716-446655440000 \
user_id "user:1001" \
username "alice@email.com" \
role "student" \
login_time "2025-11-28T10:30:00Z" \
ip_address "102.133.45.67" \
device "Chrome/MacOS"
```

Set session expiration (1800 secs = 30 min):

```redis
EXPIRE sess:550e8400-e29b-41d4-a716-446655440000 1800
```

Add user to active users set (real-time tracking):

```redis
SADD active:users:2025-11-28 "user:1001"
```

Track login event:

```redis
RPUSH user:1001:login_events "2025-11-28T10:30:00Z | 102.133.45.67 | Chrome/MacOS"
LTRIM user:1001:login_events -10 -1 
# Keep last 10 login events
```

Increment daily login counter:

```redis
INCR stats:logins:2025-11-28
```

**Expected Output:**

#### Step 2: Session Validation (Read)

When user makes a request, we validate the session:

```redis
GET sess:550e8400-e29b-41d4-a716-446655440000
```

If session exists, retrieve user details:

```redis
HGETALL sess:550e8400-e29b-41d4-a716-446655440000
```

Check remaining session time:

```redis
TTL sess:550e8400-e29b-41d4-a716-446655440000
```

Get user's role for authorization:

```redis
HGET sess:550e8400-e29b-41d4-a716-446655440000 role
```

**Expected Output:**

#### Step 3: Session Activity Update

After refreshing the session (`EXPIRE sess:session_id duration`), update last activity:

```redis
HSET sess:550e8400-e29b-41d4-a716-446655440000 last_activity "2025-11-28T10:45:00Z"
```

Track page views:

```redis
INCR user:1001:page_views:2025-11-28
```

#### Step 4: Shopping Cart Management

Add items to shopping cart (using hash):

```redis
HSET cart:user:1001 \
item:course101 "1|299.99|Introduction to Python" \
item:course205 "1|399.99|Advanced Data Structures" \
item:book42 "2|29.99|Redis Essentials"
```

Set cart expiration (24 hours):

```redis
EXPIRE cart:user:1001 86400
```

Get cart contents:

```redis
HGETALL cart:user:1001
```

Update item quantity:

```redis
HSET cart:user:1001 item:book42 "3|29.99|Redis Essentials"
```

Remove item from cart:

```redis
HDEL cart:user:1001 item:course101
```

Calculate cart total (application logic):

```redis
HVALS cart:user:1001
```

**Expected Output:**

#### Step 5: Rate Limiting (API Throttling)

Limit: 100 requests per minute per user. Key format: `ratelimit:user_id:minute`.
Record request:

```redis
INCR ratelimit:user:1001:2025-11-28:10:45
EXPIRE ratelimit:user:1001:2025-11-28:10:45 60
```

Check if limit exceeded:

```redis
GET ratelimit:user:1001:2025-11-28:10:45
```

Simulate multiple requests:

```redis
INCR ratelimit:user:1001:2025-11-28:10:45
INCR ratelimit:user:1001:2025-11-28:10:45
GET ratelimit:user:1001:2025-11-28:10:45
```

**Rate Limiting Logic (Pseudocode):**

```python
current_count = redis.get(f"ratelimit:user:{user_id}:{current_minute}")
if current_count and int(current_count) > 100:
    return "Rate limit exceeded. Try again later."
else:
    redis.incr(f"ratelimit:user:{user_id}:{current_minute}")
    redis.expire(f"ratelimit:user:{user_id}:{current_minute}", 60)
    # Process request
```

#### Step 6: Real-Time Leaderboard (Sorted Sets)

Track student points using sorted sets. The Score represents total points.
Add students to leaderboard:

```redis
ZADD leaderboard:november:2025 \
850 "user:1001" \
920 "user:1002" \
780 "user:1003" \
1050 "user:1004" \
890 "user:1005"
```

Get top 3 students (highest scores):

```redis
ZREVRANGE leaderboard:november:2025 0 2 WITHSCORES
```

Get student's rank (0-indexed, from highest):

```redis
ZREVRANK leaderboard:november:2025 "user:1001"
```

Get student's score:

```redis
ZSCORE leaderboard:november:2025 "user:1001"
```

Increment student's score (earned 50 points):

```redis
ZINCRBY leaderboard:november:2025 50 "user:1001"
```

Get students within score range:

```redis
ZRANGEBYSCORE leaderboard:november:2025 800 900 WITHSCORES
```

Get total number of students:

```redis
ZCARD leaderboard:november:2025
```

**Expected Output:**

#### Step 7: Session Logout (Delete)

Delete session:

```redis
DEL sess:550e8400-e29b-41d4-a716-446655440000
```

Remove from active users:

```redis
SREM active:users:2025-11-28 "user:1001"
```

Optional: Clear shopping cart if not persistent:

```redis
DEL cart:user:1001
```

Log logout event:

```redis
RPUSH user:1001:login_events "2025-11-28T11:15:00Z | LOGOUT"
LTRIM user:1001:login_events -10 -1
```

#### Step 8: Analytics Queries

```redis
# Get count of active users today
SCARD active:users:2025-11-28

# Get total logins today
GET stats:logins:2025-11-28

# Get user's page views today
GET user:1001:page_views:2025-11-28

# Get recent login events for user
LRANGE user:1001:login_events 0 -1

# Get all active sessions (pattern matching - use carefully!)
KEYS sess:*
```

### Scenario Performance Benefits

**Comparison: Redis vs Traditional RDBMS**

| Operation | Redis (Key-Value) | PostgreSQL (RDBMS) |
| :--- | :--- | :--- |
| Session Write | \< 1ms | 5-20ms |
| Session Read | \< 1ms | 3-15ms |
| Leaderboard Query | \< 1ms | 50-200ms |
| Rate Limit Check | \< 1ms | 10-30ms |
| Auto-Expiration | Native (TTL) | Requires cleanup jobs |
| Concurrent Users | Millions | Thousands |

**Real-World Impact:**

  * **Latency:** 10-20x faster response times
  * **Throughput:** Handle 100,000+ req/sec per instance
  * **Simplicity:** No complex queries or indexes needed
  * **Cost:** Lower server requirements due to efficiency

-----

## 5\. Advanced Operations

### Transactions (MULTI/EXEC)

Achieve atomic operations (all or nothing) using transactions:

```redis
MULTI
SET account:1001:balance 1000
DECRBY account:1001:balance 100
INCRBY account:1002:balance 100
EXEC
```

### Pipelining (Batch Operations)

Send multiple commands without waiting for responses to reduce network round trips.
*In redis-cli, use the `--pipe` flag. In application code, use the pipeline feature.*

```redis
MULTI
SET user:2001 "User 2001"
SET user:2002 "User 2002"
SET user:2003 "User 2003"
SET user:2004 "User 2004"
SET user:2005 "User 2005"
EXEC
```

### Pub/Sub (Real-Time Notifications)

**Terminal 1 (Subscriber):**

```redis
SUBSCRIBE user:1001:notifications
```

**Terminal 2 (Publisher):**

```redis
PUBLISH user:1001:notifications "Your course has been approved!"
```

### Data Persistence Check

Save current state to disk:

```redis
SAVE
```

Get last save time:

```redis
LASTSAVE
```

Enable background save:

```redis
BGSAVE
```

Check persistence configuration:

```redis
CONFIG GET save
CONFIG GET appendonly
```

### Performance Monitoring

Get server information:

```redis
INFO
```

Monitor commands in real-time:

```redis
MONITOR
```

Get statistics:

```redis
INFO stats
```

Get memory usage:

```redis
INFO memory
MEMORY USAGE user:1001:login_events
```

-----

## 6\. Group Contribution Summary

### Optional: Remove Test Data

**CAUTION:** Delete all keys in the current database:

```redis
FLUSHDB
```

Or delete specific patterns:

```redis
DEL user:1001:name user:1001:bio cart:user:1001
```

Exit Redis CLI:

```redis
EXIT
```

### Stop Docker Container

```bash
docker stop redis-lab
```

### Key Takeaways

1.  **Use Meaningful Key Names:** Follow naming conventions (e.g., `object:id:field`).
2.  **Set Appropriate TTLs:** Prevent memory bloat with expiration.
3.  **Monitor Memory Usage:** Use `INFO memory` regularly.
4.  **Use Hashes for Objects:** More memory efficient than multiple string keys.
5.  **Implement Application-Level Validation:** Redis doesn't enforce schemas.

### Team Members and Responsibilities

| Student ID | Member Name | Contribution |
| :--- | :--- | :--- |
| 112721 | Kathembo Tsongo | Setup Instructions, Docker Configuration |
| 226022 | Kavira Neema Nancy | CRUD operations |
| 136371 | Joseph Vunanga | Applied Scenario Implementation |
| 225637 | Olang Sharon Leah | Advanced Operations |

-----

## Appendix: Troubleshooting

### Common Issues

**1. Cannot connect to Redis:**
Check if container is running:

```bash
docker ps
```

Check logs:

```bash
docker logs redis-lab
```

Restart container:

```bash
docker restart redis-lab
```

**2. Connection refused error:**
Verify port mapping:

```bash
docker port redis-lab
```

Check firewall settings on Ubuntu:

```bash
sudo ufw status
```

**3. Out of memory errors:**
Check memory usage:

```redis
INFO memory
```

Clear unnecessary data:

```redis
FLUSHDB
```

Configure max memory:

```redis
CONFIG SET maxmemory 256mb
CONFIG SET maxmemory-policy allkeys-lru
```

-----

## References

**Further Learning Materials:**

1.  [Official Documentation](https://redis.io/docs/)
2.  [Redis University: Free courses](https://university.redis.com/)
3.  [Redis Commands Reference](https://redis.io/commands/)
4.  [Try Redis Online](https://try.redis.io/)
