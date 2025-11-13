# **MediLink Backend – Test Plan**

## **1. Introduction**

This test plan outlines the testing strategy for the **MediLink Backend**, covering core modules such as authentication, authorization, appointments, user management, and doctor–patient workflows. The primary goal is to ensure API reliability, correctness, security, and smooth integration of key features.


## **2. Objectives**

* Validate that all core APIs work as expected.
* Ensure correct behavior of JWT authentication & role-based authorization.
* Verify accuracy of DB operations across all modules.
* Identify bugs early and maintain backend stability.
* Confirm that API responses follow the intended schema.


## **3. Scope**

### **In Scope**

* Unit tests for:

  * User signup/login
  * JWT access & refresh tokens
  * Role-based routes (admin/doctor/patient)
  * CRUD operations for doctors, patients, slots, appointments

### **Out of Scope**

* Frontend tests
* UI/UX validation
* Load/performance testing


## **4. Test Items**

Components to be tested:

* **Authentication Module:** login, signup, password hash, JWT, refresh tokens
* **Authorization Module:** role-based access, middleware validation
* **Users Module:** user creation, role mapping
* **Doctors Module:** profile creation, specialization, chamber details
* **Patients Module**
* **Appointments:** booking, fetching history, cancellation
* **DB CRUD Operations**


## **5. Testing Approach**

### **Unit Testing**

* Framework: `pytest`
* DB: Use an in-memory SQLite test database
* Use dependency overrides for `get_db`
* Mock:

  * JWT tokens
  * User roles
  * External dependencies (if any)

### **Integration Testing**

* Test complete API workflows using FastAPI TestClient.
* Validate status codes, response bodies, and permission handling.

### **Smoke Testing**

* Ensure server starts without errors.


## **6. Test Environment**

* **Language:** Python
* **Framework:** FastAPI
* **Database:** Postgresql test DB for integration
* **Tools:**

  * pytest
  * pytest-cov
  * HTTPX TestClient
* **Dependencies:** Installed from `requirements.txt`



## **7. Test Data**

* Dummy user accounts:

  * Admin
  * Doctor
  * Patient
* Dummy JWT access & refresh tokens
* Mock appointments and slots
* Valid + invalid request payloads
* Boundary and edge-case inputs


## **8. High-Level Test Cases**

### **Authentication**

* User signup success
* Duplicate email rejection
* Login with correct credentials
* Login with wrong password
* Access token generation
* Refresh token rotation
* Expired/Invalid token scenarios

### **Authorization**

* Admin-only endpoint access
* Doctor-only endpoint access
* Unauthorized access (missing/invalid token)
* Forbidden access (insufficient role)

### **Users & Roles**

* Create user
* Assign/update role
* Fetch user profile
* Validation errors

### **Doctors**

* Create/update doctor profile
* Fetch doctor details
* Invalid doctor ID handling

### **Appointments**

* Book appointment
* Prevent booking unavailable slot
* Fetch user appointment history
* Cancel appointment
* Validation for invalid date/slot

### **Common API Behavior**

* 400 Bad Request
* 401 unauthorized
* 403 forbidden
* 404 not found
* 500 internal errors for edge cases


## **9. Exit Criteria**

Testing will be considered complete when:

* All critical & high-priority tests pass
* No blocking or major bugs remain
* Code coverage is **≥ 70%** or project-defined threshold
* All APIs deliver stable and consistent responses


## **10. Risks & Mitigation**

| Risk                             | Mitigation                              |
| -------------------------------- | --------------------------------------- |
| Database failure                 | Use isolated test DB                    |
| Token expiry during long tests   | Mock expiry or override timers          |
| Unstable external components     | Mock external services                  |
| Role mismatch or privilege leaks | Strict testing of permission middleware |



## **11. Deliverables**

* Test scripts under `/tests`
* Coverage report (`pytest --cov`)
* Final test summary document