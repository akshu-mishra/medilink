# Medilink Backend - Unit Test Plan

## 1. Overview
This document outlines the **Unit Testing Plan** for the **Medilink Backend** — a healthcare management system developed using **FastAPI**, **JWT authentication**, and **PostgreSQL**.  
The goal is to ensure that all individual components, routes, and functions work correctly and securely in isolation.



## 2. Modules Under Test

1. **Authentication**

    - Register (Patient)
    - Login (All roles)
    - JWT Token creation & validation

2. **Authorization**

    - Role-based access (Admin / Doctor / Patient)
    - Restricted routes check

3. **Admin Module**

    - Create Doctor
    - View all users / doctors

4. **Doctor Module**

    - View & manage appointment slots
    - Update profile info

5. **Patient Module**

    - Register, view, and book appointment

6. **Database Layer**

    - ORM Model Validation
    - Foreign Key Mapping (user_id in doctors)


## 3. Unit Test Objectives

- Validate that all API endpoints return expected HTTP status codes.  
- Verify correct JWT generation and decoding.  
- Ensure bcrypt password hashing and verification work correctly.  
- Test all CRUD operations for models.  
- Mock database queries and ensure functions return valid responses.  
- Verify exceptions and input validation handling.


## 4. Test Strategy

| Type | Description | Tool |
|------|--------------|------|
| **Positive Tests** | Verify valid inputs and correct outputs. | Pytest |
| **Negative Tests** | Invalid inputs, wrong credentials, or unauthorized access. | Pytest |
| **Mock Tests** | Mock DB and JWT to test function logic independently. | unittest.mock |
| **Validation Tests** | Test schema validation (Pydantic). | Pytest |
| **Error Handling** | Verify correct exception messages and status codes. | FastAPI TestClient |


## 5. Sample Test Cases

| ID | Module | Test Scenario | Expected Result | Status |
|----|---------|----------------|-----------------|---------|
| UT01 | Auth | Register new patient | 201 Created, JWT returned |  |
| UT02 | Auth | Login with valid credentials | 200 OK + Token |  |
| UT03 | Auth | Login with wrong password | 401 Unauthorized |  |
| UT04 | Auth | Verify JWT token validation | 200 OK |  |
| UT05 | Authorization | Doctor tries to create doctor | 403 Forbidden |  |
| UT06 | Admin | Admin creates new doctor | 201 Created |  |
| UT07 | Patient | Book appointment with valid slot | 200 OK |  |
| UT08 | DB | Validate user-doctor foreign key | Pass |  |
| UT09 | Security | Password hash comparison | True |  |
| UT10 | Error Handling | Missing field in request | 422 Validation Error |  |



## 6. Test Snippet (test_auth.py)

```bash
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_patient_register():
    response = client.post("/register", json={
        "email": "patient1@medilink.com",
        "password": "Password@123",
        "login_as": "patient"
    })
    assert response.status_code == 201
    assert "access_token" in response.json()

def test_login_with_invalid_password():
    response = client.post("/login", json={
        "email": "patient1@medilink.com",
        "password": "wrongpass"
    })
    assert response.status_code == 401

```