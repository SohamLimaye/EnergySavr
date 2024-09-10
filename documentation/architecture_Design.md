---

# **Architecture Design Document**

## **1. Introduction**

### **1.1 Purpose**
This document provides a detailed architecture design for the energy savings-based loyalty rewards system, which allows users to earn points for saving non-renewable energy and using renewable energy, and redeem those points for rewards.

### **1.2 Scope**
The architecture covers the entire system, including the web application, mobile application, backend services, databases, and third-party integrations.

### **1.3 Audience**
This document is intended for developers, system architects, project managers, and stakeholders involved in the development and deployment of the system.

---

## **2. System Overview**

### **2.1 System Context**
The system comprises a web app and mobile app that interact with backend services via an API Gateway. The backend services manage user authentication, energy usage tracking, points calculation, and reward redemption. The system also integrates with third-party services for energy data validation and notification.

---

## **3. Architectural Design**

### **3.1 Architectural Goals and Constraints**
- **Scalability:** The system should handle a growing number of users and energy usage data.
- **Security:** User data must be protected, with secure authentication and data encryption.
- **Performance:** The system should respond quickly to user actions, particularly during reward redemption.
- **Maintainability:** The architecture should support easy updates and maintenance.

### **3.2 High-Level Architecture**
The system is divided into the following main components:
- **Client-Side:** Web and Mobile applications.
- **Backend Services:** API Gateway, Authentication Service, Energy Management Service, Rewards Management Service, Points Calculation Service.
- **Databases:** User DB, EnergyUsage DB, Rewards DB, Redemption DB.
- **Third-Party Integrations:** External Energy API, Email Service, Notification Service.

---

### **3.3 Component Descriptions**

#### **3.3.1 API Gateway**
- **Purpose:** Acts as the entry point for all client requests, routing them to the appropriate backend services.
- **Technology:** AWS API Gateway, Express.js.

#### **3.3.2 Authentication Service**
- **Purpose:** Handles user registration, login, and authentication.
- **Technology:** Node.js, JWT for authentication, bcrypt for password hashing.

#### **3.3.3 Energy Management Service**
- **Purpose:** Manages the storage and processing of energy usage data.
- **Technology:** Node.js, integration with External Energy API.

#### **3.3.4 Rewards Management Service**
- **Purpose:** Manages the rewards catalog and redemption process.
- **Technology:** Node.js, integration with Email and Notification services.

#### **3.3.5 Points Calculation Service**
- **Purpose:** Calculates points based on energy usage and updates user balances.
- **Technology:** Node.js, connection to User DB and Redemption DB.

### **3.4 Data Flow**
1. **User Interaction:**
   - The user interacts with the web or mobile app to input energy usage data, view points, or redeem rewards.
   - The app sends requests to the API Gateway, which forwards them to the respective backend services.

2. **Energy Usage Tracking:**
   - The Energy Management Service stores the usage data and calculates the points to be awarded.
   - Points Calculation Service updates the user's points balance in the User DB.

3. **Reward Redemption:**
   - When the user redeems a reward, the Rewards Management Service updates the inventory and records the redemption in the Redemption DB.

4. **Notifications:**
   - Upon successful redemption, the system sends an email and/or push notification to the user.

### **3.5 Technology Stack**
- **Frontend:** React.js (Web), React Native (Mobile).
- **Backend:** Node.js with Express.js.
- **Databases:** MongoDB for all database requirements.
- **Authentication:** JWT with bcrypt for password hashing.
- **Hosting and Deployment:** AWS (API Gateway, Lambda, DynamoDB).
- **Third-Party Services:** Twilio (Notifications), SendGrid (Emails), External Energy API.

---

## **4. Detailed Design**

### **4.1 Class Diagrams**
Refer to the Class Diagram in the previous section.

### **4.2 Sequence Diagrams**
Refer to the Sequence Diagram in the previous section.

### **4.3 Data Models**
- **User Model:** Stores user information and points balance.
- **EnergyUsage Model:** Stores energy usage data with references to the user.
- **Reward Model:** Stores reward details like name, description, points required, and inventory.
- **Redemption Model:** Records the reward redemption history.

### **4.4 API Specifications**
- **Authentication:**
  - **POST /register:** Registers a new user.
  - **POST /login:** Logs in a user.
- **Energy Usage:**
  - **POST /energy-usage:** Tracks energy usage data.
  - **GET /points:** Retrieves the user's current points balance.
- **Rewards:**
  - **GET /rewards:** Retrieves the available rewards.
  - **POST /redeem:** Redeems a reward using points.

---

## **5. Deployment and Maintenance**

### **5.1 Deployment Architecture**
- **AWS Services:** API Gateway, Lambda Functions, DynamoDB for NoSQL storage.
- **CI/CD Pipeline:** Integrated using AWS CodePipeline for automated deployments.

### **5.2 Monitoring and Logging**
- **Monitoring:** AWS CloudWatch for monitoring the system's health and performance.
- **Logging:** Centralized logging using AWS CloudWatch Logs.

### **5.3 Backup and Recovery**
- **Database Backups:** Automated daily backups using AWS DynamoDB's backup service.
- **Disaster Recovery Plan:** Detailed plan for recovering from data loss or service outages.

---

## **6. Security Considerations**

### **6.1 Authentication and Authorization**
- **JWT Tokens:** Used for secure user authentication.
- **Role-Based Access Control (RBAC):** Differentiates between user roles (e.g., User, Admin).

### **6.2 Data Encryption**
- **Encryption at Rest:** MongoDB with encrypted storage.
- **Encryption in Transit:** HTTPS/TLS for all communications between clients and servers.

### **6.3 Vulnerability Management**
- **Regular Security Audits:** Automated security scans and manual code reviews.
- **Patch Management:** Regular updates and patches for third-party libraries.

---

## **7. Conclusion**

---
