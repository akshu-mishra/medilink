# Appointment Booking → Approve / Reject → Expiry / Auto-cancel
This module manages the complete appointment lifecycle between **patients** and **doctors** in the MediLink system. It ensures that every booking request follows a consistent flow, maintaining slot integrity and time accuracy.

When a patient books an available slot, the appointment status is initially set to **"Pending"**. The respective doctor then reviews the request and either **approves** or **rejects** it based on availability or priority tasks.  

- **Approved appointments** automatically mark the slot as **"Confirmed" (Green)**, ensuring no other bookings can occur during that time.  
- **Rejected appointments** mark the slot as **"Busy" (Grey)**, indicating that the doctor is unavailable for that period.  
- **Auto-cancel/Expiry** logic ensures that if a pending request remains unacted upon beyond its threshold time, the system automatically **rejects** the appointment and frees up the slot, prompting the patient to choose another available time.  

This automation reduces manual overhead for doctors and ensures real-time updates in the appointment dashboard for both parties. It also prevents overlapping or stale bookings, maintaining accuracy and transparency in scheduling.
```mermaid
sequenceDiagram
  autonumber
  participant Patient as Patient
  participant UI as Frontend
  participant Backend as FastAPI
  participant DB as Database
  participant Doctor as Doctor

  Note over Patient,UI: Patient books an available slot
  Patient->>UI: Click "Book" (slot_id, doctor_id)
  UI->>Backend: POST /appointments {patient_id, slot_id, doctor_id}
  Backend->>DB: SELECT slot WHERE id = slot_id
  alt slot is Available
    Backend->>DB: INSERT appointment (status="Pending", created_at)
    Backend->>DB: UPDATE slot.status="Pending" (color=Yellow)
    DB-->>Backend: OK
    Backend-->>UI: 201 {message: "Request saved - Pending"}
    UI-->>Patient: Show "Pending" (slot color = Yellow)
  else slot not available
    Backend-->>UI: 400 {detail: "Slot unavailable"}
    UI-->>Patient: Show {"Slot unavailable, please choose another time"}
  end

  Note over Doctor,Backend: Doctor reviews and acts
  Doctor->>UI: Click Approve / Reject
  UI->>Backend: PATCH /appointments/{id}/action {action: "approve"|"reject"}
  alt Approve
    Backend->>DB: UPDATE appointment.status="Approved"
    Backend->>DB: UPDATE slot.status="Confirmed" (color=Green)
    Backend-->>UI: 200 {message: "Approved successfully"}
    UI-->>Doctor: Show success
  else Reject
    Backend->>DB: UPDATE appointment.status="Rejected"
    Backend->>DB: UPDATE slot.status="Busy" (color=Grey)
    Backend-->>UI: 200 {message: "Rejected successfully"}
    UI-->>Doctor: Show "Doctor unavailable" message
  end

  Note over Backend,DB: When pending slot passes time threshold
  UI->>Backend: GET /appointments/{patient_id} (on dashboard refresh)
  Backend->>DB: SELECT appointment WHERE status="Pending"
  alt expired pending found
    Backend->>DB: UPDATE appointment.status="Rejected"
    Backend->>DB: UPDATE slot.status="Busy" (color=Grey)
    DB-->>Backend: OK
    Backend-->>UI: 200 {message:"Doctor unavailable, please book another slot"}
    UI-->>Patient: Show message: "Doctor unavailable, please book another slot"

  end
```


