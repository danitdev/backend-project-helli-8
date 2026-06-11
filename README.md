# Backend Project Helli-8

A full-featured Node.js backend server with authentication, news/article management, user search functionality, and admin capabilities.

## 🚀 Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Authentication**: JWT-based
- **File Upload**: Multer (image upload)
- **Database**: MongoDB (assumed based on model structure)
- **Package Manager**: npm


## 🗄️ Database Models

| Model | Purpose | Key Fields |
|-------|---------|-------------|
| **User.js** | Authentication & profiles | `name`, `email`, `password` (hashed), `role` (user/admin) |
| **MainNew.js** | News/articles | `title`, `content`, `author`, `images`, `likes`, `comments` |
| **Upload.js** | File tracking | `filename`, `path`, `uploadedBy`, `relatedModel` |

## 📡 API Routes

### 1. Authentication (`login.js`)
Base path: `/api/auth`

| Method | Endpoint | Middleware | Description |
|--------|----------|------------|-------------|
| POST | `/register` | - | Create new user |
| POST | `/login` | - | Login & receive JWT |
| GET | `/me` | `auth` | Get current user profile |
| POST | `/forgot-password` | - | Request password reset |
| POST | `/reset-password` | - | Reset password |

### 2. News Management (`news.js`)
Base path: `/api/news`

| Method | Endpoint | Middleware | Description |
|--------|----------|------------|-------------|
| POST | `/` | `auth` + `uploadimg` | Create article with images |
| GET | `/` | - | Get all published articles |
| GET | `/:id` | - | Get single article |
| PUT | `/:id` | `auth` | Update your article |
| DELETE | `/:id` | `auth` + `admin` | Delete any article (admin) |
| POST | `/:id/like` | `auth` | Like/unlike article |
| POST | `/:id/comment` | `auth` | Add comment |

### 3. Search (`search.js`)
Base path: `/api/search`

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/?q=...` | Search across all content |
| GET | `/news?q=...` | Search only articles |
| GET | `/users?q=...` | Search users |
| GET | `/by-tag/:tag` | Find articles by tag |

### 4. User Management (`users.js`)
Base path: `/api/users`

| Method | Endpoint | Middleware | Description |
|--------|----------|------------|-------------|
| GET | `/` | `auth` + `admin` | List all users (admin) |
| GET | `/:id` | `auth` | Get user profile |
| PUT | `/:id` | `auth` | Update profile |
| DELETE | `/:id` | `auth` + `admin` | Delete user (admin) |
| GET | `/:id/stats` | `auth` | Get user statistics |
| PUT | `/profile/image` | `auth` + `uploadimg` | Upload profile picture |

## 🔐 Authentication Flow

1. **Register** → User created with hashed password
2. **Login** → Server returns JWT token
3. **Authenticated Requests** → Include token in header:  
   `Authorization: Bearer <your_jwt_token>`
4. **Protected Routes** → `auth.js` middleware validates token
5. **Admin Routes** → `admin.js` checks `role === 'admin'`

## 📤 Image Upload

- **Middleware**: `uploadimg.js`
- **Storage**: `/upload` directory
- **Limits**: Images only, max size (check your config)
- **Usage**: Include `multipart/form-data` with field name `image` or `images`

## 🛠️ Installation & Setup

### Prerequisites
- Node.js 
- MongoDB
- npm

### Steps

```bash
# Clone the repository
git clone https://github.com/danitdev/Backend-Project-helli-8.git
cd Backend-Project-helli-8/the\ project\ 1.2

# Install dependencies
npm install

# Configure environment (create .env file)
# PORT=3000
# MONGODB_URI=mongodb://localhost:27017/helli8_db
# JWT_SECRET=your_super_secret_key

# Start MongoDB
mongod

# Run the app
npm run dev   # Development with auto-reload
npm start     # Production
