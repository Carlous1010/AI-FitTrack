# AI FitTrack API 🏋️‍♂️🤖

AI FitTrack API is an AI-powered fitness management backend application built using **Node.js, Express.js, MongoDB with Mongoose, JWT Authentication, bcryptjs**, and the **Google Gemini AI SDK**.

It enables users to register and authenticate securely, manage their workout activities, search workout records, and request personalized AI workout recommendations and fitness insights through Google Gemini AI.

---

## 🎨 Tech Stack

- **Runtime Environment:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB Atlas via Mongoose ODM
- **AI Integration:** Google Gemini SDK (`@google/genai`)
- **Security & Authentication:** JWT (JSON Web Token), bcryptjs
- **Middleware & Logging:** CORS, Morgan
- **Environment Configuration:** dotenv
- **API Testing:** Thunder Client and Postman
- **Development Environment:** Visual Studio Code

---

## 📁 MVC Architecture

```text
AI-FitTrack/
│
├── src/
│   ├── config/
│   │   └── db.js
│   │
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── workoutController.js
│   │   └── aiController.js
│   │
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Workout.js
│   │
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── workoutRoutes.js
│   │   ├── aiRoutes.js
│   │   └── index.js
│   │
│   ├── services/
│   │   └── geminiService.js
│   │
│   ├── app.js
│   └── server.js
│
├── .env.example
├── .gitignore
├── package.json
├── package-lock.json
├── README.md
└── FitTrack.postman_collection.json
````

### Architecture Components

* **Config:** Handles database connection configuration.
* **Controllers:** Contains authentication, workout, and AI application logic.
* **Middleware:** Provides JWT authentication and centralized error handling.
* **Models:** Defines MongoDB schemas using Mongoose.
* **Routes:** Defines REST API endpoints.
* **Services:** Provides the Google Gemini AI integration.
* **App:** Configures the Express application.
* **Server:** Starts the backend server.

---

# 🚀 Getting Started

## 1. Prerequisites

The following software and services are required:

* **Node.js**
* **npm**
* **MongoDB Atlas or MongoDB**
* **Google Gemini API Key**
* **Visual Studio Code or another code editor**

## 2. Installation

Clone the repository or navigate to the project directory and run:

```bash
npm install
```

This installs all dependencies specified in `package.json`.

## 3. Configuration Setup

Create a local `.env` file in the project root using `.env.example` as the reference.

Required environment variables:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
GEMINI_API_KEY=your_google_gemini_api_key
GEMINI_MODEL=your_gemini_model
```

The actual `.env` file is excluded from the repository to protect sensitive credentials.

## 4. Running the Server

Start the development server:

```bash
npm run dev
```

Or start the application normally:

```bash
npm start
```

The API runs on:

```text
http://localhost:5000
```

The root endpoint can be used to verify the server:

```text
GET /
```

A successful response confirms that the AI FitTrack API is running.

---

# 🛠️ API Specifications & Reference

## 🔐 Feature 1: Authentication

### Register User

* **URL:** `/api/auth/register`
* **Method:** `POST`
* **Authentication Required:** No

**Request Body:**

```json
{
  "name": "John Doe",
  "email": "john@gmail.com",
  "password": "123456"
}
```

**Response:**

```json
{
  "success": true,
  "token": "JWT_TOKEN",
  "user": {
    "id": "USER_ID",
    "name": "John Doe",
    "email": "john@gmail.com"
  }
}
```

### Login User

* **URL:** `/api/auth/login`
* **Method:** `POST`
* **Authentication Required:** No

**Request Body:**

```json
{
  "email": "john@gmail.com",
  "password": "123456"
}
```

**Response:**

```json
{
  "success": true,
  "token": "JWT_TOKEN",
  "user": {
    "id": "USER_ID",
    "name": "John Doe",
    "email": "john@gmail.com"
  }
}
```

### User Profile

* **URL:** `/api/auth/profile`
* **Method:** `GET`
* **Authentication Required:** Yes

The JWT token is supplied using Bearer authentication.

**Response:**

```json
{
  "success": true,
  "user": {
    "id": "USER_ID",
    "name": "John Doe",
    "email": "john@gmail.com"
  }
}
```

---

# 🏃‍♂️ Feature 2: Workout Management

All workout operations are protected by JWT authentication and are associated with the authenticated user.

### Add Workout

* **URL:** `/api/workouts`
* **Method:** `POST`
* **Authentication Required:** Yes

**Request Body:**

```json
{
  "workoutName": "Evening Jog",
  "category": "Running",
  "duration": 30,
  "caloriesBurned": 350,
  "workoutDate": "2026-09-27"
}
```

Supported workout categories include:

* `Cardio`
* `Strength Training`
* `Yoga`
* `Running`
* `Cycling`
* `Walking`

### View All Workouts

* **URL:** `/api/workouts`
* **Method:** `GET`
* **Authentication Required:** Yes

Returns the workout records belonging to the authenticated user.

### View Workout by ID

* **URL:** `/api/workouts/:id`
* **Method:** `GET`
* **Authentication Required:** Yes

Retrieves a specific workout using its unique identifier.

### Update Workout

* **URL:** `/api/workouts/:id`
* **Method:** `PUT`
* **Authentication Required:** Yes

Updates an existing workout record.

**Example Request Body:**

```json
{
  "workoutName": "Evening Cycling",
  "category": "Cycling",
  "duration": 45,
  "caloriesBurned": 380,
  "workoutDate": "2026-09-27"
}
```

### Delete Workout

* **URL:** `/api/workouts/:id`
* **Method:** `DELETE`
* **Authentication Required:** Yes

Deletes an existing workout record associated with the authenticated user.

---

# 🔍 Feature 3: Workout Search

### Search Workouts

* **URL:** `/api/workouts/search`
* **Method:** `GET`
* **Authentication Required:** Yes

The search functionality supports workout name, category, and date-based searches.

### Search by Workout Name

```text
GET /api/workouts/search?q=Morning
```

### Search by Category

```text
GET /api/workouts/search?q=Cycling
```

### Search by Date

```text
GET /api/workouts/search?q=Cycling&date=2026-09-27
```

The search functionality is scoped to the authenticated user's workout records.

---

# 🧠 Feature 4: AI Workout Recommendation

### Get AI Recommendation

* **URL:** `/api/ai/workout-recommendation`
* **Method:** `POST`
* **Authentication Required:** Yes

The endpoint generates personalized workout guidance using the user's age, fitness goal, and experience level.

**Request Body:**

```json
{
  "age": 24,
  "fitnessGoal": "Weight loss",
  "experience": "Beginner"
}
```

The request is processed through the Gemini service and returns an AI-generated workout recommendation.

**Example Response:**

```json
{
  "recommendation": "AI-generated personalized workout recommendation"
}
```

---

# 📊 Feature 5: AI Fitness Insights

### Get Fitness Insights

* **URL:** `/api/ai/fitness-insights`
* **Method:** `POST`
* **Authentication Required:** Yes

The endpoint analyzes workout performance information and generates AI-powered fitness insights.

**Request Body:**

```json
{
  "totalWorkouts": 18,
  "averageDuration": 42,
  "totalCaloriesBurned": 5200
}
```

The request is processed through the Gemini service.

**Example Response:**

```json
{
  "insight": "AI-generated fitness performance insights"
}
```

---

# 🧪 API Testing

The project was tested using Thunder Client and Postman.

The testing sequence includes:

```text
1. Register User
        ↓
2. Login User
        ↓
3. Access Protected Profile
        ↓
4. Add Workout
        ↓
5. Get All Workouts
        ↓
6. Search Workout
        ↓
7. Get Workout by ID
        ↓
8. Update Workout
        ↓
9. AI Workout Recommendation
        ↓
10. AI Fitness Insights
        ↓
11. Delete Workout
```

The API testing covered authentication, workout CRUD operations, search functionality, and both AI endpoints.

---

# 📦 Postman Collection

An importable Postman collection is included in the project root:

```text
FitTrack.postman_collection.json
```

The collection contains requests for:

* Authentication
* Workout management
* Workout search
* AI workout recommendation
* AI fitness insights

The collection also contains variables and request scripts used during API testing.

---

# 🔐 Security

AI FitTrack implements the following security mechanisms:

* JWT-based authentication
* Protected API routes
* Password hashing using bcryptjs
* User-specific workout access
* Environment variables for sensitive configuration
* Centralized error handling

Sensitive credentials such as the MongoDB connection string, JWT secret, and Gemini API key are not included in the public repository.

---

# 🗄️ Database

MongoDB is used for persistent data storage through Mongoose.

The current implementation contains two primary models.

## User Model

The User model stores user account and authentication-related information.

Main fields include:

* `_id`
* `name`
* `email`
* `password`
* `createdAt`
* `updatedAt`

## Workout Model

The Workout model stores workout information including:

* `_id`
* `user`
* `workoutName`
* `category`
* `duration`
* `caloriesBurned`
* `workoutDate`
* `createdAt`
* `updatedAt`

Each workout record is associated with its authenticated user.

---

# 🤖 Google Gemini AI Integration

The project integrates Google Gemini AI through the dedicated service:

```text
src/services/geminiService.js
```

The service provides two AI capabilities.

## AI Workout Recommendation

Uses:

```text
Age
Fitness Goal
Experience Level
```

to generate personalized workout recommendations.

## AI Fitness Insights

Uses:

```text
Total Workouts
Average Duration
Total Calories Burned
```

to generate fitness performance insights.

The AI service is separated from the controllers to maintain modularity within the backend architecture.

---

# 📂 Project Documentation

The repository is organized according to the eight project phases:

```text
01 - Brainstorming and Ideation
02 - Requirement Analysis
03 - Project Design
04 - Project Planning
05 - Project Development
06 - Project Testing
07 - Project Documentation
08 - Project Demonstration
```

Each phase contains the documentation and supporting materials related to that stage of the project.

---

# 📋 Phase-wise Project Organization

## Phase 1 – Brainstorming and Ideation

Contains documentation related to the project idea, problem identification, objectives, initial scope, and proposed AI FitTrack concept.

## Phase 2 – Requirement Analysis

Contains functional requirements, non-functional requirements, technology requirements, system scope, and limitations.

## Phase 3 – Project Design

Contains technical architecture, ER diagram, DFD diagrams, user flow, MVC architecture, and other system design materials.

## Phase 4 – Project Planning

Contains project planning, task allocation, development sequence, milestones, testing planning, and documentation planning.

## Phase 5 – Project Development

Contains development documentation and the implemented project source code.

## Phase 6 – Project Testing

Contains testing documentation and screenshots covering authentication, workout APIs, search operations, and AI APIs.

## Phase 7 – Project Documentation

Contains API documentation and academic project documentation.

## Phase 8 – Project Demonstration

Contains project demonstration documentation and the final demonstration video link.

---

# 📁 Repository Structure

```text
AI-FitTrack/
│
├── README.md
│
├── 01_Brainstorming_and_Ideation/
│   └── 01_Brainstorming_and_Ideation.pdf
│
├── 02_Requirement_Analysis/
│   └── 02_Requirement_Analysis.pdf
│
├── 03_Project_Design/
│   ├── 03_Project_Design.pdf
│   ├── Technical_Architecture.png
│   ├── ER_Diagram.png
│   ├── DFD_Level_0.png
│   ├── DFD_Level_1.png
│   ├── User_Flow.png
│   └── MVC_Diagram.png
│
├── 04_Project_Planning/
│   └── 04_Project_Planning.pdf
│
├── 05_Project_Development/
│   ├── 05_Project_Development.pdf
│   ├── source-code/
│   ├── Project_Structure.png
│   ├── Server_Setup.png
│   └── Database_Setup.png
│
├── 06_Project_Testing/
│   ├── 06_Project_Testing.pdf
│   ├── Authentication/
│   ├── Workout/
│   └── AI/
│
├── 07_Project_Documentation/
│   ├── 07_Project_Documentation.pdf
│   ├── API_Documentation.pdf
│   └── AI_FitTrack_Academic_Documentation.pdf
│
└── 08_Project_Demonstration/
    ├── 08_Project_Demonstration.pdf
    └── Demo_Video_Link.txt
```

---

# 🎥 Project Demonstration

The final project demonstration presents the working AI FitTrack application.

The demonstration covers:

* Project introduction
* Project purpose
* Technology stack
* Project architecture
* Project structure
* Backend server execution
* MongoDB connection
* User registration
* User login
* Protected profile access
* Workout creation
* Workout retrieval
* Workout search
* Workout update
* AI workout recommendation
* AI fitness insights
* Workout deletion
* Final project workflow

## Google Drive Demonstration Video

The final demonstration video is hosted on Google Drive with link-based viewing access.

**Video Link:**

PASTE_YOUR_GOOGLE_DRIVE_VIDEO_LINK_HERE

---

# 📚 Additional Documentation

The repository contains documentation covering the complete project lifecycle, including:

* Brainstorming and ideation
* Requirement analysis
* Project design
* Project planning
* Project development
* Project testing
* API documentation
* Academic project documentation
* Project demonstration documentation

---

# 🚀 Future Enhancements

Possible future extensions include:

* Dedicated React frontend
* Cloud deployment
* Advanced fitness progress tracking
* Additional fitness-related data models
* Notification functionality
* Advanced administrative functionality
* Production monitoring and optimization

These features are considered future enhancements and are not represented as part of the current core implementation.

---

# 👥 Team Members

## AI FitTrack Team

* **Chandra Sekar G.**
* **Chandru G T**
* **Chandru M**
* **Boopathi B L**
* **Chandan Kumar B**

**Department:** Computer Science
**Institution:** M.G.R. College

---

# 📌 Project Information

| Item                    | Details                                                          |
| ----------------------- | ---------------------------------------------------------------- |
| Project Name            | AI FitTrack – Personalized Fitness Recommendations Powered by AI |
| Project Type            | AI-Powered Fitness Management Backend                            |
| Runtime                 | Node.js                                                          |
| Framework               | Express.js                                                       |
| Database                | MongoDB                                                          |
| ODM                     | Mongoose                                                         |
| Authentication          | JWT                                                              |
| Password Security       | bcryptjs                                                         |
| Artificial Intelligence | Google Gemini AI                                                 |
| API Testing             | Thunder Client / Postman                                         |
| Development Environment | Visual Studio Code                                               |

---

# 📊 Project Outcome

AI FitTrack demonstrates the integration of:

```text
Secure Authentication
        +
Workout Management
        +
Workout Search
        +
MongoDB Data Persistence
        +
AI Workout Recommendations
        +
AI Fitness Insights
        =
AI FitTrack
```

The completed backend provides a structured REST API for fitness management while extending the system with AI-generated workout recommendations and fitness insights.

---

# 📝 Conclusion

AI FitTrack provides a structured backend solution for fitness and workout management with integrated artificial intelligence capabilities.

The application combines Node.js, Express.js, MongoDB, Mongoose, JWT authentication, bcryptjs password hashing, and Google Gemini AI into a modular REST API architecture.

The project demonstrates the development lifecycle from brainstorming and requirement analysis through system design, planning, development, testing, documentation, and final project demonstration.

---

## 🔗 Project Links

### GitHub Repository

This repository contains the AI FitTrack source code, phase-wise documentation, testing evidence, diagrams, API documentation, and project documentation.

### Project Demonstration Video

The final project demonstration video is available through the Google Drive link provided in the Project Demonstration section above.

```
```
