# PrimeTradeAI Task Manager API

## 📋 Project Overview

A production-ready REST API built with **Node.js**, **Express**, and **MongoDB** featuring user authentication, role-based access control (RBAC), and task management. This project demonstrates scalable backend architecture with security best practices, comprehensive API documentation, and a simple React frontend for API interaction.

**🎯 Built as a Backend Developer Intern Assignment** - To showcase backend development skills, API design, security implementation, and full-stack integration capabilities.

---

## ✨ Core Features

### Backend (Primary Focus)

- ✅ **User Authentication**: Secure registration & login with JWT tokens and bcrypt password hashing
- ✅ **Role-Based Access Control**: User and Admin roles with protected endpoints
- ✅ **Task Management API**: Complete CRUD operations for tasks (secondary entity)
- ✅ **API Versioning**: `/api/v1/` for future-proof extensibility
- ✅ **Input Validation**: Request validation using `express-validator`
- ✅ **Interactive API Docs**: Swagger UI at `/api-docs`
- ✅ **Centralized Error Handling**: Consistent error responses across all endpoints
- ✅ **Structured Logging**: Pino logger for debugging and monitoring
- ✅ **Redis Caching**: Performance optimization for GET requests
- ✅ **Security Hardening**: CORS, rate limiting, input sanitization

### Frontend (Supportive)

- ✅ **React.js UI**: Simple, responsive interface for API interaction
- ✅ **User Registration & Login**: Forms with validation and error handling
- ✅ **Protected Dashboard**: JWT-based authentication flow
- ✅ **Task CRUD Interface**: Create, read, update, and delete tasks
- ✅ **Real-time Feedback**: Success/error messages from API responses
- ✅ **Token Management**: Automatic token storage and injection

---

## 🗂️ Project Structure

```
PrimeTradeAI/
├── backend/
│   ├── controllers/       # Request handlers (auth, tasks, users)
│   ├── middlewares/       # Auth, validation, error handling
│   ├── models/           # MongoDB schemas (User, Task)
│   ├── routes/           # API route definitions
│   ├── utils/            # Helper functions, logger, cache
│   ├── server.js         # Application entry point
│   ├── swagger.js        # Swagger documentation config
│   ├── .env.example      # Environment variables template
│   └── package.json      # Backend dependencies
│
├── frontend/             # React.js UI (basic implementation)
│   ├── src/
│   │   ├── components/   # React components (Login, Register, Dashboard)
│   │   ├── services/     # API service layer
│   │   ├── context/      # Auth context for state management
│   │   └── App.js        # Main application component
│   └── package.json      # Frontend dependencies
│
├── docker-compose.yml    # Multi-container Docker setup
├── Dockerfile            # Docker configuration for backend
└── README.md            # This file
```

---

## 🛠️ Technology Stack

### Backend

- **Runtime**: Node.js (v18+)
- **Framework**: Express.js
- **Database**: MongoDB (Atlas)
- **Caching**: Redis
- **Authentication**: JWT (jsonwebtoken) + bcrypt
- **Validation**: express-validator
- **Documentation**: Swagger UI
- **Logging**: Pino
- **Security**: CORS, rate limiting

### Frontend

- **Framework**: React.js (v18+)
- **HTTP Client**: Axios
- **Styling**: CSS3
- **State Management**: Context API

### DevOps

- **Containerization**: Docker + Docker Compose
- **Version Control**: Git/GitHub

---

## 🚀 Quick Start Guide

### Prerequisites

Ensure you have the following installed:

- **Node.js** (v18 or higher) - [Download](https://nodejs.org/)
- **MongoDB** (local or Atlas account) - [Download](https://www.mongodb.com/try/download/community)
- **Redis** (local or cloud instance) - [Download](https://redis.io/download)
- **Git** - [Download](https://git-scm.com/)
- **Docker** (optional) - [Download](https://www.docker.com/)

---

### Installation Steps

#### 1️⃣ Clone the Repository

```bash
git clone https://github.com/yourusername/PrimeTradeAI.git
cd PrimeTradeAI
```

#### 2️⃣ Backend Setup

```bash
cd backend
npm install
```

**Create `.env` file:**

```env
# Server Configuration
PORT=3000

# MongoDB
MONGODB_URI=mongodb://localhost:27017/primetradeai
# For MongoDB Atlas:
# MONGODB_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/primetradeai?retryWrites=true&w=majority

# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-minimum-32-characters-long

# Redis Configuration
REDIS_URL=redis://localhost:6379
# For cloud Redis:
# REDIS_URL=redis://<username>:<password>@<host>:<port>

```

> ⚠️ **Security Note**: Generate a strong JWT secret using: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`

**Start Redis (Windows):**

```bash
# Using Docker (recommended)
docker run -d -p 6379:6379 --name redis redis:alpine

# Or download from: https://github.com/microsoftarchive/redis/releases
```

**Run Backend:**

```bash
# Development mode (with hot reload)
npm run dev

# Production mode
npm start
```

Backend starts at: `http://localhost:3000`

#### 3️⃣ Frontend Setup

```bash
cd ../frontend
npm install
npm start
```

Frontend starts at: `http://localhost:3001`

---

## 📚 API Documentation

### Access Swagger UI

Interactive API documentation available at:
**`http://localhost:3000/api-docs`**

### Authentication Flow

1. **Register** → `POST /api/v1/users/register`
2. **Login** → `POST /api/v1/users/login` (returns JWT token)
3. **Use Token** → Include in all protected endpoints:
   ```
   Authorization: Bearer <your-jwt-token>
   ```

---

## 🔐 API Endpoints

### Authentication & User Management

| Method   | Endpoint                 | Description              | Auth   | Role       |
| -------- | ------------------------ | ------------------------ | ------ | ---------- |
| `POST`   | `/api/v1/users/register` | Register new user        | ❌ No  | -          |
| `POST`   | `/api/v1/users/login`    | User login (returns JWT) | ❌ No  | -          |
| `GET`    | `/api/v1/users/me`       | Get current user profile | ✅ Yes | User/Admin |
| `PUT`    | `/api/v1/users/me`       | Update user profile      | ✅ Yes | User/Admin |
| `DELETE` | `/api/v1/users/me`       | Delete own account       | ✅ Yes | User/Admin |

**Example Request (Register):**

```json
POST /api/v1/users/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "SecurePass123!",
  "role": "user"
}
```

**Example Response:**

```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user"
  }
}
```

### Task Management (CRUD)

| Method   | Endpoint            | Description               | Auth   | Role       |
| -------- | ------------------- | ------------------------- | ------ | ---------- |
| `GET`    | `/api/v1/tasks`     | Get user's tasks (cached) | ✅ Yes | User/Admin |
| `GET`    | `/api/v1/tasks/:id` | Get specific task         | ✅ Yes | User/Admin |
| `POST`   | `/api/v1/tasks`     | Create new task           | ✅ Yes | User/Admin |
| `PUT`    | `/api/v1/tasks/:id` | Update task               | ✅ Yes | User/Admin |
| `DELETE` | `/api/v1/tasks/:id` | Delete task               | ✅ Yes | User/Admin |

**Example Request (Create Task):**

```json
POST /api/v1/tasks
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Complete Backend Assignment",
  "description": "Build REST API with authentication",
  "status": "in-progress",
  "priority": "high",
  "dueDate": "2025-10-31"
}
```

### Admin Endpoints (Role: Admin Only)

| Method   | Endpoint                       | Description      | Auth   | Role  |
| -------- | ------------------------------ | ---------------- | ------ | ----- |
| `GET`    | `/api/v1/users/admin/all`      | Get all users    | ✅ Yes | Admin |
| `GET`    | `/api/v1/users/admin/:id`      | Get user by ID   | ✅ Yes | Admin |
| `PATCH`  | `/api/v1/users/admin/:id/role` | Change user role | ✅ Yes | Admin |
| `DELETE` | `/api/v1/users/admin/:id`      | Delete any user  | ✅ Yes | Admin |
| `GET`    | `/api/v1/tasks/admin/all`      | Get all tasks    | ✅ Yes | Admin |
| `DELETE` | `/api/v1/tasks/admin/:id`      | Delete any task  | ✅ Yes | Admin |

---

## 🗄️ Database Schema Design

### User Schema

```javascript
{
  name: String (required, trimmed),
  email: String (required, unique, lowercase),
  password: String (required, hashed with bcrypt),
  role: String (enum: ['user', 'admin'], default: 'user'),
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**

- `email` (unique)
- `role` (for admin queries)

### Task Schema

```javascript
{
  title: String (required),
  description: String,
  status: String (enum: ['pending', 'in-progress', 'completed'], default: 'pending'),
  priority: String (enum: ['low', 'medium', 'high'], default: 'medium'),
  dueDate: Date,
  user: ObjectId (ref: 'User', required),
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**

- `user` (for user-specific queries)
- `status` (for filtering)
- Compound: `{user: 1, status: 1}` (optimized queries)

---

## ⚡ Caching Strategy

### Redis Implementation

- **GET** `/api/v1/tasks` responses cached for **5 minutes per user**
- **Cache Key Format**: `tasks:user:{userId}`
- **Automatic Invalidation** on:
  - Task creation (POST)
  - Task updates (PUT)
  - Task deletion (DELETE)

### Cache Flow

```
1. GET /api/v1/tasks
2. Check Redis: tasks:user:507f1f77bcf86cd799439011
3. If HIT → Return cached data
4. If MISS → Query MongoDB → Cache result → Return data
```

---

## 🐳 Docker Deployment

### Quick Start with Docker Compose

```bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### `docker-compose.yml`

```yaml
version: "3.8"

services:
  # Backend API
  api:
    build: ./backend
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - MONGODB_URI=mongodb://mongo:27017/primetradeai
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=${JWT_SECRET}
      - JWT_EXPIRE=7d
    depends_on:
      - mongo
      - redis
    restart: unless-stopped

  # MongoDB
  mongo:
    image: mongo:7
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    restart: unless-stopped

  # Redis
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
    restart: unless-stopped

  # Frontend (optional)
  frontend:
    build: ./frontend
    ports:
      - "3001:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:3000/api/v1
    depends_on:
      - api
    restart: unless-stopped

volumes:
  mongo-data:
```

### Manual Docker Build

```bash
# Build backend image
cd backend
docker build -t primetradeai-api .

# Run with environment variables
docker run -p 3000:3000 --env-file .env primetradeai-api
```

---

## 📈 Scalability & Performance

### Current Implementation

| Feature                | Implementation                 | Benefit                           |
| ---------------------- | ------------------------------ | --------------------------------- |
| **Caching**            | Redis for GET tasks            | Reduces DB load, faster responses |
| **Database Indexes**   | User ID, status, compound keys | Optimized query performance       |
| **Connection Pooling** | MongoDB native pooling         | Efficient connection management   |
| **Async Operations**   | Async/await throughout         | Non-blocking I/O                  |
| **Error Handling**     | Centralized middleware         | Consistent error responses        |
| **Logging**            | Pino structured logging        | Debugging and monitoring          |
| **Security**           | JWT + bcrypt + Helmet          | Industry-standard protection      |

### Scalability Roadmap

#### Short-term (1-3 months)

- ✅ Add rate limiting per user/IP
- ✅ Implement database connection pooling tuning
- ✅ Add API response compression (gzip)
- ✅ Set up monitoring (Application Insights / Datadog)
- ✅ Add health check endpoints

#### Mid-term (3-6 months)

- 🔄 **Microservices Architecture**:
  - Auth Service (user authentication)
  - Task Service (task management)
  - API Gateway (Kong/Azure API Management)
- 🔄 **Message Queue**: RabbitMQ/Azure Service Bus for async tasks
- 🔄 **Load Balancing**: Nginx or Azure Load Balancer
- 🔄 **Database Sharding**: Horizontal scaling for MongoDB

#### Long-term (6-12 months)

- 🚀 **Event-Driven Architecture**: Event sourcing for audit trails
- 🚀 **CQRS Pattern**: Separate read/write models
- 🚀 **GraphQL Layer**: Flexible querying for complex UIs
- 🚀 **Kubernetes**: Container orchestration for auto-scaling
- 🚀 **Multi-region Deployment**: Global CDN and database replication

---

## 🔒 Security Implementation

### Authentication & Authorization

- ✅ **Password Hashing**: bcrypt with 10 rounds
- ✅ **JWT Tokens**: Signed with HS256, 7-day expiration
- ✅ **Token Storage**: HTTP-only cookies (recommended) or localStorage
- ✅ **Role-Based Access**: Middleware checks for user/admin roles
- ✅ **Token Refresh**: Implement refresh token flow (future)

### Input Validation & Sanitization

- ✅ **express-validator**: Schema-based validation
- ✅ **Mongoose Validation**: Schema-level constraints
- ✅ **SQL Injection Prevention**: MongoDB parameterized queries
- ✅ **XSS Protection**: Helmet.js content security policy
- ✅ **NoSQL Injection**: Input sanitization with `mongo-sanitize`

### Security Headers (Helmet.js)

```javascript
- Content-Security-Policy
- X-DNS-Prefetch-Control
- X-Frame-Options: DENY
- X-Content-Type-Options: nosniff
- X-XSS-Protection: 1; mode=block
```

### Additional Security Measures

- ✅ **CORS Configuration**: Whitelist allowed origins
- ✅ **Rate Limiting**: 100 requests per 15 minutes per IP
- ✅ **Environment Variables**: Secrets never in code
- ✅ **HTTPS**: Enforce in production
- ✅ **Dependencies**: Regular `npm audit` checks

---

## 🧪 Testing

```bash
# Run unit tests
npm test

# Run integration tests
npm run test:integration

# Generate coverage report
npm run test:coverage

# Run tests in watch mode
npm run test:watch
```

### Test Structure

```
tests/
├── unit/
│   ├── controllers/
│   ├── middlewares/
│   └── utils/
├── integration/
│   ├── auth.test.js
│   └── tasks.test.js
└── setup.js
```

---

## 📝 Assignment Deliverables Checklist

### ✅ Backend (Primary Focus)

- [✅] User registration & login APIs with JWT
- [✅] Password hashing with bcrypt
- [✅] Role-based access control (user/admin)
- [✅] Task CRUD APIs (secondary entity)
- [✅] API versioning (`/api/v1/`)
- [✅] Input validation with e✅press-validator
- [✅] Error handling middleware
- [✅] Swagger API documentation
- [✅] MongoDB database schema
- [✅] Redis caching implementation
- [✅] Security best practices (Helmet, CORS)

### ✅ Frontend (Supportive)

- [✅] React.js UI with routing
- [✅] User registration form
- [✅] Login form with JWT handling
- [✅] Protected dashboard route
- [✅] Task CRUD interface
- [✅] Error/success message display
- [✅] Token storage and management

### ✅ Documentation & Deployment

- [✅] Comprehensive README.md
- [✅] Swagger/Postman collection
- [✅] Database schema documentation
- [✅] Scalability notes
- [✅] Docker deployment setup
- [✅] Environment variables guide
- [✅] Security implementation details

---

## 🚀 Live Demo & Repository

- **GitHub Repository**: [https://github.com/YashManek/PrimeTradeAI](https://github.com/YashManek1/PrimeTradeAI)
- **Live API**: [https://primetradeai-api.herokuapp.com](https://primetradeai-api.herokuapp.com)
- **Frontend Demo**: [https://primetradeai-app.netlify.app](https://primetradeai-app.netlify.app)
- **API Documentation**: [https://primetradeai-api.herokuapp.com/api-docs](https://primetradeai-api.herokuapp.com/api-docs)
- **Postman Collection**: [Download Link](https://documenter.getpostman.com/view/your-collection-id)

---

## 🎓 Learning Outcomes

This project demonstrates proficiency in:

- RESTful API design principles
- Authentication & authorization flows
- Database schema design and optimization
- Caching strategies for performance
- Security best practices
- Docker containerization
- Full-stack integration
- API documentation
- Scalable architecture patterns
- Error handling and logging
- Git version control

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style

- Follow ESLint configuration
- Write meaningful commit messages
- Add tests for new features
- Update documentation

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 📧 Contact & Support

**Developer**: Yash Manek
**Email**: yashmanek2001@gmail.com  
**GitHub**: [@YashManek1](https://github.com/YashManek1)  
**LinkedIn**: [Yash Manek](https://www.linkedin.com/in/yash-manek-/)

For issues and questions:

- Open an issue on [GitHub Issues](https://github.com/YashManek1/PrimeTradeAI/issues)
- Email: yashmanek2001@gmail.com

---

## 🙏 Acknowledgments

### Technologies & Libraries

- [Express.js](https://expressjs.com/) - Web framework
- [MongoDB](https://www.mongodb.com/) - Database
- [Redis](https://redis.io/) - Caching layer
- [Swagger](https://swagger.io/) - API documentation
- [React.js](https://react.dev/) - Frontend framework
- [JWT](https://jwt.io/) - Authentication tokens
- [bcrypt](https://github.com/kelektiv/node.bcrypt.js) - Password hashing
- [Pino](https://getpino.io/) - Logging

### Resources

- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [REST API Design Guidelines](https://restfulapi.net/)
- [OWASP Security Cheat Sheet](https://cheatsheetseries.owasp.org/)

---

**Built with ❤️ for the PrimeTrade.ai Backend Developer Internship Assignment**
