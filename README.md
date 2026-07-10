# 🦆 ducktAIpe-sprint

<p align="center">
  <img src="app/frontend/logo.png" alt="ducktAIpe Logo" width="300">
</p>

[![Live Demo](https://img.shields.io/badge/Live%20Demo-ducktAIpe--Sprint-FFD700?style=for-the-badge&logo=google-cloud&logoColor=white)](https://ducktaipe-frontend-sprint-311115041393.us-central1.run.app/)

**ducktAIpe-sprint** is an open-source, reference implementation of a multi-agent system designed for the **Google Agentic Architect Sprint 2026**. 

This fork builds on top of the original `ducktAIpe` codebase to showcase advanced **Governance, Reliability, and Safety** by implementing an **Interactive Human-in-the-Loop (HITL) Approval Gate** using an escrow-style state suspension model.

Built with Google's **Agent Development Kit (ADK)** and the **Agent-to-Agent (A2A)** protocol, the orchestrator coordinates a secure, stateful network of independent agents to research technical problems and compile repair guides, checking in with the user before executing final generation tasks.

---

## 🛠 How it Works (The Workflow)

1. **🚪 Gatekeeper:** Evaluates the user's input (image or text) to ensure there is enough visual and textual context to generate a guide.
2. **🔍 Researcher:** Gathers technical repair documentation and failure points using DuckDuckGo web search and scrapers.
3. **⚖️ Judge:** Performs quality control, evaluating findings for technical accuracy and safety warnings.
4. **⏸️ HITL Approval Gate (New!):** If the findings pass the Judge, the Orchestrator pauses, saves the findings into the session database, and sets the status to `awaiting_approval`. The frontend renders the summary and waits for manual approval.
5. **🦆 Content Builder:** Once approved, the Orchestrator resumes and delegates final compilation to the Content Builder to build a structured markdown guide complete with images, step-by-step instructions, and a relevant YouTube tutorial.

---

## 🚀 Getting Started

### Prerequisites

* **[uv](https://docs.astral.sh/uv/)**: Python package and project manager.
* **Google Cloud Project**: Required for Vertex AI (Gemini).
* **gcloud CLI**: Authenticated and configured to your active Google Cloud project.

### Local Setup & Execution

1. **Configure credentials:**
   Ensure you are logged in to your Google Cloud project:
   ```bash
   gcloud auth application-default login
   ```

2. **Initialize the environment:**
   Recreate the virtual environments and install dependencies:
   ```bash
   uv sync
   ```

3. **Start the local services:**
   Run the local execution script:
   ```bash
   ./run_local.sh
   ```
   This will start the Gatekeeper (8005), Researcher (8001), Judge (8002), Content Builder (8003), MCP Server (8888), Orchestrator (8004), and the Web Frontend (8000).

4. **Access the Web Interface:**
   Open your browser and navigate to:
   👉 **[http://localhost:8000](http://localhost:8000)**

---

## 🧪 Testing

* **MCP Server Test:** Verify that the scrapers, web search, and media tools are running correctly:
  ```bash
  uv run test_mcp.py
  ```

* **Individual Agents:** Every agent runs as a standalone FastAPI microservice. You can run them independently in their directories (e.g. `agents/researcher/`) using `uv run adk_app.py`.

---

## ☁️ Cloud Deployment

The project is containerized and ready to deploy to Google Cloud Run. We have adapted the deployment scripts to suffix all service names with `-sprint` so that they do not conflict with or overwrite any active production deployments of `ducktAIpe`.

To build and deploy all services to Cloud Run:
```bash
./deploy.sh
```
This script will build images, push them to the Google Artifact Registry, deploy the private agent cards, link them to the Orchestrator, and expose the public `ducktaipe-frontend-sprint` service.

---

*Made with ❤️ for the Google Agentic Architect Sprint 2026.*
