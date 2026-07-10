# AWS Service Setup Guide: RDS (Relational Database Service)

## 1. Overview
RDS is a managed relational database service — AWS handles backups, patching, and failover, while you get a normal SQL endpoint. During the internship an RDS MySQL instance was launched in a private subnet, connected to from an EC2 instance in the same VPC using the MySQL client, and a sample database + table were created to verify end-to-end connectivity.

## 2. Step-by-Step Setup Guide
1. Open the **RDS** console → **Create database**.
2. Choose **Standard create**. Engine: **MySQL** (community). Version: latest 8.x. Template: **Free tier**.
3. DB instance identifier: `vineet-mysql`. Master username: `admin`. Master password: set a strong password (record it).
4. Instance class: `db.t3.micro`. Storage: 20 GiB gp3.
5. **Connectivity**: VPC = `Vineet-VPC`. Public access: **No**. VPC security group: create new `Vineet-RDS-SG` allowing MySQL (3306) inbound from `Vineet-WebSG`.
6. Availability & durability: Single-AZ for the free tier lab (Multi-AZ is the production choice).
7. Additional configuration: initial database name `internshipdb`. Automated backups: 7 days.
8. **Create database** and wait until status = **Available** (5–10 minutes).
9. Copy the **Endpoint** (e.g., `vineet-mysql.xxxxx.ap-south-1.rds.amazonaws.com`).
10. From the EC2 instance:
    ```
    sudo yum install -y mariadb105
    mysql -h vineet-mysql.xxxxx.rds.amazonaws.com -u admin -p
    ```
11. Inside the MySQL prompt:
    ```
    SHOW DATABASES;
    USE internshipdb;
    CREATE TABLE students (id INT, name VARCHAR(50));
    INSERT INTO students VALUES (1, 'Vineet');
    SELECT * FROM students;
    ```

## 3. Key Configurations Used
* **Engine:** MySQL 8.x, `db.t3.micro`, 20 GiB gp3
* **DB identifier / initial DB:** `vineet-mysql` / `internshipdb`
* **Master user:** `admin`
* **Public access:** No (private subnet)
* **Security group:** `Vineet-RDS-SG` — inbound 3306 from `Vineet-WebSG`
* **Backup retention:** 7 days

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`rds_config.png`** — The **RDS → Databases** page showing `vineet-mysql` with status **Available**, engine MySQL, and the endpoint visible.
* **`rds_active.png`** — The **EC2 SSH terminal** connected to the MySQL prompt showing the successful `SELECT * FROM students;` output with the inserted row. This proves the DB is reachable and working.

*(Optional video: run `mysql -h ... -u admin -p`, log in, and execute the CREATE/INSERT/SELECT sequence.)*
