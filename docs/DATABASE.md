# Database Documentation

## Overview

The Citizen Complaints and Engagement System uses MySQL as its primary database management system. This document outlines the database schema, relationships, and important considerations for developers working with the data layer.

## Database Configuration

- **Database Name**: `citizen_complaints_db`
- **Character Set**: `utf8mb4`
- **Collation**: `utf8mb4_unicode_ci`

## Database Versioning

### Version Control
- All schema changes must be versioned
- Use migration scripts for all DDL changes
- Include rollback scripts for each migration
- Document schema changes in CHANGELOG.md

### Change Management
1. Create migration script in `database/migrations`
2. Test migration in development
3. Peer review changes
4. Apply to staging
5. Schedule production deployment

## Entity-Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|     Users      |       |   Complaints   |       |   Categories   |
+----------------+       +----------------+       +----------------+
| PK: id         |------>| FK: user_id    |       | PK: id         |
| username       |       | FK: category_id |<------| name          |
| email          |       | subject        |       | description    |
| password       |       | description    |       | department_id  |
| phone          |       | location       |       +----------------+
| address        |       | status         |              |
| user_type      |       | priority       |              |
| created_at     |       | created_at     |              V
| updated_at     |       | updated_at     |       +----------------+
+----------------+       +----------------+       |  Departments   |
                                |                +----------------+
                                |                | PK: id         |
                                v                | name           |
                        +----------------+       | description    |
                        |   Responses    |       | email          |
                        +----------------+       | phone          |
                        | PK: id         |       | created_at     |
                        | FK: complaint_id|       | updated_at     |
                        | FK: staff_id    |       +----------------+
                        | response_text  |              |
                        | created_at     |              |
                        | updated_at     |              V
                        +----------------+       +----------------+
                                |                |     Staff      |
                                |                +----------------+
                                +--------------->| PK: id         |
                                                 | FK: department_id|
                                                 | name          |
                                                 | email         |
                                                 | password      |
                                                 | phone         |
                                                 | position      |
                                                 | created_at    |
                                                 | updated_at    |
                                                 +----------------+
                                                        |
                                                        |
                                                        V
                                                 +----------------+
                                                 |   Attachments  |
                                                 +----------------+
                                                 | PK: id         |
                                                 | FK: complaint_id|
                                                 | file_name      |
                                                 | file_path      |
                                                 | file_type      |
                                                 | file_size      |
                                                 | uploaded_at    |
                                                 +----------------+
```

## Table Descriptions

### 1. Users

Stores information about citizens who register to submit complaints.

| Field       | Type         | Constraints      | Description                              |
|-------------|--------------|------------------|------------------------------------------|
| id          | INT          | PK, AUTO_INCREMENT | Unique identifier for the user           |
| username    | VARCHAR(50)  | UNIQUE, NOT NULL | User's chosen username                   |
| email       | VARCHAR(100) | UNIQUE, NOT NULL | User's email address                     |
| password    | VARCHAR(255) | NOT NULL         | Encrypted password                       |
| phone       | VARCHAR(20)  | NULL             | Contact phone number                     |
| address     | TEXT         | NULL             | User's address                           |
| user_type   | ENUM         | NOT NULL         | 'citizen', 'admin', 'agency_staff'        |
| created_at  | TIMESTAMP    | NOT NULL         | Timestamp of account creation            |
| updated_at  | TIMESTAMP    | NULL             | Timestamp of last account update         |

### 2. Departments

Government departments or agencies that handle complaints.

| Field       | Type         | Constraints      | Description                              |
|-------------|--------------|------------------|------------------------------------------|
| id          | INT          | PK, AUTO_INCREMENT | Unique identifier for the department     |
| name        | VARCHAR(100) | UNIQUE, NOT NULL | Department name                          |
| description | TEXT         | NULL             | Description of department's responsibilities |
| email       | VARCHAR(100) | NOT NULL         | Department contact email                 |
| phone       | VARCHAR(20)  | NULL             | Department contact phone                 |
| created_at  | TIMESTAMP    | NOT NULL         | Timestamp of creation                    |
| updated_at  | TIMESTAMP    | NULL             | Timestamp of last update                 |

### 3. Staff

Government employees who respond to complaints.

| Field         | Type         | Constraints      | Description                              |
|---------------|--------------|------------------|------------------------------------------|
| id            | INT          | PK, AUTO_INCREMENT | Unique identifier for the staff member   |
| department_id | INT          | FK, NOT NULL     | Department the staff belongs to          |
| name          | VARCHAR(100) | NOT NULL         | Staff member's name                      |
| email         | VARCHAR(100) | UNIQUE, NOT NULL | Staff email address                      |
| password      | VARCHAR(255) | NOT NULL         | Encrypted password                       |
| phone         | VARCHAR(20)  | NULL             | Contact phone number                     |
| position      | VARCHAR(100) | NULL             | Job title or position                    |
| created_at    | TIMESTAMP    | NOT NULL         | Timestamp of account creation            |
| updated_at    | TIMESTAMP    | NULL             | Timestamp of last account update         |

### 4. Categories

Categories for classifying complaints.

| Field         | Type         | Constraints      | Description                              |
|---------------|--------------|------------------|------------------------------------------|
| id            | INT          | PK, AUTO_INCREMENT | Unique identifier for the category       |
| name          | VARCHAR(100) | UNIQUE, NOT NULL | Category name                            |
| description   | TEXT         | NULL             | Description of the category              |
| department_id | INT          | FK, NOT NULL     | Department responsible for this category |
| created_at    | TIMESTAMP    | NOT NULL         | Timestamp of creation                    |
| updated_at    | TIMESTAMP    | NULL             | Timestamp of last update                 |

### 5. Complaints

Stores the complaints submitted by citizens.

| Field        | Type         | Constraints      | Description                              |
|--------------|--------------|------------------|------------------------------------------|
| id           | INT          | PK, AUTO_INCREMENT | Unique identifier for the complaint      |
| user_id      | INT          | FK, NOT NULL     | ID of the user who submitted             |
| category_id  | INT          | FK, NOT NULL     | Category of the complaint                |
| subject      | VARCHAR(255) | NOT NULL         | Brief subject line                       |
| description  | TEXT         | NOT NULL         | Detailed description                     |
| location     | TEXT         | NULL             | Location related to the complaint        |
| status       | ENUM         | NOT NULL         | 'new', 'in_progress', 'resolved', 'closed' |
| priority     | ENUM         | NOT NULL         | 'low', 'medium', 'high', 'urgent'        |
| created_at   | TIMESTAMP    | NOT NULL         | Submission timestamp                     |
| updated_at   | TIMESTAMP    | NULL             | Last update timestamp                    |

### 6. Responses

Responses from staff to complaints.

| Field        | Type         | Constraints      | Description                              |
|--------------|--------------|------------------|------------------------------------------|
| id           | INT          | PK, AUTO_INCREMENT | Unique identifier for the response       |
| complaint_id | INT          | FK, NOT NULL     | Complaint being responded to             |
| staff_id     | INT          | FK, NOT NULL     | Staff member who responded               |
| response_text| TEXT         | NOT NULL         | Content of the response                  |
| created_at   | TIMESTAMP    | NOT NULL         | Response timestamp                       |
| updated_at   | TIMESTAMP    | NULL             | Last update timestamp                    |

### 7. Attachments

Files attached to complaints.

| Field        | Type         | Constraints      | Description                              |
|--------------|--------------|------------------|------------------------------------------|
| id           | INT          | PK, AUTO_INCREMENT | Unique identifier for the attachment     |
| complaint_id | INT          | FK, NOT NULL     | Associated complaint                     |
| file_name    | VARCHAR(255) | NOT NULL         | Original filename                        |
| file_path    | VARCHAR(255) | NOT NULL         | Path to the stored file                  |
| file_type    | VARCHAR(100) | NOT NULL         | MIME type                                |
| file_size    | INT          | NOT NULL         | File size in bytes                       |
| uploaded_at  | TIMESTAMP    | NOT NULL         | Upload timestamp                         |

### 8. Complaint Assignments

Tracks which staff members are assigned to which complaints.

| Field        | Type         | Constraints      | Description                              |
|--------------|--------------|------------------|------------------------------------------|
| id           | INT          | PK, AUTO_INCREMENT | Unique identifier for the assignment     |
| complaint_id | INT          | FK, NOT NULL     | Complaint being assigned                 |
| staff_id     | INT          | FK, NOT NULL     | Staff member assigned to the complaint   |
| assigned_at  | TIMESTAMP    | NOT NULL         | When the assignment was made             |
| assigned_by  | INT          | FK, NULL         | ID of user who made the assignment (optional) |
| is_primary   | BOOLEAN      | NOT NULL         | Whether this staff is primary handler    |
| notes        | TEXT         | NULL             | Optional notes about the assignment      |

## Database Relationships

1. **Users to Complaints**: One-to-Many
   - A user can submit multiple complaints
   - Each complaint belongs to one user

2. **Categories to Complaints**: One-to-Many
   - A category can have multiple complaints
   - Each complaint belongs to one category

3. **Departments to Categories**: One-to-Many
   - A department can handle multiple categories
   - Each category is assigned to one department
   - This relationship is key for routing complaints to the correct agency

4. **Departments to Staff**: One-to-Many
   - A department can have multiple staff members
   - Each staff member belongs to one department
   - Agency staff users are associated with their respective departments

5. **Complaints to Responses**: One-to-Many
   - A complaint can have multiple responses
   - Each response belongs to one complaint

6. **Staff to Responses**: One-to-Many
   - A staff member can create multiple responses
   - Each response is created by one staff member
   - This allows tracking which agency staff responded to which complaints

7. **Complaints to Attachments**: One-to-Many
   - A complaint can have multiple attachments
   - Each attachment belongs to one complaint

8. **Departments to Complaints**: Indirect Many-to-Many (through categories)
   - Complaints are routed to departments based on their category
   - This allows each agency to see all complaints assigned to them

9. **Staff to Complaints**: Many-to-Many (through assignments)
   - Specific staff members within an agency can be assigned to handle specific complaints
   - This enables appropriate workload distribution within agencies

## Sample SQL Commands

### Creating the Database

```sql
CREATE DATABASE citizen_complaints_db
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

### Sample Table Creation

```sql
USE citizen_complaints_db;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    user_type ENUM('citizen', 'admin') NOT NULL DEFAULT 'citizen',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP
);

-- Additional tables would follow the same pattern
```

## Backup and Restore Procedures

### Creating a Backup

```bash
# From the command line:
mysqldump -u [username] -p citizen_complaints_db > backup.sql

# From phpMyAdmin:
# 1. Select the database
# 2. Click on "Export" tab
# 3. Choose "Custom" export method
# 4. Select SQL format
# 5. Click "Go"
```

### Restoring from Backup

```bash
# From the command line:
mysql -u [username] -p citizen_complaints_db < backup.sql

# From phpMyAdmin:
# 1. Select the database
# 2. Click on "Import" tab
# 3. Choose the backup file
# 4. Click "Go"
```

## Data Management Rules

### Data Validation
- All input must be validated before database operations
- Use database constraints (NOT NULL, UNIQUE, CHECK)
- Validate data types and formats at the application level
- Implement proper error handling for validation failures

### Query Optimization
1. **Indexing Strategy**
   - Index all foreign keys
   - Add composite indexes for common query patterns
   - Avoid over-indexing write-heavy tables
   - Use EXPLAIN to analyze query performance

2. **Query Patterns**
   - Use prepared statements exclusively
   - Limit result sets with pagination
   - Select only required columns
   - Avoid SELECT * in production queries
   - Use JOINs instead of subqueries when possible

### Transaction Management
- Use transactions for related operations
- Keep transactions as short as possible
- Implement proper error handling and rollback
- Set appropriate isolation levels
- Avoid long-running transactions

## Performance Guidelines

### Table Design
- Normalize data to 3NF (Third Normal Form)
- Use appropriate data types (e.g., ENUM for status fields)
- Consider denormalization for read-heavy operations
- Implement proper primary and foreign key constraints

### Maintenance
- Regular database optimization (weekly)
- Monitor and remove orphaned records
- Archive old data based on retention policies (see [SECURITY.md](./SECURITY.md))
- Regular backup and recovery testing

## Security Measures

### Access Control
- Follow principle of least privilege
- Use separate database users for different application roles
- Implement row-level security where needed
- Regularly audit user permissions

### Data Protection
- Encrypt sensitive data at rest
- Implement field-level encryption for PII
- Mask sensitive data in logs
- Regular security audits of database configurations

## Monitoring & Maintenance

### Performance Monitoring
- Monitor slow queries
- Track database growth
- Monitor connection pool usage
- Set up alerts for abnormal patterns

### Backup Strategy
- Daily full backups
- Transaction log backups every 15 minutes
- Test restore procedures monthly
- Off-site backup storage
- Documented recovery procedures

4. **Data Validation**: Always validate data before inserting or updating.

5. **Error Handling**: Implement proper error handling and logging for database operations.

6. **Regular Backups**: Schedule regular backups of the database.

7. **Soft Deletes**: Consider implementing soft deletes (marking records as deleted rather than actually deleting them) for important data.

---

Last updated: May 16, 2025
