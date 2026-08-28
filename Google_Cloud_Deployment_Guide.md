# 🚀 Vibe Coding: Idea to Execution | My Google Workshop Experience

Recently, I had the incredible opportunity to participate in a Google Workshop centered around **"Vibe Coding - Idea to Execution."** The core concept? Using Agentic AI and the Model Context Protocol (MCP) to turn architectural ideas into fully deployed, enterprise-grade realities without manually clicking through endless cloud consoles.

I wanted to share my experience, the core learnings, and how I applied these concepts to build and deploy my full-stack application, **SkillStack**, entirely on Google Cloud Platform (GCP).

---

## 💡 The Workshop Experience: What is "Vibe Coding"?

"Vibe Coding" is the paradigm shift of using AI agents (like Antigravity) combined with standard protocols (like MCP) to orchestrate infrastructure. Instead of manually writing deployment scripts or navigating UI menus, I guided the AI to analyze my project context and execute the necessary commands. 

The **Model Context Protocol (MCP)** was the bridge. It allowed the AI to securely connect to my Google Cloud Run server context, understand my GCP resources, and execute `gcloud` CLI commands natively to provision everything from scratch. It felt like pair-programming with a Senior DevOps engineer!

---

## 🏗️ The Project: SkillStack

**SkillStack** is a decoupled full-stack application featuring a React/Vite frontend and a Java Spring Boot backend. 

My goal for the workshop was to deploy this architecture natively on GCP. Here is how we executed the idea using Vibe Coding:

### 1. The Database: Google Cloud SQL
To establish a robust data layer, we couldn't use serverless compute (which is stateless). We needed a persistent solution.
* **The Execution:** Using conversational commands, the AI utilized the `gcloud` CLI to provision a dedicated **Google Cloud SQL (MySQL 8.0)** instance and generated the database schema.

### 2. The Backend: Spring Boot on Cloud Run
The Java Spring Boot API was containerized and deployed to **Google Cloud Run**.
* **The Execution:** The AI automatically integrated the `mysql-socket-factory-connector-j-8` dependency into my `pom.xml`, allowing Spring Boot to connect to Cloud SQL securely over Google's internal network. We deployed it with `--min-instances 1` to eliminate cold starts and securely injected sensitive credentials via environment variables at runtime.

### 3. The Frontend: React/Vite on Cloud Run
To keep everything in a unified ecosystem, the frontend was also deployed natively on Cloud Run.
* **The Execution:** Since Cloud Run expects a web server, the AI wrote a multi-stage `Dockerfile`. It used Node to build the Vite code and an incredibly lightweight Nginx server to host the static files. Nginx was configured to handle React's client-side SPA routing perfectly on port 8080.

---

## 🧠 Key Technical Takeaways

By executing this deployment using Vibe Coding, I solidified several core software engineering principles:

1. **Decoupled Architecture Scalability:** 
   By deploying the Frontend and Backend as two separate Cloud Run services, they can scale independently. If the frontend gets heavy traffic, Google clones the frontend container without wasting backend compute resources.
2. **Statelessness vs. Stateful Design:** 
   Cloud Run is inherently stateless (data stored locally in the container is lost upon restart). This project reinforced the need to map file uploads (like certificates) to external stateful services in true production environments.
3. **Dynamic CORS Configuration:** 
   I learned how to securely configure backend `CORS_ALLOWED_ORIGINS` dynamically via environment variables to exclusively accept traffic from the newly deployed frontend.
4. **Infrastructure as Code (IaC) Mindset:** 
   By using MCP and CLI commands instead of a UI, the entire deployment process became documented, repeatable, and highly professional.

---

If you are a developer looking to bridge the gap between writing code and deploying enterprise infrastructure, I highly recommend diving into **Agentic AI** and **Google Cloud Run**. The gap between "Idea" and "Execution" has never been smaller!

*#GoogleCloud #CloudRun #VibeCoding #AgenticAI #SpringBoot #React #DevOps #SoftwareEngineering*
