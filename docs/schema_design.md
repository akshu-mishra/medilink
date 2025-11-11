# Schema Design and Indexing

## Schema Design 

```mermaid
---
config:
  layout: elk
---
erDiagram
	users {
		int id PK "auto-increment"  
		varchar(100) email UK "index(email)"  
		varchar(255) password_hash  ""      
		timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  ""  
	}
	roles {
		int id PK "auto-increment"  
		varchar(50) name UK ""
		timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  ""  
	}
	user_roles {
		int id PK "auto-increment"  
		int user_id FK ""  
		int role_id FK ""  
		timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  ""  
	}
	patients {
		int id PK "auto-increment"  
		int user_id FK "index(user_id)"  
		varchar(100) name  ""  
		varchar(10) gender  ""  
		date dob  ""    
		varchar(10) blood_group  ""  
		timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  ""  
	}
%% INDEX: user_id (fetch patient profile after login)
	doctors {
		int id PK "auto-increment"  
		int user_id FK "index(user_id)"  
		varchar(100) name  ""  
		varchar(100) specialization  ""  
		varchar(100) qualification  ""  
		varchar(255) position  ""  
		date dob  ""  
		text chamber  ""  
		time start_time  ""  
		time end_time  ""  
		int consultation_fee  ""  
		boolean status  ""  
		timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  ""  
	}
%% INDEX: user_id (fetch doctor profile)
	slots {
		int id PK "auto-increment"  
		int doctor_id FK "composit-index(doctor_id, date, status)"  
		date date  "composit-index(doctor_id, date, status)"  
		time start_time  ""  
		time end_time  ""  
		varchar(20) status  "composit-index(doctor_id, date, status)" 
        timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  "" 
	}
%% COMPOSITE INDEX: (doctor_id, slot_date, status) - find available slots
	appointments {
		int id PK "auto-increment"  
		int patient_id FK "index(patient_id)"  
		int doctor_id FK "index(doctor_id)"  
		int slot_id FK "index(slot_id)"  
		varchar(20) status  ""  
		timestamp booked_at  ""  
		text reason_for_visit  ""  
		text notes(optional)  "" 
        timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  "" 
	}
%% INDEX: patient_id (patient appointment history)
%% INDEX: doctor_id (doctor appointment list)
%% INDEX: slot_id (prevent double-booking)
	prescriptions {
		int id PK "auto-increment"  
		int appointment_id FK "index(appointment_id)"    
		text diagnosis  ""  
		text consultation_details  ""  
		text doctor_notes(optional)  ""  
		timestamp created_at  ""  
		timestamp updated_at  ""  
		timestamp deleted_at  ""  
    }
%% INDEX: appointment_id (fetch prescription fast)
	users||--o{user_roles:"assigned"
	roles||--o{user_roles:"maps"
	users||--||patients:"has"
	users||--||doctors:"has"
	doctors||--o{slots:"creates"
	doctors||--o{appointments:"receives"
	patients||--o{appointments:"books"
	appointments||--o{prescriptions:"generates"
```


## Indexing Table

| **Table Name**   | **Indexed Column(s)**                     | **Reason for Indexing**                                         | **Use Case** |
|------------------|-------------------------------------------|-----------------------------------------------------------------|--------------|
| **users**        | `email`                                   | Ensures unique user login and fast authentication lookups.      | Validate login credentials instantly using `email`. |
| **patients**     | `user_id`                                 | Quickly fetches patient details linked to their user account.   | Retrieve patient profile after successful login. |
| **doctors**      | `user_id`                                 | Improves access speed to doctor profiles tied to user records.  | Load doctor profile on dashboard initialization. |
| **slots**        | `(doctor_id, date, status)` *(Composite)*  | Enhances slot availability checks and prevents redundant scans. | Display all available slots for a specific doctor and date. |
| **appointments** | `patient_id`                              | Speeds up patient appointment history retrieval.                | Show patient’s past and upcoming appointments. |
| **appointments** | `doctor_id`                               | Optimizes doctor’s appointment list queries.                    | Doctor views all scheduled appointments efficiently. |
| **appointments** | `slot_id`                                 | Prevents double-booking and maintains data integrity.           | Ensure only one booking per slot. |
| **prescriptions**| `appointment_id`                          | Enables fast fetching of prescriptions linked to appointments.  | View prescription details post-appointment. |


