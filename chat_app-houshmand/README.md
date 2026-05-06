# 💬 Chattrixx — A Real-Time WebSocket Chat App

<p align="center">
  <img src="readme_files/chat.png" alt="Chat Interface" width="700"/>
</p>

A full-stack, real-time group chat application built with **FastAPI**, **WebSockets**, **PostgreSQL**, and a vanilla HTML/CSS/JS frontend — all containerized with **Docker Compose**.

---

## ✨ Features

- 🔐 **JWT Authentication** — Secure login with token-based auth
- 💬 **Real-Time Messaging** — Instant message delivery via WebSockets
- 👥 **Group Chat Rooms** — Create and join groups by unique address
- 📬 **Unread Message Tracking** — Offline users receive missed messages on reconnect
- ✏️ **Edit & Delete Messages** — With live broadcast of changes to all online members
- 👤 **User Profiles** — Display name, bio, and profile picture support
- 🛡️ **Role-Based Access** — Admin and Member roles per group
- 🐘 **PostgreSQL** — Persistent, production-grade database
- 🗄️ **pgAdmin** — Built-in database GUI for easy management
- 🐳 **Docker Compose** — One-command setup for the entire stack

---

## 🖼️ Screenshots

| Login | Group List | Chat |
|-------|-----------|------|
| ![Login](readme_files/login.png) | ![Groups](readme_files/group_list.png) | ![Chat](readme_files/chat.png) |

| Unread Messages | API Docs |
|-----------------|---------|
| ![Unread](readme_files/unread_message.png) | ![API](readme_files/api.png) |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│                Docker Compose               │
│                                             │
│  ┌──────────┐    ┌──────────┐               │
│  │  Nginx   │    │ FastAPI  │               │
│  │ :80      │───▶│ :8000    │               │
│  │ Frontend │    │ Backend  │               │
│  └──────────┘    └────┬─────┘               │
│                       │                     │
│              ┌────────▼────────┐            │
│              │   PostgreSQL    │            │
│              │   :5432         │            │
│              └────────────────┘            │
│                                             │
│  ┌──────────┐                               │
│  │ pgAdmin  │ :5050                         │
│  └──────────┘                               │
└─────────────────────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- Git

### 1. Clone the repository

```bash
git clone https://github.com/AmanGupta-001/Chattrixx-A-WebSocket-Chat-App.git
cd Chattrixx-A-WebSocket-Chat-App/chat_app-houshmand
```

### 2. Create your environment file

```bash
cp .env.example .env
```

Edit `.env` with your settings (see [Environment Variables](#-environment-variables)).

### 3. Start the application

```bash
docker compose up --build
```

That's it! The full stack will be up and running.

---

## 🌐 Access the App

| Service | URL | Description |
|---------|-----|-------------|
| **Frontend** | http://localhost | Chat UI served by Nginx |
| **Backend API** | http://localhost:8000 | FastAPI backend |
| **API Docs** | http://localhost:8000/docs | Interactive Swagger UI |
| **pgAdmin** | http://localhost:5050 | Database management GUI |

> **pgAdmin credentials:** `admin@admin.com` / `admin`

---

## 🔌 API Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/token` | Login and get JWT access token |
| `GET`  | `/health` | Health check |

### Users
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/user/create/` | Register a new user |
| `GET`  | `/user/me/` | Get current user profile |
| `PUT`  | `/user/update/` | Update user profile |

### Groups
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/group/create/` | Create a new chat group |
| `POST` | `/group/join` | Join a group by address |
| `GET`  | `/group/{id}/members` | List group members |
| `GET`  | `/group/{id}/messages` | Get group message history |

### Messages
| Method | Endpoint | Description |
|--------|----------|-------------|
| `PUT`  | `/message/{id}/edit` | Edit a sent message |
| `DELETE` | `/message/{id}/delete` | Delete a message |

### WebSockets
| Endpoint | Description |
|----------|-------------|
| `WS /send-message?token=...&group_id=...` | Send real-time messages |
| `WS /get-unread-messages?token=...&group_id=...` | Receive messages (including unread) |

---

## ⚙️ Environment Variables

Create a `.env` file in the `chat_app-houshmand/` directory:

```env
# Database
DATABASE_URL=postgresql://root:1234@db:5432/postgres

# JWT
SECRET_KEY=your-super-secret-key-here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|-----------|---------|
| **FastAPI** | High-performance async web framework |
| **WebSockets** | Real-time bidirectional communication |
| **SQLAlchemy 2.0** | ORM for database interactions |
| **PostgreSQL** | Relational database |
| **psycopg3** | PostgreSQL driver |
| **python-jose** | JWT token handling |
| **bcrypt** | Password hashing |
| **Pydantic v2** | Data validation and serialization |
| **Uvicorn** | ASGI server |

### Frontend
| Technology | Purpose |
|-----------|---------|
| **HTML5 / CSS3** | Structure and styling |
| **Vanilla JavaScript** | WebSocket client logic |
| **Nginx** | Static file serving |

### Infrastructure
| Technology | Purpose |
|-----------|---------|
| **Docker Compose** | Multi-container orchestration |
| **PostgreSQL 16** | Production database |
| **pgAdmin 4** | Database GUI |

---

## 📁 Project Structure

```
chat_app-houshmand/
├── backend/
│   ├── chat/
│   │   ├── views/
│   │   │   ├── auth.py         # Login & token endpoints
│   │   │   ├── groups.py       # Group management endpoints
│   │   │   ├── messages.py     # Message edit/delete endpoints
│   │   │   ├── user.py         # User profile endpoints
│   │   │   └── websocket.py    # WebSocket handlers
│   │   ├── database.py         # SQLAlchemy engine & session
│   │   ├── models.py           # Database models
│   │   ├── schema.py           # Pydantic schemas
│   │   ├── crud.py             # Database operations
│   │   ├── setting.py          # App configuration
│   │   └── utils/
│   │       ├── jwt.py          # JWT helpers
│   │       └── exception.py    # Custom exceptions
│   ├── main.py                 # App entry point
│   ├── requirements.txt        # Python dependencies
│   └── Dockerfile
├── frontend/
│   ├── index.html              # Group list page
│   ├── login.html              # Login page
│   ├── chat.html               # Chat room page
│   ├── create_user.html        # Registration page
│   ├── script.js               # WebSocket & chat logic
│   ├── group_list.js           # Group list logic
│   ├── style.css               # Chat styles
│   └── style_index.css         # Index/landing styles
├── docker-compose.yaml
└── README.md
```

---

## 🔧 Manual Setup (Without Docker)

<details>
<summary>Click to expand</summary>

### Backend

```bash
cd backend
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux/Mac

pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Frontend

Serve the `frontend/` folder with any static file server:

```bash
# Using Python
cd frontend
python -m http.server 80
```

</details>

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add some amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<p align="center">Built with ❤️ using FastAPI & WebSockets</p>
