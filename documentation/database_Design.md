---

# **Database Design Document**

## **1. Introduction**

### **1.1 Purpose**
This document outlines the database design for the energy savings-based loyalty rewards system, detailing the structure, relationships, and constraints of the database.

### **1.2 Scope**
The document covers the database schema, including tables, relationships, and data types, as well as indexing, performance considerations, and security measures.

### **1.3 Audience**
This document is intended for database administrators, developers, and system architects responsible for implementing and maintaining the database.

---

## **2. Database Overview**

### **2.1 Database Management System (DBMS)**
- **DBMS:** MongoDB (NoSQL)
- **Version:** Latest stable release
- **Hosting:** AWS DynamoDB or MongoDB Atlas

### **2.2 Database Structure**
The database is designed to support the core functionalities of the system, including user management, energy usage tracking, points calculation, and rewards redemption.

---

## **3. Data Model**

### **3.1 Entities and Attributes**

#### **3.1.1 User**
- **Collection Name:** `users`
- **Description:** Stores user information and points balance.
- **Attributes:**
  - `user_id` (UUID, Primary Key): Unique identifier for the user.
  - `name` (String): User's full name.
  - `email` (String, Unique): User's email address.
  - `password` (String): Hashed password.
  - `total_points` (Integer): Total points accumulated by the user.
  - `role` (String): User role (e.g., User, Admin).
  - `created_at` (DateTime): Timestamp when the user was created.
  - `updated_at` (DateTime): Timestamp when the user was last updated.

#### **3.1.2 EnergyUsage**
- **Collection Name:** `energy_usage`
- **Description:** Records energy consumption data by users.
- **Attributes:**
  - `usage_id` (UUID, Primary Key): Unique identifier for the energy usage record.
  - `user_id` (UUID, Foreign Key): Reference to the user's `user_id`.
  - `date` (Date): Date of energy consumption.
  - `energy_consumed` (Float): Amount of energy consumed (in kWh).
  - `energy_type` (String): Type of energy (Renewable or Non-Renewable).
  - `points_earned` (Integer): Points awarded for this usage.
  - `created_at` (DateTime): Timestamp when the record was created.

#### **3.1.3 Reward**
- **Collection Name:** `rewards`
- **Description:** Contains details about rewards available for redemption.
- **Attributes:**
  - `reward_id` (UUID, Primary Key): Unique identifier for the reward.
  - `name` (String): Name of the reward.
  - `description` (String): Description of the reward.
  - `points_required` (Integer): Number of points required to redeem the reward.
  - `inventory` (Integer): Number of available units of the reward.
  - `created_at` (DateTime): Timestamp when the reward was created.
  - `updated_at` (DateTime): Timestamp when the reward was last updated.

#### **3.1.4 Redemption**
- **Collection Name:** `redemptions`
- **Description:** Tracks the history of reward redemptions by users.
- **Attributes:**
  - `redemption_id` (UUID, Primary Key): Unique identifier for the redemption.
  - `user_id` (UUID, Foreign Key): Reference to the user's `user_id`.
  - `reward_id` (UUID, Foreign Key): Reference to the redeemed `reward_id`.
  - `redemption_date` (DateTime): Timestamp when the redemption occurred.
  - `points_redeemed` (Integer): Number of points used for the redemption.

---

### **3.2 Relationships**

#### **3.2.1 User to EnergyUsage**
- **Type:** One-to-Many
- **Description:** A user can have multiple energy usage records.
- **Implementation:** `user_id` in `energy_usage` collection references `user_id` in `users` collection.

#### **3.2.2 User to Redemption**
- **Type:** One-to-Many
- **Description:** A user can have multiple reward redemptions.
- **Implementation:** `user_id` in `redemptions` collection references `user_id` in `users` collection.

#### **3.2.3 Reward to Redemption**
- **Type:** One-to-Many
- **Description:** A reward can be redeemed multiple times.
- **Implementation:** `reward_id` in `redemptions` collection references `reward_id` in `rewards` collection.

---

## **4. Indexing Strategy**

### **4.1 Indexes**

#### **4.1.1 Users Collection**
- **Index on `email`:** To ensure quick lookups and enforce uniqueness.
- **Index on `total_points`:** To optimize queries that involve sorting or filtering by points.

#### **4.1.2 EnergyUsage Collection**
- **Index on `user_id`:** To optimize retrieval of energy usage records for a specific user.
- **Compound Index on `date` and `user_id`:** To efficiently query usage records within a date range for a user.

#### **4.1.3 Rewards Collection**
- **Index on `points_required`:** To optimize filtering and sorting rewards by points required.

#### **4.1.4 Redemptions Collection**
- **Index on `user_id`:** To optimize retrieval of redemption records for a specific user.
- **Index on `reward_id`:** To optimize queries involving specific rewards.

### **4.2 Performance Considerations**
- **Sharding:** Consider sharding the `energy_usage` collection by `user_id` if the volume of data becomes very large.
- **Caching:** Implement caching for frequently accessed data, such as the rewards catalog.

---

## **5. Security and Compliance**

### **5.1 Data Encryption**
- **Encryption at Rest:** All sensitive data (e.g., passwords, user details) is encrypted at rest using AES-256 encryption.
- **Encryption in Transit:** All data transmitted between the client and server is encrypted using TLS/SSL.

### **5.2 Access Control**
- **Role-Based Access Control (RBAC):** Implemented to ensure that only authorized users (e.g., Admins) can perform certain actions, like managing rewards.

### **5.3 Data Backup and Recovery**
- **Daily Backups:** Automated daily backups of all collections.
- **Disaster Recovery:** A disaster recovery plan is in place to restore data in the event of a failure.

---

## **6. Data Migration**

### **6.1 Migration Strategy**
- **Initial Data Load:** Scripts will be developed to load initial data (e.g., existing users, rewards) into the database.
- **Incremental Migrations:** Versioned migration scripts will be created to handle schema changes as the system evolves.

### **6.2 Tools**
- **Migration Tools:** Use MongoDB's native tools or third-party tools like `mongomigrate` for managing database migrations.

---

## **7. Testing and Validation**

### **7.1 Testing Strategy**
- **Unit Testing:** Test individual queries and operations within each collection.
- **Integration Testing:** Ensure that relationships between collections (e.g., user and energy usage) are functioning correctly.
- **Performance Testing:** Load testing to validate indexing strategies and overall database performance.

### **7.2 Data Validation**
- **Schema Validation:** Implement schema validation using MongoDB's built-in schema validation features to enforce data integrity.

---

## **8. Conclusion**

---
