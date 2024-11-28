# Study Notion Backend (RBAC Implementation)

This repository contains the backend implementation of **Study Notion**, a full-stack e-learning platform. The focus here is on the **Authentication**, **Authorization**, and **Role-Based Access Control (RBAC)** aspects of the backend system. These are designed to ensure secure access to the system's resources for different user roles such as **Admin**, **Instructor**, and **Student**.

## **Overview**

This project demonstrates how a backend system for an educational platform can handle:

1. **Authentication**: Secure login/logout mechanisms with OTP verification.
2. **Authorization**: Granting resource access based on user roles.
3. **Role-Based Access Control (RBAC)**: Enforcing permissions to ensure users only access resources allowed by their role.

Built using **Node.js** and **Express.js**, with **MongoDB** as the database, the project adheres to security best practices and modular design principles.

---

## **Core Features**

### **Authentication System**

- **Secure Registration/Login**:

  - Users (Admin, Instructor, Student) can register and log in using secure mechanisms.
  - Passwords are hashed using **bcrypt.js** for enhanced security.

- **JSON Web Tokens (JWT)**:

  - Upon successful login, a JWT is generated and returned to the client for session management.
  - Tokens are verified before granting access to protected resources.

- **OTP-Based Verification**:
  - OTPs (One-Time Passwords) are generated using **otp-generator** and sent to users for email verification.
  - Only verified accounts can access the platform's features.

---

### **Role-Based Access Control (RBAC)**

- Roles defined for this system:

  - **Admin**: Full access, including managing users and system-wide settings.
  - **Instructor**: Access to create and manage courses.
  - **Student**: Access to enroll and interact with courses.

- Role validation is performed using middleware to verify user roles before granting access to API endpoints.

- Middleware example:
  ```javascript
  const roleMiddleware = (roles) => (req, res, next) => {
    const userRole = req.user.role;
    if (!roles.includes(userRole)) {
      return res.status(403).json({ error: "Access denied" });
    }
    next();
  };
  ```

---

### **APIs Implemented**

1. **User Management**

   - Create, update, and delete user profiles.
   - Role-based permissions applied for access.

2. **Authentication APIs**

   - **POST /login**: Authenticate a user and generate a JWT.
   - **POST /register**: Register a new user (with role-based differentiation).
   - **POST /verify-otp**: Verify OTP for account activation.

3. **Role-Specific APIs**
   - Admin APIs for managing users and system configurations.
   - Instructor APIs for managing courses.
   - Student APIs for course enrollment and interaction.

---

### **Middleware**

- Used to enforce **authentication** and **authorization** on protected routes.
- Example:
  ```javascript
  const authMiddleware = (req, res, next) => {
    const token = req.header("Authorization").replace("Bearer ", "");
    try {
      const decoded = jwt.verify(token, process.env.JWT_SECRET);
      req.user = decoded;
      next();
    } catch (err) {
      res.status(401).json({ error: "Authentication failed" });
    }
  };
  ```

---

## **Tech Stack**

- **Node.js**: Backend runtime environment.
- **Express.js**: Web framework for building APIs.
- **MongoDB**: Database for storing user and role data.
- **JWT**: For session management.
- **bcrypt.js**: For secure password hashing.
- **cors**: For handling cross-origin requests.
- **otp-generator**: For OTP-based user verification.

---

## **How RBAC Is Implemented**

- Each user is assigned a **role** during registration.
- The role determines which API endpoints they can access.
- Middleware checks user roles before processing the request.

---

## **Security Measures**

- Passwords are hashed before storing in the database using **bcrypt.js**.
- **JWTs** are signed with a secret key for secure session handling.
- Only verified users can access the platform using OTP-based verification.
- Sensitive endpoints are protected by middleware that checks authentication and authorization.

---

## **Getting Started**

### **Prerequisites**

- Node.js
- MongoDB

### **Installation**

1. Clone the repository:
   ```bash
   git clone <repository-link>
   ```
2. Navigate to the project directory:
   ```bash
   cd study-notion-backend
   ```
3. Install dependencies:
   ```bash
   npm install
   ```
4. Configure environment variables in a `.env` file:
   ```plaintext
   PORT=5000
   MONGO_URI=<your-mongodb-uri>
   JWT_SECRET=<your-secret-key>
   ```
5. Start the server:
   ```bash
   npm start
   ```

---

## **Testing the System**

Use tools like **Postman** to test the following endpoints:

1. Register a user (Student, Instructor, Admin).
2. Log in with the registered credentials.
3. Access role-specific routes (e.g., Admin routes for managing users).
4. Verify OTP functionality.

---

## **Future Enhancements**

- Integration with third-party OAuth providers.
- Adding rate-limiting to prevent brute force attacks.
- Implementing logging for monitoring suspicious activity.

---

This project showcases a robust implementation of **Authentication**, **Authorization**, and **Role-Based Access Control (RBAC)** in a backend system. Feel free to explore and test it to understand the core principles of securing APIs in a web application.
