# **Functional Use Cases**
This section outlines the functional use cases of the MediLink Appointment System, describing how system components interact to fulfill business objectives through technical features and workflows.
Each use case focuses on the system behavior, triggered by user actions and handled via the backend (FastAPI + PostgreSQL) and frontend (React / PWA).
## **1. Functional Context**
MediLink’s functional layer is responsible for implementing the digital workflows that allow doctors, patients, and administrators to interact seamlessly.
Each function from appointment scheduling to data validation is automated to ensure accuracy, security, and real-time synchronization across users.
## **2. Functional Use Case Summary**
| **Use Case ID** | **Use Case Name** | **Primary Actor(s)** | **Goal / Description** | **Expected Outcome** |
|------------------|-------------------|------------------------|--------------------------|----------------------|
| **FUC-01** | User Authentication | Admin, Doctor, Patient | Secure login and registration through JWT-based authentication. | Access granted after validation. |
| **FUC-02** | Manage Doctor Records | Admin | Add, update, or delete doctor information in the database. | Records updated successfully. |
| **FUC-03** | Manage Appointment Slots | Doctor | Auto-Create or selct time slots for patient booking. | Slots stored and visible to patients. |
| **FUC-04** | Book Appointment | Patient | Select a doctor and time slot to request an appointment. | Request saved as “Pending.” |
| **FUC-05** | Approve/Reject Appointment | Doctor | Approve or reject patient requests. | Appointment status updated. |
| **FUC-06** | Cancel Appointment | Patient | Cancel previously booked appointments. | Slot becomes available again. |
| **FUC-07** | Logout | All Users | End active session securely. | Session terminated, user redirected to login. |

## **3. FUC-01 – User Authentication**
**Description**
Ensures that only verified users can access MediLink using secure credentials.
The system uses JWT tokens for session management and role-based access control.
```mermaid
flowchart TD
    A["User Enters Credentials"] --> B["Frontend Sends Login Request (POST /auth/login)"]
    B --> C["Backend Validates Credentials"]
    C --> D{"Credentials Valid?"}
    D -->|Yes| E["Generate JWT Token and Assign Role"]
    D -->|No| F["Return Error Message: Invalid Username or Password"]
    E --> G["Redirect User to Respective Dashboard"]
    F --> H["Prompt Retry or Password Reset"]
```

### **Alternate Flow / Error Handling**

| **Failure Type** | **System Response** | **User Message** |
|------------------|--------------------|------------------|
| Invalid credentials | HTTP 401 returned | “Invalid username or password.” |
| Token expired | Token refresh triggered | “Session expired. Please log in again.” |

## **4. FUC-03 – Manage Appointment Slots (Doctor)**
**Description**
Allows doctors to define their available consultation slots, which patients can later view and book.

```mermaid
flowchart TD
    A["Doctor Logs In"] --> B["Access 'Manage Slots' Section"]
    B --> C["Select Date for Slot Management"]
    C --> D["View Predefined 30-Minute Time Slots (Checkboxes)"]
    D --> E["Tick / Untick Slots to Mark Availability"]
    E --> F["Optionally Edit Slot Date or Availability (Not Time)"]
    F --> G["Frontend Sends API Request (POST /slots)"]
    G --> H["FastAPI Validates Slot Data (Check duplicates, date validity)"]
    H --> I{"Validation Passed?"}
    I -->|Yes| J["Save Slots / Updates in Database"]
    I -->|No| K["Return Error Message (e.g., Duplicate Slot or Invalid Date)"]
    J --> L["Show Success Message to Doctor"]

```

### **Validation Rules:**

- No duplicate slots.
- Slot date must be todat or future.
- Slots linked to the logged-in doctor’s ID.

## **5. FUC-06 – Cancel Appointment (Patient)**
**Description**
Patient cancels a scheduled appointment before consultation.
```mermaid
flowchart TD
    A["Patient Opens Appointment List"] --> B["Select Appointment to Cancel"]
    B --> C["Click 'Cancel'"]
    C --> D["System Updates Status = Cancelled"]
    D --> E["Slot Released to Database and again available to all patient for booking with color = White"]
    E --> F["Update Doctor"]
    F --> G["Patient Sees Updated Schedule"]
```
## **7. Summary**
MediLink’s functional use cases provide the backbone of the system, enabling smooth user system interactions through secure, validated, and automated processes.
Each function directly supports a corresponding business use case, ensuring that operational goals such as improved efficiency, reduced workload, and better patient satisfaction are achieved through reliable technical execution.