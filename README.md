# Distributed Database Management System (DDBMS)

A fully-featured **Distributed Database Management System** built with Java, simulating multi-site data processing and storage across two AWS virtual machine hosts connected via the JSch library.

---

## Overview

A distributed database is a collection of multiple interconnected databases spread across a digital network, sharing no physical components. This DDBMS enables users to securely register and authenticate, execute SQL queries, manage transactions, export data structures, analyze usage, and visualize entity-relationship diagrams — all across a distributed, multi-node architecture.

---

## Tech Stack

| Component | Technology |
|---|---|
| Language | Java |
| Remote Connectivity | JSch (Java Secure Channel) |
| Infrastructure | AWS EC2 Virtual Machines (2 hosts) |
| Architecture | Multi-site processing & multi-site data storage |

---

## Modules

### Module 1 — DB Design
Manages both **global** and **local metadata** across distributed nodes. Handles schema definitions, table structures, and inter-node catalog synchronization to maintain a consistent view of the distributed database.

### Module 2 — Query Implementation
A **query executor** that parses and executes common SQL statements (`SELECT`, `INSERT`, `UPDATE`, `DELETE`, `CREATE`, `DROP`) across distributed sites, routing queries to the appropriate host based on data locality.

### Module 3 — Transaction Processing
A **transaction manager** that enforces ACID properties across distributed nodes. Supports multi-site transaction coordination, commit/rollback handling, and concurrency control to ensure data consistency.

### Module 4 — Log Management
Tracks and persists a complete **transaction and event log** for the system. All queries, logins, errors, and system events are recorded with timestamps and user context, enabling audit trails and recovery support.

### Module 5 — Data Modelling – Reverse Engineering
Generates an **Entity-Relationship Diagram (ERD)** by reverse-engineering the existing database schema. Introspects tables, columns, primary keys, and foreign key relationships to produce a visual data model of the system.

### Module 6 — Export Structure and Values
Provides a **dump/export utility** that exports both table structures (DDL `CREATE` statements) and data values (`INSERT` statements) from any node. Supports generating portable SQL dumps for backup, migration, or replication purposes.

### Module 7 — Analytics
Tracks **per-user query analytics**, including total queries executed and a breakdown by query type (SELECT, INSERT, UPDATE, DELETE, etc.). Allows users and administrators to monitor usage patterns and system activity over time.

### Module 8 — User Interface & Login Security
Provides a **secure user interface** for registration and login. Credentials are encrypted before storage, enforcing security at the authentication layer. Users are registered into the DDBMS following defined security protocols, with session management handled throughout the application lifecycle.

---

## Key Features

- **Distributed Architecture** — Data and processing are split across two AWS-hosted nodes, simulating a real-world distributed environment.
- **Secure Authentication** — Passwords and credentials are encrypted; users must register and authenticate before interacting with the system.
- **Full SQL Support** — Execute standard SQL queries with transparent distribution across sites.
- **ACID Transactions** — Multi-site transaction management with rollback support.
- **Event Logging** — Comprehensive logging of all system events and user actions.
- **ERD Generation** — Automatic reverse-engineered diagrams from live schema data.
- **Export/Dump Utility** — Export both schema structure and data values for any table or database.
- **Usage Analytics** — Per-user query statistics broken down by operation type.

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Client / UI Layer                 │
│             (Login, Query Input, Analytics)         │
└───────────────────────┬─────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────┐
│              DDBMS Core (Java Application)          │
│  ┌──────────────┐  ┌───────────┐  ┌──────────────┐  │
│  │ Query Engine │  │Transaction│  │  Log Manager │  │
│  └──────────────┘  │  Manager  │  └──────────────┘  │
│  ┌──────────────┐  └───────────┘  ┌──────────────┐  │
│  │Global Catalog│                 │  Analytics   │  │
│  └──────────────┘                 └──────────────┘  │
└───────────────────────┬─────────────────────────────┘
                        │  JSch SSH Connection
          ┌─────────────┴─────────────┐
          │                           │
┌─────────▼──────────┐   ┌────────────▼──────────┐
│   AWS Host / Node 1│   │   AWS Host / Node 2   │
│   (Local DB + Meta)│   │   (Local DB + Meta)   │
└────────────────────┘   └───────────────────────┘
```

---

## Getting Started

### Prerequisites

- Java 11+
- JSch library (`jsch-0.1.55.jar` or later)
- Two AWS EC2 instances (or equivalent VMs) with SSH access
- A database engine installed on each node (e.g., MySQL / PostgreSQL)

### Configuration

1. Clone the repository:
   ```bash
   git clone https://github.com/your-org/distributed-dbms.git
   cd distributed-dbms
   ```

2. Configure host connection details in `config.properties`:
   ```properties
   host1.ip=<AWS_HOST_1_IP>
   host2.ip=<AWS_HOST_2_IP>
   ssh.user=ec2-user
   ssh.key.path=/path/to/your/private-key.pem
   ```

3. Build the project:
   ```bash
   javac -cp .:lib/jsch.jar src/**/*.java -d out/
   ```

4. Run the application:
   ```bash
   java -cp out/:lib/jsch.jar Main
   ```

---

## Usage

1. **Register** a new account with a username and password (credentials are encrypted).
2. **Login** using your registered credentials.
3. **Execute SQL queries** via the query interface — the system routes operations to the correct distributed node.
4. **View analytics** to see your query history and usage breakdown.
5. **Export** table structures or data using the dump utility.
6. **Generate an ERD** from the current schema via the data modelling module.
7. **Inspect logs** for a full audit trail of all system events.

---

## Security

- User passwords are **encrypted at rest** before being stored in the system.
- All inter-node communication is secured via **SSH tunnelling** using JSch.
- Session management ensures authenticated access to all modules.

---

