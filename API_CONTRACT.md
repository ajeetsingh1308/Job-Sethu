Here's your text converted cleanly into a proper **API_CONTRACT.md** file in Markdown format:

```
# API Contract for Job Sethu

This document serves as the single source of truth for all API communication between the Job Sethu frontend (React/React Native) and the backend (Supabase/Node.js Serverless Functions).

---

## Data Models

These are the primary data structures that inform the API payloads.

### User

Represents a user's public profile data stored in our database.

```json
{
  "id": "uuid", // Primary key, matches Supabase Auth user ID
  "full_name": "string",
  "email": "string",
  "avatar_url": "string | null",
  "skills": ["string"],
  "location": "geography(Point, 4326)"
}
```

### **Job**

Represents a job posting created by a user.

```
{
  "id": "uuid",
  "poster_id": "uuid", // Foreign key to users.id
  "title": "string",
  "description": "text",
  "skills_required": ["string"],
  "image_url": "string | null",
  "location": "geography(Point, 4326)",
  "status": "enum('open', 'in_progress', 'completed', 'closed')"
}
```

### **Application**

Represents a user's application for a specific job.

```
{
    "id": "uuid",
    "job_id": "uuid",
    "applicant_id": "uuid",
    "status": "enum('pending', 'accepted', 'rejected')"
}
```

### **Message**

Represents a single message within a job-specific chat.

```
{
    "id": "uuid",
    "job_id": "uuid",
    "sender_id": "uuid",
    "receiver_id": "uuid",
    "content": "text",
    "created_at": "timestampz"
}
```

* * * * *

**Authentication**
------------------

Authentication is handled directly by the **Supabase client library** on the frontend for signup, login, and session management.

There are no custom /api/auth/signup or /api/auth/login endpoints.

All protected API endpoints listed below require a JWT in the request header:

```
Authorization: Bearer <SUPABASE_JWT>
```

* * * * *

**API Endpoints**
-----------------

### **User & Profile Management**

#### **1\. Get Current User Profile**

-   **Feature:** Get authenticated user's profile

-   **HTTP Method:**  GET

-   **Endpoint Path:**  /api/profile

-   **Description:** Retrieves the profile information for the currently logged-in user.

-   **Success Response (200 OK):**

```
{
  "id": "a1b2c3d4-...",
  "full_name": "Ananya Sharma",
  "email": "ananya.sharma@example.com",
  "avatar_url": "https://.../avatar.png",
  "skills": ["Gardening", "Event Planning"]
}
```

-   **Error Response(s):**

    -   401 Unauthorized: User is not authenticated.

#### **2\. Update Current User Profile**

-   **Feature:** Update authenticated user's profile

-   **HTTP Method:**  PUT

-   **Endpoint Path:**  /api/profile

-   **Description:** Updates profile information for the logged-in user.

-   **Request Body:**

```
{
  "full_name": "Ananya Sharma",
  "skills": ["Gardening", "Event Planning", "Tutoring"]
}
```

-   **Success Response (200 OK):** Returns the updated User object.

-   **Error Response(s):**

    -   400 Bad Request: Validation failed.

    -   401 Unauthorized: User is not authenticated.

* * * * *

### **Job Management**

#### **3\. Get All Jobs**

-   **Feature:** Get all job listings

-   **HTTP Method:**  GET

-   **Endpoint Path:**  /api/jobs

-   **Description:** Retrieves a list of open jobs, with optional filters for search and location.

-   **Success Response (200 OK):**

```
[
  {
    "id": "j1a2b3c4-...",
    "title": "Need help with garden weeding",
    "image_url": "https://.../garden.png",
    "location": { "type": "Point", "coordinates": [73.8567, 18.5204] },
    "poster": {
        "full_name": "Rohan Patel"
    }
  }
]
```

-   **Error Response(s):**

    -   500 Internal Server Error: Database query failed.

#### **4\. Get Single Job Details**

-   **Feature:** Get single job details

-   **HTTP Method:**  GET

-   **Endpoint Path:**  /api/jobs/{id}

-   **Description:** Retrieves the full details for a specific job.

-   **Success Response (200 OK):** Returns the full Job object with poster details.

-   **Error Response(s):**

    -   404 Not Found: Job with the specified ID does not exist.

#### **5\. Create a New Job**

-   **Feature:** Create a new job posting

-   **HTTP Method:**  POST

-   **Endpoint Path:**  /api/jobs

-   **Description:** Creates a new job posting for the authenticated user.

-   **Request Body:**

```
{
  "title": "Math Tutor Needed",
  "description": "Looking for a tutor for grade 10 algebra.",
  "skills_required": ["Mathematics", "Tutoring"],
  "location": { "lat": 18.5204, "lon": 73.8567 }
}
```

-   **Success Response (201 Created):** Returns the newly created Job object.

-   **Error Response(s):**

    -   400 Bad Request: Validation failed.

    -   401 Unauthorized: User is not authenticated.

#### **6\. Update a Job**

-   **Feature:** Update a job posting

-   **HTTP Method:**  PUT

-   **Endpoint Path:**  /api/jobs/{id}

-   **Description:** Updates an existing job. Only the job's poster can perform this action.

-   **Request Body:**

```
{
  "title": "Urgent Math Tutor Needed",
  "description": "Updated description...",
  "status": "closed"
}
```

-   **Success Response (200 OK):** Returns the fully updated Job object.

-   **Error Response(s):**

    -   401 Unauthorized: User is not authenticated.

    -   403 Forbidden: User is not the poster of the job.

    -   404 Not Found: Job does not exist.

#### **7\. Delete a Job**

-   **Feature:** Delete a job posting

-   **HTTP Method:**  DELETE

-   **Endpoint Path:**  /api/jobs/{id}

-   **Description:** Deletes a job posting. Only the job's poster can perform this action.

-   **Success Response (204 No Content):** An empty response indicating success.

-   **Error Response(s):**

    -   401 Unauthorized: User is not authenticated.

    -   403 Forbidden: User is not the poster of the job.

    -   404 Not Found: Job does not exist.

* * * * *

### **Job Applications**

#### **8\. Apply for a Job**

-   **Feature:** Apply for a job

-   **HTTP Method:**  POST

-   **Endpoint Path:**  /api/jobs/{id}/apply

-   **Description:** Allows the authenticated user to apply for a specific job.

-   **Success Response (201 Created):** Returns the new Application object.

-   **Error Response(s):**

    -   401 Unauthorized: Not authenticated.

    -   404 Not Found: Job does not exist.

    -   409 Conflict: User has already applied for this job.

#### **9\. Get Applicants for a Job**

-   **Feature:** Get job applicants

-   **HTTP Method:**  GET

-   **Endpoint Path:**  /api/jobs/{id}/applicants

-   **Description:** Retrieves a list of users who have applied for a job. Only accessible by the job poster.

-   **Success Response (200 OK):**

```
[
    {
        "application_id": "app1-...",
        "status": "pending",
        "applicant": {
            "id": "u4d5e6f7-...",
            "full_name": "Priya Singh",
            "avatar_url": "https://.../priya.png",
            "skills": ["Gardening", "Teamwork"]
        }
    }
]
```

-   **Error Response(s):**

    -   403 Forbidden: User is not the job poster.

    -   404 Not Found: Job does not exist.

#### **10\. Update Application Status**

-   **Feature:** Accept or reject an applicant

-   **HTTP Method:**  PUT

-   **Endpoint Path:**  /api/applications/{id}

-   **Description:** Allows a job poster to accept or reject an application.

-   **Request Body:**

```
{
    "status": "accepted"
}
```

-   **Success Response (200 OK):**

```
{
    "message": "Application status updated successfully."
}
```

-   **Error Response(s):**

    -   403 Forbidden: User is not the job poster.

    -   404 Not Found: Application does not exist.

* * * * *

### **Chat & Messaging**

#### **11\. Get Message History for a Job**

-   **Feature:** Get chat history

-   **HTTP Method:**  GET

-   **Endpoint Path:**  /api/jobs/{id}/messages

-   **Description:** Retrieves the full message history for a specific job chat. Accessible only by the poster and the applicant.

-   **Success Response (200 OK):** An array of Message objects.

-   **Error Response(s):**

    -   403 Forbidden: User is not part of this job's communication.

    -   404 Not Found: Job does not exist.

#### **12\. Send a Message**

-   **Feature:** Send a chat message

-   **HTTP Method:**  POST

-   **Endpoint Path:**  /api/jobs/{id}/messages

-   **Description:** Sends a message in the context of a job.

-   **Request Body:**

```
{
    "content": "Hi, I'm available to discuss this further tomorrow morning."
}
```

-   **Success Response (201 Created):** Returns the newly created Message object.

-   **Error Response(s):**

    -   403 Forbidden: User is not part of this job's communication.

    -   404 Not Found: Job does not exist.

* * * * *

### **AI Features (Gemini Integration)**

#### **13\. Suggest Job Details**

-   **Feature:** Generate job description and skills

-   **HTTP Method:**  POST

-   **Endpoint Path:**  /api/ai/suggest-job-details

-   **Description:** Takes a job title and uses Gemini to generate a suggested description and list of skills.

-   **Request Body:**

```
{
  "title": "Help moving boxes to a new apartment"
}
```

-   **Success Response (200 OK):**

```
{
  "description": "Seeking a reliable individual to assist with moving boxes...",
  "skills": ["Manual Labor", "Punctuality", "Physical Fitness"]
}
```

-   **Error Response(s):** 500 Internal Server Error if the AI service fails.

#### **14\. Suggest Chat Reply**

-   **Feature:** Generate reply suggestions for chat

-   **HTTP Method:**  POST

-   **Endpoint Path:**  /api/ai/suggest-reply

-   **Description:** Analyzes chat context and suggests relevant replies.

-   **Request Body:**

```
{
  "job_title": "Need help with garden weeding",
  "message_history": [
    { "sender": "applicant", "content": "Hi, is this job still available?" }
  ]
}
```

-   **Success Response (200 OK):**

```
{
  "suggestions": [
    "Yes, it is! Are you available this week?",
    "Yes! Could you tell me about your experience?",
    "It is. What is your availability like?"
  ]
}
```

-   **Error Response(s):** 500 Internal Server Error if the AI service fails.

```
---

Do you want me to also add a **table of contents at the top** so your teammates can quickly jump to each section in the `.md` file?
```
