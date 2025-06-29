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
| id      | INT          | PRIMARY KEY, AUTO\_INCREMENT | Unique ID for each customer |
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
| id           | INT           | PRIMARY KEY, AUTO\_INCREMENT | Unique ID for each policy             |
| policy\_type | VARCHAR(50)   | NOT NULL                     | Type of insurance (e.g. Life, Health) |
| premium      | DECIMAL(10,2) | NOT NULL                     | Premium amount                        |
| customer\_id | INT           | FOREIGN KEY → customer(id)   | References customer table             |

```

