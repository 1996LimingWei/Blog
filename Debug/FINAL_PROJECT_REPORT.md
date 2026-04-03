# COMP 7940 Cloud Computing - Final Project Report

## HKBU AI Assistant: A Cloud-Native Telegram Bot with RAG and Conversation Memory

---

**Bot ID:** @Papapap2025bot (t.me/Papapap2025bot)

**Team Members:**
- Liming Wei (Student ID: [To be filled])
- Maxi (Student ID: [To be filled])
- Matt (Student ID: [To be filled])

---

## 1. Executive Summary

This project presents the development and deployment of HKBU AI Assistant, a cloud-native conversational AI system designed to serve the Hong Kong Baptist University community. The assistant operates as a Telegram bot, providing expert information on university-related topics including courses, faculty, campus buildings, and administrative procedures. The system incorporates several advanced technologies: Retrieval-Augmented Generation (RAG) for knowledge grounding, conversation memory for contextual continuity, Google Maps integration for location services, and comprehensive cloud infrastructure with monitoring capabilities.

The application is fully containerized using Docker and deployed through GitHub Container Registry (GHCR) with automated CI/CD pipelines. The production environment runs on Kubernetes clusters with integrated alerting and notification systems. All conversational data is persisted in Azure Cosmos DB with MongoDB API compatibility, and cost monitoring alerts are configured to maintain operational expenses below $10 USD monthly.

---

## 2. Project Overview and Technical Architecture

### 2.1 System Components

The HKBU AI Assistant follows a microservices architecture comprising three primary components:

**Telegram Bot Service (Python-telegram-bot)** serves as the user-facing interface, handling message reception, response formatting with MarkdownV2 support, and session management. This component maintains in-memory user session mappings while delegating processing to the backend service.

**ChatGPT Service (Flask API)** functions as the core processing engine, implementing the RAG pipeline, LLM integration with HKBU's GPT-5-mini deployment, conversation history retrieval and summarization, and Google Maps link generation for location queries.

**Data Persistence Layer (Azure Cosmos DB)** stores conversation logs with automatic summarization for long interactions, maintaining contextual continuity across sessions while respecting token budget constraints.

### 2.2 Knowledge Base and RAG Implementation

The RAG system indexes approximately 647 knowledge chunks from five distinct sources: 127 bilingual FAQ pairs covering admissions and academic policies, 73 sections from a comprehensive campus guide, 36 building records with GPS coordinates, 52 course catalog entries organized by department, and 359 faculty directory entries. These documents are embedded using the all-MiniLM-L6-v2 model (384-dimensional vectors) and stored in ChromaDB with cosine similarity indexing.

At query time, the system retrieves the top-5 most semantically similar chunks and injects them into the LLM system prompt, enabling grounded responses that minimize hallucination while providing accurate, source-traceable information.

### 2.3 Conversation Memory and Summarization

The system implements a dual-layer memory architecture. Recent conversations (up to six messages) are retained in full fidelity, while older exchanges undergo periodic summarization using the LLM itself. This approach maintains bounded token consumption regardless of conversation length while preserving essential context. The summarization trigger activates when conversation history exceeds six messages, compressing older content into two to three sentences that capture key facts and decisions.

### 2.4 Google Maps Integration

For location-related queries, the system extracts building coordinates from the RAG-retrieved context and generates interactive Google Maps links. Users receive clickable "View on Google Maps" and "Get Walking Directions" options, with optional static map images when API quotas permit. This feature transforms abstract coordinate data into practical navigation assistance.

---

## 3. Team Member Responsibilities

**Liming Wei** led data collection and RAG implementation, sourcing and structuring HKBU knowledge base materials including FAQ documents, building directories, and course catalogs. Implemented the ChromaDB vector store with embedding generation and similarity search. Developed the Google Maps service module for location query enhancement. Configured database integration with Azure Cosmos DB and established cost monitoring alerts. Set up CI/CD pipeline integration through GitHub Actions for automated container builds and GHCR publishing.

**Maxi** designed the overall system architecture, establishing service boundaries and communication protocols between components. Implemented Telegram bot integration with proper message handling, session management, and MarkdownV2 formatting. Led Kubernetes cluster deployment with service orchestration, load balancing, and auto-scaling configurations. Established Docker containerization for both services with optimized image layering and multi-stage builds. Configured cluster-level monitoring, alerting, and notification systems for operational visibility.

**Matthew** contributed to initial project ideation and requirement analysis, documenting functional specifications and user interaction flows. Authored comprehensive project documentation including technical specifications and user guides. Conducted code reviews across all modules to ensure quality and consistency. Performed end-to-end testing of integrated components, validating RAG retrieval accuracy, conversation memory functionality, and Google Maps link generation.

---

## 4. Cloud Resource Utilization and Cost Management

The project leverages multiple Azure cloud services with careful cost monitoring:

**Azure Cosmos DB (MongoDB API)** provides managed NoSQL database services for conversation persistence. The database is configured with provisioned throughput optimized for the expected query patterns, with automatic scaling disabled to prevent unexpected charges.

**Azure Kubernetes Service (AKS)** hosts the containerized application workloads with node pools sized appropriately for the development and demonstration scope.

**Azure Monitor and Alerts** track resource consumption with thresholds configured to trigger notifications when monthly expenditure approaches the $10 USD limit.

*[Screenshot of Azure billing dashboard to be attached]*

---

## 5. Technical Requirements Fulfillment

### 5.1 Containerization and Orchestration

Both services are containerized with Docker using Python 3.11-slim base images. The chatgpt-service image includes the ChromaDB vector store and knowledge base data files, while the telegram-bot image contains the messaging interface code. Images are built and published to GitHub Container Registry through automated GitHub Actions workflows triggered on pull requests and main branch pushes.

### 5.2 CI/CD Pipeline

The GitHub Actions workflow (`docker-build.yml`) automates the complete build and deployment pipeline: repository checkout, GHCR authentication using personal access tokens, Docker image building for both services, and image publishing with repository-derived tagging. This ensures consistent, reproducible deployments aligned with version control state.

### 5.3 Database Integration

Conversation persistence is implemented through PyMongo with Azure Cosmos DB compatibility. The schema stores messages with session identifiers, timestamps, user metadata, and role designations. Queries filter by session_id with in-application sorting to accommodate Cosmos DB limitations. The summarization algorithm maintains conversation context without unbounded growth.

### 5.4 Monitoring and Alerting

Kubernetes deployments include health checks, resource limits, and liveness probes. Azure Monitor tracks database throughput consumption and storage growth. Alert rules notify the team when metrics exceed defined thresholds, enabling proactive cost management and operational intervention.

*[Screenshots of Kubernetes dashboard, Azure Monitor, and Telegram bot interactions to be attached]*

---

## 6. Conclusion

The HKBU AI Assistant demonstrates successful integration of modern cloud-native technologies to deliver a practical, knowledge-grounded conversational AI service. The RAG architecture ensures factual accuracy while the conversation memory system provides contextual continuity. Full containerization and Kubernetes deployment enable scalable, maintainable operations. Cost monitoring and alerting maintain financial predictability suitable for academic project constraints.

The modular architecture separates concerns effectively, allowing independent scaling and maintenance of components. The knowledge base can be extended with additional HKBU information sources, and the conversation memory system can be enhanced with more sophisticated summarization or cross-session knowledge transfer in future iterations.

---

## Appendix: Evidence of Technical Implementation

*[Screenshots to be attached:]*
- Telegram bot conversation showing RAG responses
- Google Maps integration for building queries
- Azure Cosmos DB data explorer showing conversation records
- Kubernetes pod status and service endpoints
- GitHub Actions workflow execution history
- Azure cost analysis and billing dashboard
