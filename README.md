# Setting Up RDS to Be Configured to Apache Web Server

This project launches an Amazon EC2 instance running Apache, creates a MariaDB database on Amazon RDS, and connects the database to the web server through the EC2 compute resource setup. It also verifies that the Apache web server responds on its public IP and serves a sample PHP page.

**Skill:** Relational Database setup with Web Server
**Course:** IT 442 Cloud Infrastructure, George Mason University (Lab 6)

## Architecture

Browser → EC2 instance `lab6-web-server` (`t2.micro`, Apache, PHP) → Amazon RDS MariaDB instance `it442-db-lab6` (`db.t3.micro`, port 3306)

## Steps

### 1. Launch the EC2 instance
- Name: `lab6-web-server`
- Instance type: `t2.micro`
- Confirmed the instance ID was issued and the instance state is **Running**
- Recorded the public IPv4 address for use with the web server and RDS

### 2. Verify the web server
- Opened the instance's public IPv4 address in a browser. The Apache default page displayed "It works!"

### 3. Create the RDS database
- Engine: MariaDB
- Creation method: Full configuration
- Template: Sandbox
- Settings:
  - DB instance identifier: `it442-db-lab6`
  - Master username: `it442_user_lab6`
  - Credentials management: Self managed
- Instance configuration:
  - Burstable classes
  - Instance type: `db.t3.micro`
- Connectivity:
  - Connect to an EC2 compute resource: `lab6-web-server`
  - Database authentication: Password authentication
- Additional configuration:
  - Initial database name: `sample_lab6`
  - DB parameter group: `lab6-db-pg`
  - Option group: `default:mariadb-11-8`
  - Automated backup: disabled

### 4. Confirm the database is available
- The database showed **Available** with the Summary status and the Connectivity & security tab listed the endpoint and port 3306.
- I switched the connection method to **Endpoints** to view the endpoint, database name, and port.

### 5. Verify the web server connects to the database
- Opened the sample PHP page on the EC2 instance in a browser. It displayed "Welcome to Lab6 Sample page" and "Please enter your data here."

## Verification

| Check | Result |
|---|---|
| EC2 instance | Running, public IPv4 recorded |
| Apache web server | Returned "It works!" on the public IP |
| RDS database | Created with status Available, MariaDB, `db.t3.micro` |
| RDS to EC2 connection | Set during creation through the EC2 compute resource |
| Sample PHP page | Loaded from the EC2 instance |

## Notes

- Sandbox templates were used instead of the free tier because the lab is for research and education.
- Automated backups were turned off, which is fine for a lab but should be enabled for any production database.
- Credentials are not included in this README. Use a password manager or AWS Secrets Manager for real projects.
- Instance IDs, IP addresses, RDS endpoint, VPC ID, and account number have been redacted.

## Cleanup

Delete the RDS instance (with no final snapshot), then terminate the EC2 instance after grading to avoid charges.
