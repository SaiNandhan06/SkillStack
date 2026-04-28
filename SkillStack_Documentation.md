# 📚 SkillStack: Comprehensive Project Documentation

Welcome to the complete documentation for the **SkillStack** project! This guide is written to help you deeply understand the entire project, from the big picture down to the smallest file. Whether you are reviewing the code or preparing for a project presentation, this document will give you a complete understanding of how everything works together.

---

## 1. 🧠 Project Overview

### What does SkillStack do?
SkillStack is a full-stack web application designed to help users track their professional skills, set goals, and manage certifications. It also includes an administrative dashboard for managing users and verifying certifications. Think of it as a digital, interactive portfolio and career tracker.

### Technology Stack
The project is divided into two main parts: the Frontend (what the user sees) and the Backend (the logic and data processing).

*   **Frontend Technologies:**
    *   **React:** The core JavaScript library used to build the interactive user interface.
    *   **Vite:** A blazing-fast build tool and development server.
    *   **Tailwind CSS:** A utility-first CSS framework for rapid and modern styling.
    *   **Radix UI / Shadcn:** Accessible, pre-built components for a polished look.
*   **Backend Technologies:**
    *   **Java & Spring Boot:** The robust framework powering the server, API, and business logic.
    *   **Spring Security & JWT:** Used for securing the app and managing user logins.
    *   **Hibernate / Spring Data JPA:** Tools that translate Java code into database commands.
*   **Database:**
    *   **MySQL:** A reliable relational database used to store all users, skills, and certifications.

### High-Level Architecture
SkillStack uses a **Decoupled Client-Server Architecture**. 
This means the Frontend and Backend are entirely separate applications. The React frontend acts as the "Client". Whenever it needs data (like a list of skills) or wants to save data (like a new user registration), it sends an HTTP request over the internet to the Spring Boot "Server" (the API). The server processes the request, talks to the MySQL database, and sends a response back to the frontend.

---

## 2. 📁 Complete Folder Structure

Here is the complete folder structure of the project, represented as a tree, with explanations for what each folder does.

```text
SkillStack/
│
├── frontend/                     # 🖥️ Everything related to the User Interface
│   ├── public/                   # Static files (like favicons or images) served directly to the browser
│   ├── src/                      # The main source code for the React application
│   │   ├── components/           # Reusable UI building blocks (Buttons, Navbars, Cards)
│   │   ├── context/              # Global state management (sharing data across components without passing props)
│   │   ├── data/                 # Static mock data or configuration constants
│   │   ├── hooks/                # Custom React logic (reusable functions starting with 'use')
│   │   └── pages/                # The actual screens of the app (Login.jsx, Dashboard.jsx, etc.)
│   ├── package.json              # List of all Node.js libraries the frontend uses
│   └── vite.config.js            # Configuration for the Vite bundler
│
└── skillstack-backend/           # ⚙️ Everything related to Server Logic & Database
    ├── src/main/java/com/skillstack/
    │   ├── config/               # Security rules, API documentation setups, and CORS policies
    │   ├── controller/           # The "Receivers". They catch requests from the frontend and send responses
    │   ├── dto/                  # Data Transfer Objects (Custom objects to shape the data sent/received securely)
    │   ├── entity/               # Java representations of database tables (e.g., a User class = a User table)
    │   ├── exception/            # Custom error handling (so the app doesn't crash, it sends a neat error message)
    │   ├── filter/               # Security checkpoints (like checking if a user has a valid JWT token)
    │   ├── repository/           # The layer that directly talks to the MySQL Database
    │   └── service/              # The "Brain". Contains the core business logic (e.g., verifying a password)
    ├── src/main/resources/
    │   └── application.properties# Central config file for database URLs, passwords, and server ports
    └── pom.xml                   # List of all Java libraries (Maven dependencies) the backend uses
```

---

## 3. 📄 File-by-File Explanation

### 🖥️ Frontend Files

*   **`frontend/src/main.jsx`**: The ultimate entry point of the frontend. It attaches the React application to the HTML file (`index.html`).
*   **`frontend/src/App.jsx`**: The routing hub. It defines which URL shows which page (e.g., navigating to `/login` renders the `Login.jsx` page component).
*   **`frontend/src/api.js`**: **CRITICAL FILE.** This is the communication setup. It configures `axios` (the tool used to make API calls) with the base URL of your backend. It also automatically attaches the user's security token to every request.
*   **`frontend/package.json`**: The instruction manual for Node.js. It lists all dependencies (React, Tailwind, Axios) and defines commands like `npm run dev`.
*   **`frontend/vite.config.js`**: Configures how Vite builds the project, setting up the development server and allowing easy imports using aliases (like `@/components`).
*   **`frontend/src/pages/*.jsx`**: These are the main screens of your app. For example, `AdminDashboard.jsx` is the page the admin sees when they log in.

### ⚙️ Backend Files

*   **`skillstack-backend/src/.../SkillStackApplication.java`**: The main execution point for the Java backend. Running this file starts the Spring Boot server.
*   **`skillstack-backend/src/main/resources/application.properties`**: **CRITICAL FILE.** This holds the environment variables for the backend, such as the Database URL, Database Password, Port number, and Email SMTP configuration.
*   **`skillstack-backend/pom.xml`**: Maven's instruction manual. It downloads the necessary Java libraries (Spring Web, Spring Security, MySQL Connector).
*   **Controllers (e.g., `AuthController.java`)**: These files define your API endpoints. `AuthController` contains the code that runs when someone hits the `/api/v1/auth/login` endpoint.
*   **Services (e.g., `CertificationService.java`)**: Where the actual work happens. If a user uploads a certification, the controller passes it to the service, which validates the data before saving it.
*   **Repositories (e.g., `UserRepository.java`)**: Interfaces that extend Spring Data JPA. You don't even have to write SQL queries; you just call methods like `findByEmail()`, and Spring writes the SQL for you!

---

## 4. 🌐 API Integration (How they talk)

### Where API calls are made (Frontend)
In the frontend, whenever data is needed, components use the pre-configured `axios` instance from `src/api.js`. 
For example, in a login page, you might see: `api.post('/auth/login', userData)`.

### Where API routes are defined (Backend)
In the backend, routes are defined in the **Controllers** using annotations. 
For example, in `AuthController.java`, you will see `@PostMapping("/login")` inside a class annotated with `@RequestMapping("/api/v1/auth")`.

### Axios / Fetch Setup & Base URL
The file `frontend/src/api.js` uses `axios.create()`. It relies on an environment variable (`VITE_API_BASE_URL`) to know where the backend lives. 
*   In development, it points to `http://localhost:8080/api/v1`.
*   In production, it points to your Render backend URL.

### The Full Request Flow:
1.  **Frontend:** User clicks "Save Goal". React calls `api.post('/goals', goalData)`.
2.  **API Service (`api.js`):** Axios intercepts the request, grabs the user's JWT token from local storage, and adds it to the headers.
3.  **Backend Route:** The HTTP request travels to `https://your-backend.com/api/v1/goals`.
4.  **Controller:** `GoalController.java` receives the JSON data.
5.  **Service & Repository:** The Controller passes data to `GoalService`, which uses `GoalRepository` to save the data to MySQL.
6.  **Response:** The backend sends a `201 Created` success message back over the internet.
7.  **Frontend Update:** Axios receives the success message, and React updates the screen to show the new goal.

---

## 5. 🔐 Middleware Explanation

Middleware is code that runs *in the middle* of a request, before it reaches its final destination (the Controller).

*   **Authentication Middleware (`JwtAuthenticationFilter.java`):** This is the security guard. When a request arrives (e.g., a user trying to view their profile), this filter stops the request, looks for a JWT token in the header, and verifies its cryptographic signature. If the token is fake or expired, it rejects the request with a `401 Unauthorized` error.
*   **Security Config (`SecurityConfig.java`):** This defines the rules for the security guard. It says things like: *"Allow anyone to access `/auth/login`, but require a valid token for `/api/v1/certifications`."*
*   **Error Handling (`exception/` folder):** If the code crashes (e.g., trying to find a user that doesn't exist), this middleware catches the crash and translates it into a clean JSON error message, rather than sending a messy Java stack trace to the frontend.

---

## 6. 🗄️ Database Connection

*   **Database Used:** MySQL (A powerful relational database).
*   **How connection is established:** Spring Boot uses Hibernate (an ORM - Object Relational Mapper). Hibernate allows you to write Java classes (`Entity`), and it automatically creates the corresponding SQL tables.
*   **Configuration File:** The connection is managed in `application.properties`.

### The Flow:
**Backend Starts** → Reads `application.properties` → Finds the `DB_URL`, `DB_USERNAME`, and `DB_PASSWORD` → Spring Boot establishes a secure connection pool to the MySQL database → Hibernate checks the Java Entities and updates the SQL tables if needed (`spring.jpa.hibernate.ddl-auto=update`).

---

## 7. 🌍 Environment Variables

Environment variables are secret settings that change depending on where the app is running (your laptop vs. the live internet).

### Frontend Variables
*   **`VITE_API_BASE_URL`**: Used in `api.js`. It tells the React app exactly where to send API requests. 
    *   *Why needed?* Because locally, the backend is on `localhost:8080`, but in production, it's on a live server (e.g., Render).

### Backend Variables
*   **`PORT`**: The port the backend server runs on (usually 8080).
*   **`DB_URL` / `DB_USERNAME` / `DB_PASSWORD`**: The address and login credentials for your MySQL database.
*   **`JWT_SECRET`**: A long, random string used to digitally sign user login tokens. If a hacker doesn't have this secret, they cannot forge a login token.
*   **`MAIL_USERNAME` / `MAIL_PASSWORD`**: Your SMTP credentials (like a Gmail account) used by the backend to send automated emails (verifications, etc.).
*   **`CORS_ALLOWED_ORIGINS`**: A security setting telling the backend exactly which frontend URLs are allowed to talk to it.

### Deployment Platforms
*   **Vercel (Frontend Hosting):** You must enter `VITE_API_BASE_URL` in Vercel's settings so the live frontend knows how to reach the live backend.
*   **Render (Backend Hosting):** You must enter the `DB_URL`, `JWT_SECRET`, and `CORS_ALLOWED_ORIGINS` (pointing to your Vercel URL) in Render's environment settings.
*   **Railway (Database Hosting):** Railway generates your live MySQL database. It provides you with the `DB_URL` and passwords that you then paste into Render.

---

## 8. 🚀 Deployment Architecture

### Separation of Concerns
The frontend, backend, and database are hosted on entirely different servers.
*   **Frontend (Vercel):** Acts as a global CDN. It serves the HTML, CSS, and JS files to the user's browser at lightning speed.
*   **Backend (Render):** A cloud server continuously running your Java application, waiting for HTTP requests.
*   **Database (Railway):** A managed MySQL database server dedicated to storing data securely.

### Communication & CORS
Because the frontend (e.g., `skillstack.vercel.app`) and backend (e.g., `skillstack-api.onrender.com`) live on different domains, browsers enforce a security rule called **CORS** (Cross-Origin Resource Sharing). The Spring Boot backend must explicitly be configured (`CORS_ALLOWED_ORIGINS`) to trust requests coming from the Vercel URL.

---

## 9. ⏱️ Monitoring (Uptime)

*   **UptimeRobot:** A free service used to monitor the application.
*   **Which endpoint is monitored:** Usually a specific health check endpoint (like `/api/v1/health`) or the base backend URL.
*   **Why it is needed:** Free hosting platforms like Render put your backend server to "sleep" if no one uses it for 15 minutes. When the next user visits, they experience a massive delay (30-50 seconds) while the server "wakes up." UptimeRobot automatically pings the server every 10 minutes to trick the platform into keeping your backend awake permanently.

---

## 10. 🔄 Real Data Flow (Step-by-Step)

Let's trace a user logging into the application:

1.  **User Action:** User types email and password on the React Frontend and clicks "Login".
2.  **Frontend (React/Axios):** React captures the data, packages it into JSON, and uses Axios to send an HTTP POST request to the API.
3.  **API (Internet):** The JSON data travels securely over HTTPS to the Render backend.
4.  **Backend (Controller):** `AuthController.java` receives the login request.
5.  **Backend (Service & DB):** `AuthService` queries the MySQL database (via `UserRepository`) to find the user by email. It then securely compares the hashed password.
6.  **Backend (JWT Generation):** If passwords match, the backend generates a secure JWT string, stamping it with the `JWT_SECRET`.
7.  **Response:** The backend sends the JWT back to the frontend.
8.  **UI Update:** The frontend saves the JWT to `localStorage` (to stay logged in) and React redirects the user to the `Dashboard.jsx` page.

---

## 11. 🏗️ Why This Architecture & Structure?

*   **Why separate Frontend and Backend?**
    *   *Scalability:* If the UI gets heavy traffic, you can upgrade Vercel independently. If data processing is heavy, you upgrade Render independently.
    *   *Flexibility:* If you ever want to build an iOS/Android app later, the Spring Boot API is already built! The mobile app can just connect to the exact same backend endpoints as the React web app.
*   **Why this Folder Structure?**
    *   *Maintainability:* By separating concerns (Controllers for routes, Services for logic, Repositories for DB), you always know exactly where a bug is hiding.
    *   *Modularity:* Changes in the UI don't require restarting or touching the Java server, and vice versa.

---

## 12. 🧾 Final Simple Summary

> *"SkillStack is a modern web application built in three separate layers. The user interacts with a fast, beautiful **Frontend built with React**, which acts as the face of the app. Whenever the user does something—like logging in or saving a skill—the Frontend sends a secure message over the internet to the **Backend built with Java Spring Boot**. The Backend acts as the brain; it verifies security, processes the logic, and saves or retrieves information from a **MySQL Database**. This separated architecture ensures the application is highly secure, easily scalable, and ready for professional deployment."*
