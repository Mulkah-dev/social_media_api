
# Social Media API (Django REST Framework)

A Django REST Framework API for user registration, authentication, and profile management using token authentication.


## Setup

1. **Clone the repository**

```bash
git clone <your-repo-url>
cd social_media_api
````

2. **Create a virtual environment and activate it**

```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate
```

3. **Install dependencies**

```bash
pip install -r requirements.txt
```

4. **Apply migrations**

```bash
python manage.py migrate
```

5. **Create a superuser (optional for admin access)**

```bash
python manage.py createsuperuser
```

6. **Run the development server**

```bash
python manage.py runserver
```

Your API will be available at `http://127.0.0.1:8000/`.

---

## User Model

The project uses a **custom user model** (`CustomUser`) that extends Django’s `AbstractUser`:

| Field            | Description                                   |
| ---------------- | --------------------------------------------- |
| username         | Unique username                               |
| email            | User email address                            |
| password         | User password (hashed)                        |
| bio              | Short biography                               |
| profile\_picture | Optional profile image                        |
| followers        | Many-to-many relation referencing other users |

---

## API Endpoints

| Endpoint                  | Method | Description                         |
| ------------------------- | ------ | ----------------------------------- |
| `/api/accounts/register/` | POST   | Register a new user (returns token) |
| `/api/accounts/login/`    | POST   | Authenticate a user (returns token) |
| `/api/accounts/profile/`  | GET    | Retrieve authenticated user profile |

---

### Register User

**Request**

`POST /api/accounts/register/`

```json
{
  "username": "mmy",
  "email": "mmy@example.com",
  "password": "StrongPass123",
  "bio": "My bio for testing"
}

```

**Response**

```json
{
    "id": 5,
    "username": "mmy",
    "email": "mmy@example.com",
    "bio": "My bio for testing",
    "profile_picture": null,
    "token": "221591845bdd01968bb958dec874c5f0b6e3b673"
}


### Login User

**Request**

`POST /api/accounts/login/`

```json
{
  "username": "mmy",
  "password": "StrongPass123"
}
```

**Response**

```json
{
    "username": "mmy",
    "password": "StrongPass123",
    "token": "221591845bdd01968bb958dec874c5f0b6e3b673"
}
```

---

## Authentication

* This API uses **Token Authentication**.
* Include the token in the `Authorization` header for protected endpoints:

```
Authorization: Token <your-token>
```

---

## Testing

You can use **Postman** or **cURL** to test registration and login endpoints. Make sure to:

1. Register a new user (`/register/`) → get token.
2. Use the token to authenticate requests to `/profile/` or other protected endpoints.

---

## Notes

* Ensure `rest_framework` and `rest_framework.authtoken` are added to `INSTALLED_APPS`.
* Run migrations after adding `authtoken`:

```bash
python manage.py migrate

Example request:create post
POST /api/posts/
Authorization: Token <your_token>
Content-Type: application/json

{
  "title": "My first post",
  "content": "This is my content"
}
Example response
{
  "id": 1,
  "title": "My first post",
  "content": "This is my content",
  "author": "maryam",
  "created_at": "2025-08-27T12:00:00Z",
  "updated_at": "2025-08-27T12:00:00Z",
  "comments": []
}

Follow User
POST /accounts/follow/2/
Authorization: Token <token>


Response:

{"message": "You are now following maryam"}

Feed
GET /api/feed/
Authorization: Token <token>


Response:

[
  {
    "id": 5,
    "title": "My Day",
    "content": "Had a great time coding!",
    "author": "maryam",
    "created_at": "2025-08-27T12:30:00Z",
    "updated_at": "2025-08-27T12:30:00Z",
    "comments": []
  }
]

POST /posts/<pk>/like/
→ Like a post. Returns {"detail": "Post liked."}

POST /posts/<pk>/unlike/
→ Unlike a post. Returns {"detail": "Post unliked."}

GET /notifications/
→ Returns list of user’s notifications.