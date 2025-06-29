# Insurance_Mini_Project

## 💼 Core Functionalities
- Admin Login (Optional security)

- Add Customer

- Add Policy for a Customer

- View All Policies

- Search Policy by Customer ID

- Delete Policy

- Dashboard showing summary (optional)

## SQL tables
### 🧾 Table: customer
| Column  | Data Type    | Constraints                  | Description                 |
| ------- | ------------ | ---------------------------- | --------------------------- |
| id      | INT          | PRIMARY KEY, AUTO_INCREMENT | Unique ID for each customer |
| name    | VARCHAR(100) | NOT NULL                     | Full name of the customer   |
| contact | VARCHAR(20)  |                              | Mobile number               |
| email   | VARCHAR(100) |                              | Email address               |

```sql
CREATE TABLE customer (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    contact VARCHAR(20),
    email VARCHAR(100)
);
```
### 🧾 Table: policy
```sql
| Column       | Data Type     | Constraints                  | Description                           |
| ------------ | ------------- | ---------------------------- | ------------------------------------- |
| id           | INT           | PRIMARY KEY, AUTO_INCREMENT | Unique ID for each policy             |
| policy_type | VARCHAR(50)   | NOT NULL                     | Type of insurance (e.g. Life, Health) |
| premium      | DECIMAL(10,2) | NOT NULL                     | Premium amount                        |
| customer_id | INT           | FOREIGN KEY → customer(id)   | References customer table             |

```
### 🧾 Create Table: policy
```sql
CREATE TABLE policy (
    id INT PRIMARY KEY AUTO_INCREMENT,
    policy_type VARCHAR(50) NOT NULL,
    premium DECIMAL(10, 2) NOT NULL,
    customer_id INT NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customer(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

## 🔁 Relationships
- Each policy belongs to one customer.

- One customer can have multiple policies.

- Deleting a customer will also delete all their policies (ON DELETE CASCADE).

  🔍 Sample Data Insertion
  ```sql
  -- Insert sample customers
INSERT INTO customer (name, contact, email)
VALUES 
('John Doe', '9876543210', 'john@example.com'),
('Jane Smith', '9123456780', 'jane@example.com');

-- Insert sample policies
INSERT INTO policy (policy_type, premium, customer_id)
VALUES 
('Life Insurance', 12000.00, 1),
('Health Insurance', 8000.50, 2);
```

