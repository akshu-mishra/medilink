# Technical Stack with their appropriate versions

Below are the core technologies and tools used in **MediLink**, along with their versions, usage areas, and reasons for selection.

| **Technology / Tool** | **Version** | **Usage in MediLink** | **Reason for Choosing This Version** |
|------------------------|-------------|------------------------|--------------------------------------|
| **HTML5** | 5.2 | Used for structuring all web pages such as Login, Register, Landing, and Dashboard. | HTML 5.2 provides semantic elements and full browser support for PWA integration and accessibility. |
| **CSS3** | 3.0 | For page styling, layouts, and responsive UI design. | Essential for responsive design. |
| **React** | ES13 / 2022 | Manages dynamic UI updates, API requests, and form validation. | ES2022 provides stable modern JS features like async/await, top-level await, and modular imports for smooth API integration. |
| **FastAPI** | 0.115.2 | Backend framework handling authentication, appointment APIs, and slot management. | Latest stable release optimized for async I/O and built-in Pydantic v2 support. |
| **Uvicorn** | 0.30.3 | ASGI server for running the FastAPI backend. | Lightweight, fast, and compatible with FastAPI 0.115 for concurrent request handling. |
| **PostgreSQL** | 15.6 | Relational database for storing users, doctors, appointments, and slot data. | PostgreSQL 15.6 is stable, ACID-compliant, and offers strong performance with indexing and query optimization. |
| **SQLAlchemy** | 2.0.36 | ORM for interacting between FastAPI and PostgreSQL. | SQLAlchemy 2.0 provides full async support and modern syntax compatible with Python 3.10+. |
| **Pydantic** | 2.5.3 | Used for request/response model validation in FastAPI. | Pydantic 2.5 improves validation performance and integrates seamlessly with FastAPI 0.115. |
| **Python** | 3.10.14 | Core backend programming language. | Python 3.10 introduces structural pattern matching and better async features, ensuring compatibility with FastAPI. |
| **PWA (Progressive Web App)** | 2023 Standard | Adds offline access and app install functionality for users. | PWA standards (2023) ensure secure HTTPS, caching, and installable UI for all modern browsers. |

### Summary of Stack Utilization

- **Frontend:** HTML5, CSS3, React, Bootstrap 5.3, PWA  
- **Backend:** FastAPI 0.115, Uvicorn 0.30, SQLAlchemy 2.0, Pydantic 2.5  
- **Database:** PostgreSQL 15  
- **Language:** Python 3.10  


### Versioning Note
All chosen versions are **stable releases** as of **November 2025**, ensuring long-term compatibility, active support, and smooth integration across MediLink’s modules.
