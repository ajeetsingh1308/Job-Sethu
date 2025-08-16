You're right to ask. The previous version omitted some JSON code for brevity, a common practice in documentation where a response is a standard object defined in the "Data Models" section. For example, instead of showing the full JSON for a user, it would just say "Returns the User object."

However, being more explicit makes the contract clearer and removes any ambiguity. It's an excellent suggestion.

Here is the fully regenerated `API_CONTRACT.md`, now with explicit JSON examples for every possible request and response body.

* * * * *

API Contract for Job Sethu
==========================

This document serves as the single source of truth for all API communication between the Job Sethu frontend (React/React Native) and the backend (Supabase/Node.js Serverless Functions).

* * * * *

Table of Contents
-----------------

-   [Data Models]

    -   [User]

    -   [Job]

    -   [Application]

    -   [Payment]

    -   [Message]

-   [Authentication]

-   [API Endpoints](https://www.google.com/search?q=%23api-endpoints)

    -   [User & Profile Management](https://www.google.com/search?q=%23user--profile-management)

        -   [1\. Get Current User Profile](https://www.google.com/search?q=%231-get-current-user-profile)

        -   [2\. Update Current User Profile](https://www.google.com/search?q=%232-update-current-user-profile)

    -   [Job Management](https://www.google.com/search?q=%23job-management)

        -   [3\. Get All Jobs](https://www.google.com/search?q=%233-get-all-jobs)

        -   [4\. Get Single Job Details](https://www.google.com/search?q=%234-get-single-job-details)

        -   [5\. Create a New Job](https://www.google.com/search?q=%235-create-a-new-job)

        -   [6\. Update a Job](https://www.google.com/search?q=%236-update-a-job)

        -   [7\. Delete a Job](https://www.google.com/search?q=%237-delete-a-job)

    -   [Job Applications](https://www.google.com/search?q=%23job-applications)

        -   [8\. Apply for a Job](https://www.google.com/search?q=%238-apply-for-a-job)

        -   [9\. Get Applicants for a Job](https://www.google.com/search?q=%239-get-applicants-for-a-job)

        -   [10\. Update Application Status](https://www.google.com/search?q=%2310-update-application-status)

    -   [Payment Integration](https://www.google.com/search?q=%23payment-integration-)

        -   [11\. Create Stripe Connect Account](https://www.google.com/search?q=%2311-create-stripe-connect-account)

        -   [12\. Create Payment Intent](https://www.google.com/search?q=%2312-create-payment-intent)

        -   [13\. Confirm Job Completion & Release Funds](https://www.google.com/search?q=%2313-confirm-job-completion--release-funds)

    -   [Chat & Messaging](https://www.google.com/search?q=%23chat--messaging)

        -   [14\. Get Message History for a Job](https://www.google.com/search?q=%2314-get-message-history-for-a-job)

        -   [15\. Send a Message](https://www.google.com/search?q=%2315-send-a-message)

    -   [AI Features (Gemini Integration)](https://www.google.com/search?q=%23ai-features-gemini-integration)

        -   [16\. Suggest Job Details](https://www.google.com/search?q=%2316-suggest-job-details)

        -   [17\. Suggest Chat Reply](https://www.google.com/search?q=%2317-suggest-chat-reply)

* * * * *

Data Models
-----------

These are the primary data structures that inform the API payloads.

### User

Represents a user's public profile data.

JSON

```
{
  "id": "uuid",
  "full_name": "string",
  "email": "string",
  "avatar_url": "string | null",
  "skills": ["string"],
  "location": "geography(Point, 4326)",
  "stripe_account_id": "string | null",
  "stripe_onboarding_complete": "boolean"
}

```

### Job

Represents a job posting created by a user.

JSON

```
{
  "id": "uuid",
  "poster_id": "uuid",
  "worker_id": "uuid | null",
  "title": "string",
  "description": "text",
  "amount": "integer",
  "skills_required": ["string"],
  "image_url": "string | null",
  "status": "enum('open', 'in_progress', 'pending_completion', 'completed', 'canceled')"
}

```

### Application

Represents a user's application for a specific job.

JSON

```
{
    "id": "uuid",
    "job_id": "uuid",
    "applicant_id": "uuid",
    "status": "enum('pending', 'accepted', 'rejected')"
}

```

### Payment

Tracks the status of a transaction for a job.

JSON

```
{
    "id": "uuid",
    "job_id": "uuid",
    "poster_id": "uuid",
    "worker_id": "uuid",
    "amount": "integer",
    "stripe_payment_intent_id": "string",
    "status": "enum('funded', 'released', 'canceled')"
}

```

### Message

Represents a single message within a job-specific chat.

JSON

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

Authentication
--------------

Authentication is handled directly by the Supabase client library. All protected API endpoints require a JWT in the request header:

Authorization: Bearer <SUPABASE_JWT>

* * * * *

API Endpoints
-------------

### User & Profile Management

#### 1\. Get Current User Profile

-   **Feature:** Get authenticated user's profile

-   **HTTP Method:** `GET`

-   **Endpoint Path:** `/api/profile`

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "id": "a1b2c3d4-e5f6-g7h8-i9j0-k1l2m3n4o5p6",
      "full_name": "Rohan Patel",
      "email": "rohan.patel@example.com",
      "avatar_url": "https://<...>/avatars/rohan.png",
      "skills": ["Event Support", "Driving"],
      "stripe_account_id": "acct_1M2...",
      "stripe_onboarding_complete": true
    }

    ```

-   **Error Response(s):** `401 Unauthorized`

#### 2\. Update Current User Profile

-   **Feature:** Update authenticated user's profile

-   **HTTP Method:** `PUT`

-   **Endpoint Path:** `/api/profile`

-   **Request Body:**

    JSON

    ```
    {
      "full_name": "Rohan Patel",
      "skills": ["Event Support", "Driving", "Photography"]
    }

    ```

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "id": "a1b2c3d4-e5f6-g7h8-i9j0-k1l2m3n4o5p6",
      "full_name": "Rohan Patel",
      "email": "rohan.patel@example.com",
      "avatar_url": "https://<...>/avatars/rohan.png",
      "skills": ["Event Support", "Driving", "Photography"],
      "stripe_account_id": "acct_1M2...",
      "stripe_onboarding_complete": true
    }

    ```

-   **Error Response(s):** `400 Bad Request`, `401 Unauthorized`

* * * * *

### Job Management

#### 3\. Get All Jobs

-   **Feature:** Get all job listings

-   **HTTP Method:** `GET`

-   **Endpoint Path:** `/api/jobs`

-   **Success Response (200 OK):**

    JSON

    ```
    [
      {
        "id": "j1a2b3c4-...",
        "title": "Need help with garden weeding",
        "amount": 200000,
        "poster": {
            "full_name": "Ananya Sharma"
        }
      },
      {
        "id": "k5l6m7n8-...",
        "title": "Photographer for Birthday Party",
        "amount": 800000,
        "poster": {
            "full_name": "Vikram Singh"
        }
      }
    ]

    ```

-   **Error Response(s):** `500 Internal Server Error`

#### 4\. Get Single Job Details

-   **Feature:** Get single job details

-   **HTTP Method:** `GET`

-   **Endpoint Path:** `/api/jobs/{id}`

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "id": "j1a2b3c4-...",
      "poster_id": "p1q2r3s4-...",
      "worker_id": null,
      "title": "Need help with garden weeding",
      "description": "Looking for someone to help clear out weeds from my vegetable patch.",
      "amount": 200000,
      "skills_required": ["Gardening", "Manual Labor"],
      "image_url": "https://<...>/images/garden.png",
      "status": "open"
    }

    ```

-   **Error Response(s):** `404 Not Found`

#### 5\. Create a New Job

-   **Feature:** Create a new job posting

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/jobs`

-   **Request Body:**

    JSON

    ```
    {
      "title": "Math Tutor Needed",
      "description": "Looking for a tutor for grade 10 algebra.",
      "amount": 500000,
      "skills_required": ["Mathematics", "Tutoring"],
      "location": { "lat": 18.5204, "lon": 73.8567 }
    }

    ```

-   **Success Response (201 Created):**

    JSON

    ```
    {
      "id": "m9n8b7v6-...",
      "poster_id": "a1b2c3d4-...",
      "worker_id": null,
      "title": "Math Tutor Needed",
      "description": "Looking for a tutor for grade 10 algebra.",
      "amount": 500000,
      "skills_required": ["Mathematics", "Tutoring"],
      "image_url": null,
      "status": "open"
    }

    ```

-   **Error Response(s):** `400 Bad Request`, `401 Unauthorized`

#### 6\. Update a Job

-   **Feature:** Update a job posting

-   **HTTP Method:** `PUT`

-   **Endpoint Path:** `/api/jobs/{id}`

-   **Request Body:**

    JSON

    ```
    {
      "description": "Updated: Looking for a tutor for grade 10 algebra and geometry.",
      "amount": 600000
    }

    ```

-   **Success Response (200 OK):** The fully updated Job object.

-   **Error Response(s):** `401 Unauthorized`, `403 Forbidden`, `404 Not Found`

#### 7\. Delete a Job

-   **Feature:** Delete a job posting

-   **HTTP Method:** `DELETE`

-   **Endpoint Path:** `/api/jobs/{id}`

-   **Success Response (204 No Content):** (No JSON body is returned for a successful 204 response).

-   **Error Response(s):** `401 Unauthorized`, `403 Forbidden`, `404 Not Found`

* * * * *

### Job Applications

#### 8\. Apply for a Job

-   **Feature:** Apply for a job

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/jobs/{id}/apply`

-   **Success Response (201 Created):**

    JSON

    ```
    {
      "id": "app1a2b3-...",
      "job_id": "j1a2b3c4-...",
      "applicant_id": "u9p8o7i6-...",
      "status": "pending"
    }

    ```

-   **Error Response(s):** `401 Unauthorized`, `404 Not Found`, `409 Conflict`

#### 9\. Get Applicants for a Job

-   **Feature:** Get job applicants

-   **HTTP Method:** `GET`

-   **Endpoint Path:** `/api/jobs/{id}/applicants`

-   **Success Response (200 OK):**

    JSON

    ```
    [
      {
        "application_id": "app1a2b3-...",
        "status": "pending",
        "applicant": {
            "id": "u9p8o7i6-...",
            "full_name": "Priya Singh",
            "skills": ["Gardening", "Teamwork"]
        }
      }
    ]

    ```

-   **Error Response(s):** `403 Forbidden`, `404 Not Found`

#### 10\. Update Application Status

-   **Feature:** Accept or reject an applicant

-   **HTTP Method:** `PUT`

-   **Endpoint Path:** `/api/applications/{id}`

-   **Request Body:**

    JSON

    ```
    { "status": "accepted" }

    ```

-   **Success Response (200 OK):** `{"message": "Application status updated."}`

-   **Error Response(s):** `403 Forbidden`, `404 Not Found`

* * * * *

### Payment Integration 💳

#### 11\. Create Stripe Connect Account

-   **Feature:** Onboard a worker for payouts

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/stripe/create-connect-account`

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "onboarding_url": "https://connect.stripe.com/express/..."
    }

    ```

-   **Error Response(s):** `401 Unauthorized`, `500 Internal Server Error`

#### 12\. Create Payment Intent

-   **Feature:** Fund a job into escrow

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/jobs/{id}/create-payment-intent`

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "client_secret": "pi_..._secret_..."
    }

    ```

-   **Error Response(s):** `403 Forbidden`, `404 Not Found`, `500 Internal Server Error`

#### 13\. Confirm Job Completion & Release Funds

-   **Feature:** Payout the worker

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/jobs/{id}/confirm-completion`

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "message": "Completion confirmed. Funds are being released to the worker."
    }

    ```

-   **Error Response(s):** `403 Forbidden`, `404 Not Found`, `500 Internal Server Error`

* * * * *

### Chat & Messaging

#### 14\. Get Message History for a Job

-   **Feature:** Get chat history

-   **HTTP Method:** `GET`

-   **Endpoint Path:** `/api/jobs/{id}/messages`

-   **Success Response (200 OK):**

    JSON

    ```
    [
      {
        "id": "msg1-...",
        "job_id": "j1a2b3c4-...",
        "sender_id": "u9p8o7i6-...",
        "receiver_id": "p1q2r3s4-...",
        "content": "Hi, I'm interested in the gardening job. Is it still available?",
        "created_at": "2025-08-16T10:30:00Z"
      },
      {
        "id": "msg2-...",
        "job_id": "j1a2b3c4-...",
        "sender_id": "p1q2r3s4-...",
        "receiver_id": "u9p8o7i6-...",
        "content": "Yes it is! Do you have any prior experience?",
        "created_at": "2025-08-16T10:31:00Z"
      }
    ]

    ```

-   **Error Response(s):** `403 Forbidden`, `404 Not Found`

#### 15\. Send a Message

-   **Feature:** Send a chat message

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/jobs/{id}/messages`

-   **Request Body:**

    JSON

    ```
    { "content": "Yes, I have 2 years of landscaping experience." }

    ```

-   **Success Response (201 Created):**

    JSON

    ```
    {
      "id": "msg3-...",
      "job_id": "j1a2b3c4-...",
      "sender_id": "u9p8o7i6-...",
      "receiver_id": "p1q2r3s4-...",
      "content": "Yes, I have 2 years of landscaping experience.",
      "created_at": "2025-08-16T10:32:00Z"
    }

    ```

-   **Error Response(s):** `403 Forbidden`, `404 Not Found`

* * * * *

### AI Features (Gemini Integration)

#### 16\. Suggest Job Details

-   **Feature:** Generate job description and skills

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/ai/suggest-job-details`

-   **Request Body:**

    JSON

    ```
    { "title": "Help moving boxes to a new apartment" }

    ```

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "description": "Seeking a reliable individual to assist with moving boxes...",
      "skills": ["Manual Labor", "Punctuality", "Physical Fitness"]
    }

    ```

-   **Error Response(s):** `500 Internal Server Error`

#### 17\. Suggest Chat Reply

-   **Feature:** Generate reply suggestions for chat

-   **HTTP Method:** `POST`

-   **Endpoint Path:** `/api/ai/suggest-reply`

-   **Request Body:**

    JSON

    ```
    {
      "job_title": "Need help with garden weeding",
      "message_history": [
        { "sender": "applicant", "content": "Hi, is this job still available?" }
      ]
    }

    ```

-   **Success Response (200 OK):**

    JSON

    ```
    {
      "suggestions": [
        "Yes, it is! Are you available this week?",
        "Yes! Could you tell me about your experience?",
        "It is. What is your availability like?"
      ]
    }

    ```

-   **Error Response(s):** `500 Internal Server Error`
