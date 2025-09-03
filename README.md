# 🚀 Redis-X

[![C++](https://img.shields.io/badge/C%2B%2B-17-blue.svg)](https://en.cppreference.com/w/cpp/17)
[![Linux](https://img.shields.io/badge/Platform-Linux-green.svg)](https://www.linux.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen.svg)]()

> A high-performance, Redis-inspired in-memory database built from scratch in C++

Redis-X is a lightweight, production-ready in-memory database that implements core Redis functionality with modern C++ design patterns. Built as a learning project to understand distributed systems and database internals, it features efficient data structures, non-blocking I/O, and concurrent processing capabilities.

## ✨ Features

### 🔑 Basic Key-Value Operations
- **SET/GET/DEL** - Standard string operations with O(1) hash table lookups
- **KEYS** - Pattern-based key enumeration
- **PEXPIRE/PTTL** - Millisecond-precision TTL management with automatic expiration

### 📊 Advanced Data Structures
- **Sorted Sets (ZSET)** - Implemented with AVL trees for O(log n) operations:
  - `ZADD` - Add/update members with scores
  - `ZREM` - Remove members
  - `ZSCORE` - Get member scores
  - `ZQUERY` - Range queries with pagination

### ⚡ Performance & Concurrency
- **Non-blocking I/O** - Event-driven architecture using `poll()`
- **Thread Pool** - Background processing for large data structure operations
- **Progressive Rehashing** - Zero-downtime hash table resizing
- **Efficient Memory Management** - Custom allocators and RAII patterns

### 🏗️ Core Architecture
- **Hash Tables** - Fast O(1) key lookups with collision handling
- **AVL Trees** - Self-balancing binary trees for sorted operations
- **Min-Heap** - Efficient TTL expiration management
- **Linked Lists** - Connection management and idle timeout handling

## 🛠️ Tech Stack

- **Language**: C++17
- **Platform**: Linux (POSIX sockets, pthreads)
- **Networking**: TCP sockets with non-blocking I/O
- **Data Structures**: Custom implementations (hash table, AVL tree, heap)
- **Concurrency**: POSIX threads with mutex/condition variables
- **Build System**: Make/GCC/Clang

## 🚀 Getting Started

### Prerequisites
- Linux/macOS with GCC 7+ or Clang 5+
- POSIX-compliant system (for sockets and threading)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/redis-x.git
   cd redis-x
   ```

2. **Build the server and client**
   ```bash
   # Navigate to the latest implementation
   cd 11-thread-pool-production
   
   # Compile the server
   g++ -std=c++17 -O2 -pthread -o server \
       server.cpp hashtable.cpp avl.cpp zset.cpp heap.cpp thread_pool.cpp
   
   # Compile the client
   g++ -std=c++17 -O2 -o client client.cpp
   ```

3. **Run the server**
   ```bash
   ./server
   # Server starts on port 1234
   ```

4. **Connect with the client**
   ```bash
   ./client
   ```

## 📖 Usage Examples

### Basic Key-Value Operations
```bash
# Set a key-value pair
$ ./client set mykey "Hello World"
(nil)

# Get the value
$ ./client get mykey
(str) Hello World

# Set with TTL (5000ms)
$ ./client pexpire mykey 5000
(int) 1

# Check remaining TTL
$ ./client pttl mykey
(int) 3247

# Delete the key
$ ./client del mykey
(int) 1
```

### Sorted Sets Operations
```bash
# Add members to a sorted set
$ ./client zadd leaderboard 100 "alice"
(int) 1
$ ./client zadd leaderboard 200 "bob"
(int) 1
$ ./client zadd leaderboard 150 "charlie"
(int) 1

# Get member score
$ ./client zscore leaderboard "bob"
(dbl) 200

# Query range (score >= 100, limit 10)
$ ./client zquery leaderboard 100 "" 0 10
(arr) len=6
(str) alice
(dbl) 100
(str) charlie
(dbl) 150
(str) bob
(dbl) 200
(arr) end

# Remove a member
$ ./client zrem leaderboard "alice"
(int) 1
```

### Key Management
```bash
# List all keys
$ ./client keys
(arr) len=2
(str) leaderboard
(str) mykey
(arr) end
```

## 🏛️ Architecture Overview

### Event-Driven Server
Redis-X uses a single-threaded event loop with non-blocking I/O to handle thousands of concurrent connections efficiently:

- **Connection Management**: Each client connection is represented by a `Conn` struct with read/write buffers
- **Event Loop**: Uses `poll()` to monitor socket readiness and process I/O events
- **Request Processing**: Custom binary protocol with pipelining support
- **Timer Management**: Dual-timer system for connection timeouts and TTL expiration

### Data Structure Design
- **Hash Table**: Two-table progressive rehashing for zero-downtime resizing
- **AVL Tree**: Self-balancing for sorted set operations with O(log n) complexity
- **Min-Heap**: Efficient TTL expiration with O(log n) insert/delete operations
- **Memory Safety**: RAII patterns and smart pointer usage throughout

### Concurrency Model
- **Main Thread**: Handles all I/O and request processing
- **Thread Pool**: Background workers for expensive operations (large data structure cleanup)
- **Lock-Free Design**: Minimal locking with careful data structure design

## 🔮 Future Improvements

### 🚀 Production Features
- **Persistence Layer** - AOF (Append-Only File) and RDB snapshots
- **Replication** - Master-slave replication with failover
- **Clustering** - Distributed hash partitioning across nodes
- **Memory Optimization** - Custom memory allocators and compression

### ☁️ Deployment & Scaling
- **Docker Support** - Containerized deployment with health checks
- **AWS Integration** - CloudFormation templates and auto-scaling groups
- **Monitoring** - Prometheus metrics and Grafana dashboards
- **Load Balancing** - Redis Cluster protocol implementation

### 🔧 Developer Experience
- **Redis Protocol** - Full RESP (Redis Serialization Protocol) compatibility
- **Client Libraries** - Python, Node.js, and Go client implementations
- **Benchmarking** - Redis-benchmark compatible performance testing
- **Documentation** - API documentation and architecture deep-dives

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Built with ❤️ for learning distributed systems and database internals**