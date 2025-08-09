### Super-Agent: An n8n-Powered Automation Engine

### Project Overview

The Super-Agent project is a sophisticated, multi-agent AI system built on the powerful n8n automation platform. Its purpose is to understand and execute complex development and administrative workflows from simple, natural language commands. By orchestrating a wide array of external tools and services, this system acts as an intelligent, AI-powered assistant for a modern tech stack.

Unlike traditional single-purpose AI tools, this system uses a modular, multi-agent architecture within n8n to break down complex requests into a series of coordinated, specialized tasks.

### Key Capabilities

The system is designed to handle high-level, conversational requests and translate them into concrete actions, such as:

 * "Create a dashboard from the latest tasks in our Go High Level CRM."
 * "Write a backend service for a new feature, containerize it with Docker, create a GitHub repository, and deploy it to Google Cloud Run."
 * "Set up a new Next.js project, connect it to our Supabase database, and deploy it on Vercel."
This is achieved through several core capabilities:
 * Modular Tool Integration: A robust framework for connecting to a diverse set of APIs and services.
   * VCS & CI/CD: Git, GitHub
   * Cloud & Hosting: Vercel, Google Cloud Run, Docker
   * LLM & AI Services: Gemini API, 11Labs (for voice)
   * SaaS/PaaS: Go High Level (CRM), Supabase (Database)
   * Custom Infrastructure: MCP Servers
 * Workflow Orchestration: Using n8n's visual, node-based editor, complex requests are broken down and routed to specialized "agent" workflows.
 * State Management & Context: The system maintains context across multi-step and long-running workflows, tracking progress, handling errors, and ensuring that agents have the necessary information.
 * Natural Language & Voice Interaction: The primary interface is conversational, leveraging LLMs to understand user intent and 11Labs for natural, spoken responses.
Architecture: A Multi-Agent Approach with n8n
This project is built using n8n, an extensible workflow automation tool. The multi-agent system is not a single, monolithic piece of code but rather a collection of interconnected n8n workflows, each with a distinct role.
 * The Orchestrator (Project Manager Agent): This is the main workflow that acts as the "brain." It receives a user command (e.g., from a voice-to-text service), uses an AI Agent node to interpret the request, and then routes the task to the appropriate specialized agent workflow via an Execute Workflow node.
 * Specialized Agent Workflows: Each agent is a separate n8n workflow designed for a specific domain:
   * Software Engineer Agent: Handles code generation, repository creation, and code commits using Gemini and GitHub nodes.
   * DevOps Engineer Agent: Manages infrastructure tasks like building Docker images and deploying to cloud services via HTTP Request nodes.
   * Go High Level Agent: Interacts with the Go High Level API to manage and retrieve CRM data.
   * Data Engineer Agent: Connects to the Supabase database to manage data, handle transformations, and perform vector database queries for RAG.
This modular design allows for clear separation of concerns, making the system highly scalable, maintainable, and robust.
Key Technical Features
 * Context Management with RAG: To keep agent conversations and actions grounded, a Retrieval-Augmented Generation (RAG) system is implemented. An n8n workflow processes documents, generates vector embeddings using a LLM, and stores them in a dedicated Supabase vector table. Specialized agents can query this knowledge base to retrieve relevant context on-demand.
 * Long-Running Processes: For tasks that take more than a few seconds, a state management system is in place. Workflows use Wait nodes to pause execution and save their state to a database. They can be resumed by external events (e.g., a webhook notification from a cloud provider that a deployment is complete), ensuring that context is not lost.
 * Robust Error Handling & Logging: A dedicated "Error Handler" workflow is configured to automatically run whenever an execution fails in any other workflow. This centralized system performs the following actions:
   * Logs the error details (timestamp, failing node, error message) to a database or file.
   * Sends real-time notifications to a Slack channel or via email for immediate attention.
   * Can be configured to automatically retry non-critical tasks.
 * Secure API Credential Management: All API keys and secrets are securely stored and managed within n8n's built-in credential system, avoiding hardcoding sensitive information.
   
 ### Getting Started
 * Clone the Repository:
   git clone https://github.com/your-username/Super-Agent-n8n.git
cd Super-Agent-n8n

 * Deploy n8n with Docker:
   The recommended approach is to self-host n8n using Docker for full control. Follow the official n8n documentation for the most secure and up-to-date setup instructions.
   * Prerequisites: Docker and Docker Compose installed.
   * Run the provided docker-compose.yml file to spin up your n8n instance and a PostgreSQL database.
 * Import Workflows:
   * Navigate to your n8n instance in a browser (http://localhost:5678 by default).
   * Import the .json workflow files provided in this repository. These files contain the logic for the Orchestrator, specialized agents, RAG system, and the error handler.
 * Configure Credentials:
   * In the n8n UI, go to Settings > Credentials.
   * Create new credentials for each service (Gemini, 11Labs, GitHub, Supabase, etc.) and paste your API keys and tokens.
 * Start the Workflows:
   * Activate the "Orchestrator," "Error Handler," and other necessary workflows. The system is now ready to receive commands and begin its work.
     
### Contributing

We welcome contributions to this project! If you'd like to improve the system, add a new agent, or fix a bug, please follow these steps:
 * Fork the repository.
 * Create a new branch (git checkout -b feature/new-agent).
 * Commit your changes (git commit -am 'feat: add new Agent for X').
 * Push to the branch (git push origin feature/new-agent).
 * Create a new Pull Request.
   
