## Table of Contents

- [Security & Compliance (ISO, GDPR, SOC 2)](#security--compliance-iso-gdpr-soc-2)
  - [Security Risks in DevOps and Mitigation Strategies](#security-risks-in-devops-and-mitigation-strategies)
- [Database & Storage Optimization](#database--storage-optimization)
  - [Optimizing a PostgreSQL Database for Performance](#optimizing-a-postgresql-database-for-performance)
- [Automation & Scripting](#automation--scripting)
  - [Bash Script for Server Setup](#bash-script-for-server-setup)
  - [Bash Script for Log Analysis](#bash-script-for-log-analysis)
- [Disaster Recovery & High Availability](#disaster-recovery--high-availability)
  - [Implementing Backup & Disaster Recovery Strategies](#implementing-backup--disaster-recovery-strategies)
  - [Setting Up Automated Backups in Azure](#setting-up-automated-backups-in-azure)
  - [High Availability Best Practices](#high-availability-best-practices)

---

# 3. Security & Compliance (ISO, GDPR, SOC 2)

## Security Risks in DevOps and Mitigation Strategies

DevOps prioritizes speed and efficiency, but this rapid development pace can introduce security vulnerabilities. Below are three major security risks and their mitigation strategies.

### Risk 1: Insecure Infrastructure as Code (IaC)

**What it is**  
Infrastructure as Code (IaC) enables teams to manage infrastructure through code, but flaws in this code can lead to security risks.

**Impact**
- Unauthorized access
- Data breaches
- System downtime

**Mitigation Strategies**
✔ **Code Reviews** – Conduct security audits before deployment.  
✔ **Automated Security Scanning** – Use scanning tools to detect vulnerabilities.  
✔ **Version Control** – Track changes and enable rollbacks.  
✔ **Least Privilege Principle** – Limit access rights to necessary resources.  
✔ **Compliance Alignment**  
  - **ISO 27001** (A.12.5 – Information System Security)  
  - **GDPR** (Article 32 – Security of Processing)  
  - **SOC 2** (Common Criteria – Security)  

---

### Risk 2: Leaky Secrets Management

**What it is**  
Applications require credentials such as API keys and database passwords, and storing them in plain text poses a security risk.

**Impact**
- Unauthorized access
- Data breaches
- Compromised systems

**Mitigation Strategies**
✔ **Secrets Management Tools** – Use HashiCorp Vault, AWS Secrets Manager, or Azure Key Vault.  
✔ **Environment Variables** – Store secrets outside the codebase.  
✔ **Regular Rotation** – Change credentials periodically.  
✔ **Compliance Alignment**  
  - **ISO 27001** (A.12.1 – Information Security Policies)  
  - **GDPR** (Article 32 – Security of Processing)  
  - **SOC 2** (Common Criteria – Security)  

---

### Risk 3: Vulnerable Dependencies

**What it is**  
Modern applications rely on third-party libraries, which may contain security vulnerabilities.

**Impact**
- Application compromise
- Data breaches
- Denial-of-service (DoS) attacks

**Mitigation Strategies**
✔ **Dependency Scanning** – Use tools like Snyk or OWASP Dependency-Check.  
✔ **Keep Dependencies Updated** – Regularly apply patches and updates.  
✔ **Software Composition Analysis (SCA)** – Analyze dependencies for risks.  
✔ **Compliance Alignment**  
  - **ISO 27001** (A.14.2 – System Development)  
  - **GDPR** (Article 32 – Security of Processing)  

---

# 5. Database & Storage Optimization

## Optimizing a PostgreSQL Database for Performance

### 1. Indexing for Faster Queries
Indexes reduce query execution time by minimizing the number of scanned rows.

**Types of Indexes in PostgreSQL**  
- **B-Tree Index** – Default; best for equality and range queries.  
- **GIN Index** – Optimized for full-text search and JSONB fields.  
- **BRIN Index** – Suitable for large tables with sequential data.  
- **Hash Index** – Best for exact-match queries.  

**Example: Creating an Index for Faster Lookups**
```sql
CREATE INDEX idx_email ON users (email);
SELECT * FROM users WHERE email = 'user@example.com';

✅ Performance Boost: Queries now use the index instead of scanning the full table.

2. Query Optimization

Best Practices
	•	Use EXPLAIN ANALYZE – Identify performance bottlenecks.
	•	**Avoid SELECT *** – Fetch only required columns.
	•	Index Foreign Keys – Optimize JOIN operations.

Example: Optimized JOIN Query

CREATE INDEX idx_orders_user_id ON orders (user_id);
SELECT orders.id, users.name 
FROM orders 
JOIN users ON orders.user_id = users.id 
WHERE users.email = 'user@example.com';

✅ Improvement: Queries execute faster using indexed foreign keys.

Automation & Scripting

Bash Script for Server Setup

This script automates server setup, installs essential packages, and configures a firewall.

#!/bin/bash
USER_NAME="deployuser"
PACKAGES="nginx git docker.io"

echo "Starting server setup..."
sudo apt update && sudo apt upgrade -y
sudo apt install -y $PACKAGES
sudo useradd -m -s /bin/bash -G sudo $USER_NAME
echo "$USER_NAME:password123" | sudo chpasswd
sudo ufw allow 'Nginx Full' && sudo ufw enable
sudo systemctl enable nginx docker && sudo systemctl start nginx docker
echo "Server setup completed!"

Usage:

chmod +x server_setup.sh
sudo ./server_setup.sh

Bash Script for Log Analysis

This script analyzes system logs for errors.

#!/bin/bash
LOG_FILE="/var/log/syslog"
ERROR_PATTERN="error|failed|critical"

echo "Analyzing logs..."
ERROR_COUNT=$(grep -iE "$ERROR_PATTERN" "$LOG_FILE" | wc -l)
echo "Total errors found: $ERROR_COUNT"
grep -iE "$ERROR_PATTERN" "$LOG_FILE" | tail -10

Usage:

chmod +x log_analysis.sh
./log_analysis.sh

Disaster Recovery & High Availability

Implementing Backup & Disaster Recovery Strategies

RTO & RPO: Key Recovery Metrics

Metric	Definition	Example
RTO	Max downtime before recovery	2 hours
RPO	Max acceptable data loss	15 min

Setting Up Automated Backups in Azure

Azure Backup Strategy

Component	Backup Method	Retention	Recovery Time
VMs	Azure Backup	7-30 days	5-15 min
SQL DBs	PITR & LTR	35 days	5-15 min

Enabling Automated Backups:

ALTER DATABASE mydatabase
ADD SECONDARY ON SERVER mysecondaryserver;

High Availability Best Practices

✔ Geo-Replication – Replicate data to secondary regions.
✔ Load Balancers – Distribute traffic across multiple servers.
✔ Auto-Scaling – Adjust resources based on demand.

✅ Result: Near-Zero Downtime & Data Loss
