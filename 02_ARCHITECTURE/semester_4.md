# 📌 Semester 4 – Core CS II (Operating Systems, Networks, Databases)

> **The Systems Phase**: Understand how computers actually work at the OS level, how networks communicate, and how data persists

---

## 🎯 Goals

- Understand **how operating systems manage resources**
- Learn **how the internet and networks function**
- Master **database design and querying**
- Build **full-stack applications** with proper data persistence
- Ship **1-2 systems-level projects**

---

## 📚 University Courses (Typical)

- Operating Systems
- Computer Networks
- Database Systems / Database Management
- Software Engineering or Elective

---

## 🛠 Parallel Track (Self-Directed)

Dedicate **4-6 hours per week** to hands-on systems labs alongside coursework.

---

## 📖 Part I: Operating Systems (Weeks 1-5)

### Core Concepts

#### Week 1-2: Processes & Threads

**What You'll Learn:**
- Process lifecycle and states
- Process vs Thread
- Context switching
- Process scheduling algorithms
- Inter-process communication (IPC)

**Hands-On Labs:**

```c
// Lab 1: Fork and process creation
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid = fork();
    
    if (pid == 0) {
        // Child process
        printf("Child process: PID = %d\n", getpid());
        printf("Child's parent: PID = %d\n", getppid());
    } else {
        // Parent process
        printf("Parent process: PID = %d\n", getpid());
        printf("Created child: PID = %d\n", pid);
    }
    
    return 0;
}
```

```python
# Lab 2: Multi-threading in Python
import threading
import time

def worker(num):
    """Thread worker function"""
    print(f"Worker {num} starting")
    time.sleep(2)
    print(f"Worker {num} finished")

# Create threads
threads = []
for i in range(5):
    t = threading.Thread(target=worker, args=(i,))
    threads.append(t)
    t.start()

# Wait for all threads to complete
for t in threads:
    t.join()

print("All workers finished")
```

**Terminal Commands:**

```bash
# View running processes
ps aux | grep <process_name>

# View process tree
pstree

# View system resource usage
top
htop  # better alternative

# Kill a process
kill <PID>
kill -9 <PID>  # force kill

# View all processes by user
ps -u username

# Background and foreground jobs
./long_running_script &  # Run in background
jobs  # List background jobs
fg %1  # Bring job 1 to foreground
bg %1  # Resume job 1 in background
```

**Understanding:**
- Process = independent program in execution (own memory space)
- Thread = lightweight process (shares memory with parent)
- Context switch = OS saves/restores state when switching between processes

---

#### Week 3-4: Memory Management

**What You'll Learn:**
- Virtual memory
- Paging and segmentation
- Stack vs Heap
- Memory allocation algorithms
- Memory leaks and garbage collection

**Hands-On Labs:**

```c
// Lab 1: Stack vs Heap allocation
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Stack allocation (automatic)
    int stack_var = 10;
    int stack_array[5] = {1, 2, 3, 4, 5};
    
    printf("Stack variable address: %p\n", (void*)&stack_var);
    printf("Stack array address: %p\n", (void*)stack_array);
    
    // Heap allocation (manual)
    int *heap_var = (int*)malloc(sizeof(int));
    int *heap_array = (int*)malloc(5 * sizeof(int));
    
    *heap_var = 20;
    for(int i = 0; i < 5; i++) {
        heap_array[i] = i + 10;
    }
    
    printf("Heap variable address: %p\n", (void*)heap_var);
    printf("Heap array address: %p\n", (void*)heap_array);
    
    // Always free heap memory!
    free(heap_var);
    free(heap_array);
    
    return 0;
}
```

```python
# Lab 2: Memory profiling in Python
import sys

# Check object size
x = [1, 2, 3, 4, 5]
print(f"List size: {sys.getsizeof(x)} bytes")

y = {"a": 1, "b": 2}
print(f"Dict size: {sys.getsizeof(y)} bytes")

# Memory leak demonstration
def memory_leak():
    big_list = []
    while True:
        big_list.append([0] * 1000000)  # Keeps growing!
        # Never freed = memory leak

# Don't actually run this!
# memory_leak()
```

**Terminal Commands:**

```bash
# View memory usage
free -h

# View memory usage by process
ps aux --sort=-%mem | head -10

# Monitor memory in real-time
vmstat 1

# Check for memory leaks (with valgrind)
valgrind --leak-check=full ./your_program
```

**Key Concepts:**
- **Stack**: Fast, fixed size, automatic deallocation (local variables)
- **Heap**: Slow, dynamic size, manual management (malloc/free, new/delete)
- **Virtual Memory**: Each process thinks it has all memory available
- **Page Fault**: Occurs when accessing unmapped memory page

---

#### Week 5: File Systems

**What You'll Learn:**
- File system structure (inode, directories, files)
- File permissions and ownership
- Hard links vs symbolic links
- File I/O operations

**Hands-On Labs:**

```bash
# Lab 1: File system exploration

# View file system type
df -T

# View disk usage
du -sh /home/user/*

# View inode information
ls -li  # Shows inode number

# Create hard link
ln file.txt hardlink.txt

# Create symbolic link
ln -s file.txt symlink.txt

# Check file type
file <filename>

# View file permissions in detail
stat <filename>
```

```c
// Lab 2: File I/O in C
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main() {
    // Open file
    int fd = open("test.txt", O_WRONLY | O_CREAT, 0644);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    
    // Write to file
    const char *text = "Hello from OS lab!\n";
    write(fd, text, strlen(text));
    
    // Close file
    close(fd);
    
    // Read file
    fd = open("test.txt", O_RDONLY);
    char buffer[100];
    ssize_t bytes_read = read(fd, buffer, sizeof(buffer));
    buffer[bytes_read] = '\0';
    printf("Read: %s", buffer);
    
    close(fd);
    return 0;
}
```

**Key Concepts:**
- **Inode**: Data structure storing file metadata (size, permissions, timestamps)
- **Hard Link**: Direct reference to inode (same file, multiple names)
- **Symbolic Link**: Pointer to filename (can break if original deleted)

---

## 📖 Part II: Computer Networks (Weeks 6-10)

### Core Concepts

#### Week 6-7: Network Fundamentals & TCP/IP

**What You'll Learn:**
- OSI Model vs TCP/IP Model
- IP addressing and subnetting
- TCP vs UDP
- How packets travel across the internet
- DNS and DHCP

**TCP/IP Stack:**

```
Application Layer:  HTTP, FTP, SMTP, DNS
Transport Layer:    TCP, UDP
Network Layer:      IP, ICMP, ARP
Link Layer:         Ethernet, WiFi
```

**Hands-On Labs:**

```bash
# Lab 1: Network diagnostics

# Check your IP address
ip addr show
ifconfig  # older command

# Check routing table
ip route show
route -n

# Ping a host
ping google.com

# Trace route to host
traceroute google.com
mtr google.com  # better alternative

# DNS lookup
nslookup google.com
dig google.com

# View open ports
netstat -tuln
ss -tuln  # modern alternative

# Scan ports (requires nmap)
nmap -p 1-1000 localhost
```

```python
# Lab 2: Simple TCP client
import socket

# Create socket
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to server
client.connect(('example.com', 80))

# Send HTTP request
request = "GET / HTTP/1.1\r\nHost: example.com\r\n\r\n"
client.send(request.encode())

# Receive response
response = client.recv(4096)
print(response.decode())

# Close connection
client.close()
```

**Understanding:**
- **IP Address**: Unique identifier for a device on network (e.g., 192.168.1.10)
- **Port**: Endpoint for communication (e.g., 80 for HTTP, 443 for HTTPS)
- **TCP**: Reliable, ordered, connection-oriented (web browsing, email)
- **UDP**: Fast, connectionless, no guarantees (streaming, gaming)
- **DNS**: Translates domain names to IP addresses

---

#### Week 8-9: Client-Server Architecture & HTTP

**What You'll Learn:**
- HTTP protocol deep dive
- Request methods (GET, POST, PUT, DELETE)
- Status codes and headers
- RESTful API design
- WebSockets

**HTTP Basics:**

```http
# HTTP Request Structure
GET /api/users HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer token123

# HTTP Response Structure
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 85

{"users": [{"id": 1, "name": "John"}]}
```

**Status Codes:**

| Code | Meaning | Example |
|------|---------|---------|
| 2xx | Success | 200 OK, 201 Created |
| 3xx | Redirection | 301 Moved Permanently, 304 Not Modified |
| 4xx | Client Error | 400 Bad Request, 404 Not Found, 401 Unauthorized |
| 5xx | Server Error | 500 Internal Server Error, 503 Service Unavailable |

**Hands-On Labs:**

```python
# Lab 1: HTTP server in Python
from http.server import HTTPServer, BaseHTTPRequestHandler

class SimpleHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        # Send response headers
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()
        
        # Send response body
        response = '<h1>Hello from Python HTTP Server!</h1>'
        self.wfile.write(response.encode())
    
    def do_POST(self):
        # Read request body
        content_length = int(self.headers['Content-Length'])
        body = self.rfile.read(content_length)
        
        print(f"Received POST data: {body.decode()}")
        
        # Send response
        self.send_response(200)
        self.send_header('Content-type', 'text/plain')
        self.end_headers()
        self.wfile.write(b'POST received!')

# Start server
server = HTTPServer(('localhost', 8000), SimpleHandler)
print('Server running on http://localhost:8000')
server.serve_forever()
```

```javascript
// Lab 2: Express.js REST API
const express = require('express');
const app = express();

app.use(express.json());

// In-memory database
let users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
];

// GET all users
app.get('/api/users', (req, res) => {
    res.json(users);
});

// GET user by ID
app.get('/api/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).json({ error: 'User not found' });
    res.json(user);
});

// POST create user
app.post('/api/users', (req, res) => {
    const user = {
        id: users.length + 1,
        name: req.body.name
    };
    users.push(user);
    res.status(201).json(user);
});

// PUT update user
app.put('/api/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).json({ error: 'User not found' });
    
    user.name = req.body.name;
    res.json(user);
});

// DELETE user
app.delete('/api/users/:id', (req, res) => {
    const index = users.findIndex(u => u.id === parseInt(req.params.id));
    if (index === -1) return res.status(404).json({ error: 'User not found' });
    
    users.splice(index, 1);
    res.status(204).send();
});

app.listen(3000, () => {
    console.log('API server running on http://localhost:3000');
});
```

**Testing with curl:**

```bash
# GET request
curl http://localhost:3000/api/users

# POST request
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Charlie"}'

# PUT request
curl -X PUT http://localhost:3000/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice Updated"}'

# DELETE request
curl -X DELETE http://localhost:3000/api/users/2
```

---

#### Week 10: Network Security Basics

**What You'll Learn:**
- HTTPS and TLS/SSL
- Symmetric vs Asymmetric encryption
- Firewalls and port security
- Common vulnerabilities (SQL injection, XSS, CSRF)

**Security Best Practices:**

```javascript
// NEVER do this (SQL injection vulnerable)
app.get('/user', (req, res) => {
    const userId = req.query.id;
    const query = `SELECT * FROM users WHERE id = ${userId}`;
    // Attacker can send: ?id=1 OR 1=1
    db.query(query);  // DANGEROUS!
});

// Always use parameterized queries
app.get('/user', (req, res) => {
    const userId = req.query.id;
    const query = 'SELECT * FROM users WHERE id = ?';
    db.query(query, [userId]);  // SAFE!
});

// Sanitize user input
const validator = require('validator');

app.post('/user', (req, res) => {
    const email = req.body.email;
    
    if (!validator.isEmail(email)) {
        return res.status(400).json({ error: 'Invalid email' });
    }
    
    // Safe to proceed
});
```

---

## 📖 Part III: Databases (Weeks 11-16)

### Core Concepts

#### Week 11-12: Relational Databases & SQL

**What You'll Learn:**
- Database design principles
- Normalization (1NF, 2NF, 3NF)
- Primary keys, foreign keys, indexes
- SQL queries (SELECT, JOIN, GROUP BY)
- Transactions and ACID properties

**Database Design:**

```sql
-- Example: E-commerce database design

-- Users table
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products table
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Orders table
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    status ENUM('pending', 'shipped', 'delivered', 'cancelled') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- Order items (junction table for many-to-many)
CREATE TABLE order_items (
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- Create indexes for better query performance
CREATE INDEX idx_user_email ON users(email);
CREATE INDEX idx_product_name ON products(name);
CREATE INDEX idx_order_user ON orders(user_id);
```

**Essential SQL Queries:**

```sql
-- Basic SELECT
SELECT * FROM users;
SELECT username, email FROM users WHERE user_id = 1;

-- WHERE with conditions
SELECT * FROM products WHERE price > 50 AND stock_quantity > 0;
SELECT * FROM users WHERE email LIKE '%@gmail.com';

-- ORDER BY and LIMIT
SELECT * FROM products ORDER BY price DESC LIMIT 10;

-- JOINS
-- Get all orders with user information
SELECT orders.order_id, users.username, orders.total_amount, orders.status
FROM orders
INNER JOIN users ON orders.user_id = users.user_id;

-- Get order details with products
SELECT 
    o.order_id,
    u.username,
    p.name AS product_name,
    oi.quantity,
    oi.price
FROM orders o
INNER JOIN users u ON o.user_id = u.user_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
INNER JOIN products p ON oi.product_id = p.product_id
WHERE o.order_id = 1;

-- LEFT JOIN (include orders even without items)
SELECT o.order_id, o.total_amount, oi.quantity
FROM orders o
LEFT JOIN order_items oi ON o.order_id = oi.order_id;

-- Aggregation (COUNT, SUM, AVG, MAX, MIN)
SELECT COUNT(*) FROM users;
SELECT AVG(price) FROM products;
SELECT SUM(total_amount) FROM orders WHERE status = 'delivered';

-- GROUP BY
SELECT user_id, COUNT(*) as order_count, SUM(total_amount) as total_spent
FROM orders
GROUP BY user_id
ORDER BY total_spent DESC;

-- HAVING (filter after GROUP BY)
SELECT user_id, COUNT(*) as order_count
FROM orders
GROUP BY user_id
HAVING order_count > 5;

-- Subqueries
SELECT * FROM users
WHERE user_id IN (SELECT user_id FROM orders WHERE total_amount > 100);

-- INSERT
INSERT INTO products (name, description, price, stock_quantity)
VALUES ('Laptop', 'High-performance laptop', 999.99, 50);

-- UPDATE
UPDATE products SET price = 899.99, stock_quantity = 45
WHERE product_id = 1;

-- DELETE
DELETE FROM products WHERE product_id = 1;

-- Transactions
START TRANSACTION;

UPDATE products SET stock_quantity = stock_quantity - 1 WHERE product_id = 1;
INSERT INTO orders (user_id, total_amount) VALUES (1, 99.99);

COMMIT;  -- or ROLLBACK if error
```

---

#### Week 13-14: Database Integration

**What You'll Learn:**
- Connecting applications to databases
- ORM (Object-Relational Mapping)
- Connection pooling
- Database migrations

**Node.js + MySQL:**

```javascript
const mysql = require('mysql2');

// Create connection pool
const pool = mysql.createPool({
    host: 'localhost',
    user: 'root',
    password: 'password',
    database: 'ecommerce',
    waitForConnections: true,
    connectionLimit: 10
});

const promisePool = pool.promise();

// Query examples
async function getUserById(userId) {
    const [rows] = await promisePool.query(
        'SELECT * FROM users WHERE user_id = ?',
        [userId]
    );
    return rows[0];
}

async function createUser(username, email, passwordHash) {
    const [result] = await promisePool.query(
        'INSERT INTO users (username, email, password_hash) VALUES (?, ?, ?)',
        [username, email, passwordHash]
    );
    return result.insertId;
}

async function getUserOrders(userId) {
    const [rows] = await promisePool.query(`
        SELECT o.order_id, o.total_amount, o.status, o.created_at
        FROM orders o
        WHERE o.user_id = ?
        ORDER BY o.created_at DESC
    `, [userId]);
    return rows;
}
```

**Python + SQLite:**

```python
import sqlite3

# Connect to database
conn = sqlite3.connect('ecommerce.db')
cursor = conn.cursor()

# Create table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS users (
        user_id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT UNIQUE NOT NULL,
        email TEXT UNIQUE NOT NULL
    )
''')

# Insert data
cursor.execute(
    'INSERT INTO users (username, email) VALUES (?, ?)',
    ('alice', 'alice@example.com')
)
conn.commit()

# Query data
cursor.execute('SELECT * FROM users WHERE username = ?', ('alice',))
user = cursor.fetchone()
print(user)

# Close connection
conn.close()

# Context manager (better approach)
with sqlite3.connect('ecommerce.db') as conn:
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM users')
    users = cursor.fetchall()
    for user in users:
        print(user)
```

---

#### Week 15-16: NoSQL Basics (MongoDB)

**What You'll Learn:**
- SQL vs NoSQL
- Document-based databases
- When to use NoSQL
- MongoDB CRUD operations

**MongoDB with Node.js:**

```javascript
const { MongoClient } = require('mongodb');

async function main() {
    const uri = 'mongodb://localhost:27017';
    const client = new MongoClient(uri);
    
    try {
        await client.connect();
        const db = client.db('ecommerce');
        const users = db.collection('users');
        
        // Insert
        const result = await users.insertOne({
            username: 'alice',
            email: 'alice@example.com',
            profile: {
                age: 25,
                city: 'New York'
            },
            orders: []
        });
        console.log(`Inserted: ${result.insertedId}`);
        
        // Find
        const user = await users.findOne({ username: 'alice' });
        console.log(user);
        
        // Update
        await users.updateOne(
            { username: 'alice' },
            { $set: { 'profile.age': 26 } }
        );
        
        // Delete
        await users.deleteOne({ username: 'alice' });
        
    } finally {
        await client.close();
    }
}

main();
```

**SQL vs NoSQL:**

| Feature | SQL | NoSQL |
|---------|-----|-------|
| Schema | Fixed, predefined | Flexible, dynamic |
| Scaling | Vertical (bigger server) | Horizontal (more servers) |
| Transactions | Strong ACID | Eventual consistency |
| Best for | Structured, relational data | Unstructured, hierarchical data |
| Examples | MySQL, PostgreSQL, Oracle | MongoDB, Cassandra, Redis |

---

## 📦 Projects (Choose 1-2)

### Project 1: Custom Shell

**Difficulty:** Intermediate  
**Time:** 3-4 weeks

**Requirements:**
- Implement basic shell commands (cd, ls, pwd, echo)
- Process execution (fork/exec)
- Pipe support (|)
- Background processes (&)
- Signal handling (Ctrl+C)

**Skills:** Process management, file I/O, system calls

---

### Project 2: Network Chat Application

**Difficulty:** Intermediate  
**Time:** 3-4 weeks

**Requirements:**
- TCP client-server architecture
- Multiple clients support
- Real-time messaging
- User authentication
- Chat history persistence

**Tech Stack:** Python/Node.js + Sockets + SQLite/MongoDB

---

### Project 3: Full-Stack E-Commerce App

**Difficulty:** Advanced  
**Time:** 5-6 weeks

**Requirements:**
- User authentication (signup/login)
- Product catalog with search
- Shopping cart functionality
- Order management
- Admin dashboard

**Tech Stack:**
- Frontend: React/Vue
- Backend: Node.js + Express
- Database: PostgreSQL/MySQL
- Deployment: Heroku/Railway

---

## ✅ Semester 4 Checklist

- [ ] I understand process vs thread and can explain context switching
- [ ] I know the difference between stack and heap memory
- [ ] I can explain the TCP/IP model and trace packet flow
- [ ] I can build a REST API from scratch
- [ ] I can design a normalized database schema
- [ ] I can write complex SQL queries with JOINs and aggregations
- [ ] I understand when to use SQL vs NoSQL
- [ ] I shipped at least one full-stack project with database integration
- [ ] I can use curl or Postman to test APIs
- [ ] I understand basic network security principles

---

## ⚠️ Common Mistakes

### ❌ **Only learning theory without hands-on practice**
Build servers, databases, and networks. Don't just memorize OSI layers.

### ❌ **Skipping SQL**
"I'll just use an ORM" = you don't understand your own database.

### ❌ **Not deploying**
Your project needs to run on a real server, not just localhost.

### ❌ **Weak database design**
Poor schema design = performance problems and bugs later.

---

## 📖 Recommended Resources

### Books
- *Operating Systems: Three Easy Pieces* (free online)
- *Computer Networking: A Top-Down Approach* by Kurose & Ross
- *Database Design for Mere Mortals* by Michael Hernandez

### Courses
- [CS50's Introduction to Computer Science](https://cs50.harvard.edu/) - includes OS and networks
- **[Operating Systems - Complete Course](https://www.youtube.com/watch?v=Yo2MASx_Kko&list=PLnd7R4Mcw3rLmfW78NJnXXaso1BNB9Kym)** - Recluze ⭐⭐⭐⭐⭐ (Urdu)
  - Live lectures (1+ hour each) with Q&A
  - OS history, processes, threads, memory management, file systems
  - Includes system administration roadmap
- **[Computer Networks Basics - Complete Course](https://www.youtube.com/watch?v=hV34f4GLMxI&list=PLnd7R4Mcw3rLmfW78NJnXXaso1BNB9Kym)** - Recluze ⭐⭐⭐⭐⭐ (English)
  - 8+ lectures with theory + Cisco Packet Tracer practicals
  - OSI model, TCP/UDP/QUIC, routing, IPv6, Ethernet, switches
  - Hands-on labs included
- [Networking Fundamentals](https://www.youtube.com/playlist?list=PLDQaRcbiSnqF5U8ffMgZzS7fq1rHUI3Q8)
- [SQL Tutorial - Full Database Course](https://www.youtube.com/watch?v=HXV3zeQKqGY)
- **[Probability and Statistics for CS - Complete Course](https://www.youtube.com/playlist?list=PLnd7R4Mcw3rLmfW78NJnXXaso1BNB9Kym)** - Recluze ⭐ (Urdu, 25+ lectures)
  - Essential for ML/AI track
  - Data visualization, distributions, entropy, KL-divergence
  - Watch if planning specialization in ML/AI

### Practice
- [SQLBolt](https://sqlbolt.com/) - interactive SQL lessons
- [LeetCode Database Problems](https://leetcode.com/problemset/database/)
- [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/)

---

## 🎯 Success Metric

**You know you're ready for Summer Break 2 when:**
- You can explain how a web request travels from browser to server
- You can design and implement a database for a real application
- You can build and deploy a full-stack app with authentication
- You understand system-level concepts, not just application-level

---

**Next:** [Summer Break 2 – Exploration Sprint](./exploration_sprint.md)

---

*"Systems knowledge separates scripters from engineers."*  
*— The Protocol*
