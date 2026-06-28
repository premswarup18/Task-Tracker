# Task Tracker - MERN Stack Implementation Plan

## Project Overview
A full-stack Task Tracker web application built with the MERN Stack (MongoDB, Express.js, React.js, Node.js) demonstrating complete CRUD operations, REST APIs, and responsive UI design.

---

## Phase 1: Project Setup & Infrastructure

### 1.1 Repository Structure
```
Task-Tracker/
├── backend/                    # Node.js + Express Server
│   ├── config/
│   │   └── db.js              # MongoDB connection configuration
│   ├── models/
│   │   └── Task.js            # Task schema definition
│   ├── routes/
│   │   └── taskRoutes.js       # API endpoints
│   ├── controllers/
│   │   └── taskController.js   # Business logic
│   ├── middleware/
│   │   ├── errorHandler.js     # Error handling middleware
│   │   └── validators.js       # Input validation
│   ├── .env.example
│   ├── .gitignore
│   ├── package.json
│   └── server.js               # Entry point
├── frontend/                   # React Application
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── TaskForm.jsx          # Create/Edit task form
│   │   │   ├── TaskList.jsx          # Display tasks
│   │   │   ├── TaskItem.jsx          # Individual task card
│   │   │   ├── TaskFilter.jsx        # Filter & sort (bonus)
│   │   │   └── Notification.jsx      # Toast notifications (bonus)
│   │   ├── pages/
│   │   │   └── Home.jsx              # Main page
│   │   ├── services/
│   │   │   └── api.js                # API calls (axios)
│   │   ├── styles/
│   │   │   └── App.css               # Responsive styles
│   │   ├── App.jsx
│   │   └── index.js
│   ├── .env.example
│   ├── .gitignore
│   ├── package.json
│   └── vite.config.js (or react-scripts)
├── .gitignore
└── README.md
```

### 1.2 Backend Setup
```bash
# Initialize backend
mkdir backend
cd backend
npm init -y
npm install express cors dotenv mongoose axios
npm install --save-dev nodemon

# Create directory structure
mkdir config models routes controllers middleware
```

### 1.3 Frontend Setup
```bash
# Initialize frontend
cd ..
npx create-react-app frontend
# OR
npm create vite@latest frontend -- --template react

cd frontend
npm install axios react-router-dom
npm install --save-dev tailwindcss postcss autoprefixer
```

---

## Phase 2: Backend Development

### 2.1 Database Configuration (backend/config/db.js)
```javascript
Requirements:
- Connect to MongoDB Atlas or local MongoDB
- Handle connection errors gracefully
- Export connection for use in server.js
```

**Implementation Steps:**
- [ ] Create MongoDB Atlas cluster (or use local MongoDB)
- [ ] Copy connection string
- [ ] Create `.env` file with `MONGODB_URI` and `PORT`
- [ ] Write connection logic with error handling
- [ ] Test connection on server startup

### 2.2 Task Model (backend/models/Task.js)
```javascript
Schema fields:
- _id: ObjectId (auto-generated)
- title: String (required, min 3 chars)
- description: String
- status: Enum ['pending', 'in-progress', 'completed']
- priority: Enum ['low', 'medium', 'high']
- dueDate: Date
- createdAt: Date (auto)
- updatedAt: Date (auto)
```

**Implementation Steps:**
- [ ] Define Mongoose schema with validation
- [ ] Add default values (status: 'pending', createdAt: now())
- [ ] Create model and export
- [ ] Add indexes on frequently queried fields

### 2.3 Task Controller (backend/controllers/taskController.js)
```
Methods to implement:
1. getAllTasks() - Fetch all tasks with optional filtering/sorting
2. getTaskById() - Fetch single task
3. createTask() - Create new task
4. updateTask() - Update existing task
5. deleteTask() - Delete task
6. bulkDelete() - Delete multiple tasks (bonus)
```

**Implementation Steps:**
- [ ] Create controller file with try-catch error handling
- [ ] Implement each CRUD operation
- [ ] Add proper HTTP status codes
- [ ] Add logging for debugging

### 2.4 Input Validation (backend/middleware/validators.js)
```
Validate:
- Title: required, 3-100 characters
- Description: optional, max 500 characters
- Status: must be one of allowed values
- Priority: must be one of allowed values
- DueDate: valid date format if provided
- No XSS/injection attacks
```

**Implementation Steps:**
- [ ] Use express-validator library
- [ ] Create validation middleware for each endpoint
- [ ] Return 400 with error messages on validation failure

### 2.5 API Routes (backend/routes/taskRoutes.js)
```
Endpoints:
- GET    /api/tasks           - Get all tasks (with query params for filter/sort)
- GET    /api/tasks/:id       - Get task by ID
- POST   /api/tasks           - Create task
- PUT    /api/tasks/:id       - Update task
- DELETE /api/tasks/:id       - Delete task
- DELETE /api/tasks           - Bulk delete (bonus)
```

**Implementation Steps:**
- [ ] Define routes with correct HTTP methods
- [ ] Attach validation middleware
- [ ] Link to controller methods
- [ ] Test with Postman/Insomnia

### 2.6 Error Handling (backend/middleware/errorHandler.js)
```
Handle:
- 400 Bad Request (validation errors)
- 404 Not Found (task doesn't exist)
- 500 Internal Server Error
- MongoDB errors (duplicate keys, type errors)
```

**Implementation Steps:**
- [ ] Create global error handler middleware
- [ ] Add custom error class
- [ ] Handle async errors with try-catch
- [ ] Return consistent error response format

### 2.7 Backend Entry Point (backend/server.js)
```javascript
Setup:
- Load environment variables
- Connect to MongoDB
- Initialize Express app
- Mount middleware (cors, json, error handler)
- Mount routes
- Start server on PORT
```

**Implementation Steps:**
- [ ] Create server.js with all configurations
- [ ] Add CORS configuration
- [ ] Add request logging middleware
- [ ] Test all endpoints with Postman

---

## Phase 3: Frontend Development

### 3.1 API Service (frontend/src/services/api.js)
```javascript
Functions:
- fetchTasks()       - GET all tasks
- fetchTaskById()    - GET single task
- createTask()       - POST new task
- updateTask()       - PUT existing task
- deleteTask()       - DELETE task
- Centralized error handling
- Retry logic (optional)
```

**Implementation Steps:**
- [ ] Create axios instance with base URL
- [ ] Implement all API functions with try-catch
- [ ] Handle loading and error states
- [ ] Add request/response interceptors

### 3.2 Task Form Component (frontend/src/components/TaskForm.jsx)
```
Features:
- Input fields: title, description, status, priority, dueDate
- Form validation (client-side)
- Submit button (Create/Update based on mode)
- Cancel button
- Error display
- Loading state
```

**Implementation Steps:**
- [ ] Create component with useState for form state
- [ ] Add form validation logic
- [ ] Connect to API service
- [ ] Add success/error notifications
- [ ] Handle edit mode (pre-fill fields)

### 3.3 Task List Component (frontend/src/components/TaskList.jsx)
```
Features:
- Display list of tasks
- Loading skeleton
- Empty state
- Refresh button
- Pass tasks to TaskItem components
```

**Implementation Steps:**
- [ ] Fetch tasks on component mount
- [ ] Implement useEffect for data fetching
- [ ] Add loading and error states
- [ ] Display task items in grid/list format

### 3.4 Task Item Component (frontend/src/components/TaskItem.jsx)
```
Features:
- Display task information (title, description, status, priority)
- Edit button (opens form)
- Delete button (with confirmation)
- Status badge with color coding
- Priority indicator
```

**Implementation Steps:**
- [ ] Create reusable card component
- [ ] Add edit/delete handlers
- [ ] Display task data with formatting
- [ ] Add visual indicators for status/priority

### 3.5 Filter & Sort Component (frontend/src/components/TaskFilter.jsx) - BONUS
```
Features:
- Filter by status (pending, in-progress, completed)
- Filter by priority (low, medium, high)
- Sort by: created date, due date, priority
- Reset filters button
- Real-time filtering
```

**Implementation Steps:**
- [ ] Create filter state management
- [ ] Add filter UI controls
- [ ] Pass filter params to API
- [ ] Update task list dynamically

### 3.6 Notification Component (frontend/src/components/Notification.jsx) - BONUS
```
Features:
- Toast notifications
- Types: success, error, warning, info
- Auto-dismiss after 3-5 seconds
- Close button
- Position: top-right
```

**Implementation Steps:**
- [ ] Create context for notifications
- [ ] Implement notification component
- [ ] Add show/hide logic
- [ ] Use in API responses

### 3.7 Main App Component (frontend/src/App.jsx)
```
Layout:
- Header with title
- Task Form (create mode)
- Task Filter (optional)
- Task List
- Footer
- Global notification display
```

**Implementation Steps:**
- [ ] Create main layout structure
- [ ] Manage global state (tasks, loading, error)
- [ ] Implement CRUD operation handlers
- [ ] Add responsive design

### 3.8 Styling & Responsiveness
```
Requirements:
- Mobile-first design (320px+)
- Tablet: 768px+
- Desktop: 1024px+
- Smooth animations
- Consistent color scheme
- Accessible UI (ARIA labels, semantic HTML)
```

**Implementation Steps:**
- [ ] Use Tailwind CSS or CSS Grid/Flexbox
- [ ] Test on multiple screen sizes
- [ ] Add media queries for responsive behavior
- [ ] Ensure accessibility compliance

---

## Phase 4: Integration & Testing

### 4.1 Environment Variables Setup
```
Backend (.env):
MONGODB_URI=mongodb+srv://...
PORT=5000
NODE_ENV=development
CORS_ORIGIN=http://localhost:3000

Frontend (.env):
REACT_APP_API_URL=http://localhost:5000/api
```

**Implementation Steps:**
- [ ] Create .env files from .env.example
- [ ] Add to .gitignore
- [ ] Update backend/server.js to use env vars
- [ ] Update frontend/services/api.js to use env vars

### 4.2 API Testing
```
Test each endpoint:
- GET /api/tasks (empty, with data)
- POST /api/tasks (valid, invalid data)
- GET /api/tasks/:id (valid, invalid ID)
- PUT /api/tasks/:id (valid, invalid data)
- DELETE /api/tasks/:id
```

**Implementation Steps:**
- [ ] Use Postman/Insomnia collection
- [ ] Test all CRUD operations
- [ ] Test validation errors
- [ ] Test 404 and 500 errors

### 4.3 Frontend-Backend Integration
```
Test:
- Form submission creates task
- List updates automatically
- Edit form pre-fills data
- Delete removes task from list
- Status/priority changes work
- Error messages display
```

**Implementation Steps:**
- [ ] Run backend and frontend concurrently
- [ ] Test complete user workflows
- [ ] Check browser console for errors
- [ ] Test on different browsers

### 4.4 CORS Configuration
```
Backend:
- Allow frontend origin
- Allow credentials if needed
- Allow required HTTP methods
```

**Implementation Steps:**
- [ ] Configure cors() middleware
- [ ] Test requests from frontend
- [ ] Debug CORS errors

---

## Phase 5: Deployment

### 5.1 Backend Deployment (Choose One)
```
Options:
1. Heroku
2. Railway
3. Render
4. DigitalOcean/AWS
5. Vercel (serverless)
```

**Implementation Steps for Heroku:**
- [ ] Create Heroku account
- [ ] Install Heroku CLI
- [ ] Create `Procfile` in backend: `web: node server.js`
- [ ] Push to Heroku: `git push heroku main`
- [ ] Set environment variables in Heroku dashboard
- [ ] Get deployed backend URL

**Implementation Steps for Railway:**
- [ ] Connect GitHub repo to Railway
- [ ] Set root directory to `/backend`
- [ ] Add environment variables
- [ ] Deploy and get URL

### 5.2 Frontend Deployment (Choose One)
```
Options:
1. Vercel (recommended for React)
2. Netlify
3. GitHub Pages (static only)
4. AWS S3 + CloudFront
```

**Implementation Steps for Vercel:**
- [ ] Connect GitHub repo to Vercel
- [ ] Set root directory to `/frontend`
- [ ] Add environment variable: `REACT_APP_API_URL=<backend_url>`
- [ ] Deploy and get URL

**Implementation Steps for Netlify:**
- [ ] Connect GitHub repo
- [ ] Set build command: `npm run build`
- [ ] Set publish directory: `build`
- [ ] Add environment variables
- [ ] Deploy

### 5.3 Database Deployment
```
MongoDB Atlas:
- Create free tier cluster
- Add IP whitelist (0.0.0.0 for development)
- Create database user with strong password
- Get connection string
- Add to backend .env
```

**Implementation Steps:**
- [ ] Sign up for MongoDB Atlas
- [ ] Create cluster
- [ ] Create database and collection
- [ ] Create user credentials
- [ ] Add connection string to backend

### 5.4 Domain Configuration (Optional)
```
Steps:
- Purchase domain (GoDaddy, Namecheap, etc.)
- Point nameservers to hosting provider
- Configure SSL certificate
- Test HTTPS connection
```

---

## Bonus Features Implementation

### B1. Task Filtering & Sorting
```
Features:
- Filter by status (pending, in-progress, completed)
- Filter by priority (low, medium, high)
- Sort by due date, priority, created date
- Combine multiple filters
- Reset filters
```

### B2. Notifications
```
Features:
- Toast notifications for success/error
- Auto-dismiss
- Multiple notification types
- Context API for global state
```

### B3. Advanced UI Components
```
Features:
- Calendar view for due dates
- Drag-and-drop to change status
- Batch operations (select multiple)
- Dark mode toggle
- Search functionality
```

### B4. Backend Enhancements
```
Features:
- User authentication (JWT)
- Pagination with limit/offset
- Analytics endpoints
- Rate limiting
- Request logging
```

---

## Development Checklist

### Backend
- [ ] MongoDB connection configured
- [ ] Task model created with validation
- [ ] All CRUD controllers implemented
- [ ] API routes defined
- [ ] Input validation middleware added
- [ ] Error handling middleware implemented
- [ ] CORS configured
- [ ] Environment variables setup
- [ ] API tested with Postman
- [ ] Deployed to hosting platform

### Frontend
- [ ] API service created with axios
- [ ] TaskForm component built
- [ ] TaskList component built
- [ ] TaskItem component built
- [ ] Filter component built (bonus)
- [ ] Notification system implemented (bonus)
- [ ] Responsive design implemented
- [ ] Environment variables configured
- [ ] Integration tested
- [ ] Deployed to hosting platform

### Testing
- [ ] All CRUD operations work
- [ ] Form validation works
- [ ] Error handling tested
- [ ] Responsive design tested (mobile, tablet, desktop)
- [ ] Cross-browser testing done
- [ ] Accessibility checked
- [ ] Performance tested

---

## Technology & Dependencies

### Backend
```json
{
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.0",
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "express-validator": "^7.0.0",
    "axios": "^1.3.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.20"
  }
}
```

### Frontend
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "axios": "^1.3.0",
    "react-router-dom": "^6.10.0"
  },
  "devDependencies": {
    "vite": "^4.2.0",
    "tailwindcss": "^3.2.7",
    "postcss": "^8.4.24",
    "autoprefixer": "^10.4.14"
  }
}
```

---

## Timeline Estimate

| Phase | Duration | Tasks |
|-------|----------|-------|
| Phase 1: Setup | 1-2 hours | Project structure, dependencies |
| Phase 2: Backend | 6-8 hours | DB, models, controllers, routes, validation |
| Phase 3: Frontend | 6-8 hours | Components, styling, integration |
| Phase 4: Testing | 3-4 hours | Integration, debugging, optimization |
| Phase 5: Deployment | 2-3 hours | Deploy backend, frontend, database |
| **Bonus Features** | **2-4 hours** | Filters, notifications, advanced UI |
| **Total** | **20-29 hours** | Complete implementation |

---

## Key Learning Outcomes

Upon completion, you will understand:
- ✅ Full-stack web application development
- ✅ REST API design and implementation
- ✅ Database design and MongoDB operations
- ✅ React component architecture and state management
- ✅ Form handling and validation
- ✅ Error handling and logging
- ✅ CORS and security basics
- ✅ Deployment and DevOps fundamentals
- ✅ Responsive and accessible UI design

---

## Troubleshooting Guide

### Common Issues & Solutions

#### MongoDB Connection Failed
```
Problem: Cannot connect to MongoDB
Solution:
1. Check MongoDB URI in .env
2. Verify IP whitelist in MongoDB Atlas
3. Check username/password
4. Test connection string in MongoDB Compass
```

#### CORS Errors
```
Problem: Cross-Origin Request Blocked
Solution:
1. Check CORS middleware in server.js
2. Verify frontend URL in CORS_ORIGIN
3. Test with curl: curl -H "Origin: ..." backend-url
```

#### API Not Responding
```
Problem: Frontend cannot reach backend
Solution:
1. Check if backend server is running
2. Verify API_URL in frontend .env
3. Check network tab in DevTools
4. Test endpoint with Postman
```

#### Form Not Submitting
```
Problem: Form submission fails silently
Solution:
1. Check browser console for errors
2. Verify API endpoint URL
3. Check request payload in Network tab
4. Verify backend validation rules
```

---

## Next Steps
1. Clone this repository
2. Create `backend` and `frontend` directories
3. Follow Phase 1 setup instructions
4. Implement Phase 2-3 sequentially
5. Test thoroughly in Phase 4
6. Deploy in Phase 5
7. Add bonus features if time permits

Good luck! 🚀
