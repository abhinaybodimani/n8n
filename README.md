# Automated AI Workflows & Data Pipelines

A centralized repository containing containerized automation workflows, data extraction pipelines, and AI-driven applications. These projects primarily leverage **n8n** for orchestration and local **Ollama** models within Docker to bypass cloud API rate limits and token costs.

## Repository Overview

This monorepo houses multiple independent automation projects. Each project resides in its own directory with a dedicated `README.md` detailing its specific setup, environment variables, and node configurations.

| Project | Description | Core Tech Stack |
| :--- | :--- | :--- |
| **[GroceryAIAgent](./GroceryAIAgent)** | An extraction pipeline that parses Danish supermarket flyers. It uses local vision models to identify promotional food items and archives the data idempotently in a PostgreSQL database. | n8n, Ollama (Qwen/Gemma), PostgreSQL |
| **[WhatsAppWisher](./WhatsAppWisher)** | An automated communication pipeline designed to parse contact dates and send scheduled WhatsApp greetings for birthdays and anniversaries. | n8n, JavaScript, Webhooks |
| **[DailyAINews](./DailyAINews)** | An automated aggregation workflow that collects, processes, and formats daily artificial intelligence news and system architecture updates. | n8n, LLMs, Web Scraping |

## Core Infrastructure & Tech Stack

These projects share a common technical philosophy centered around self-hosted, open-source infrastructure:

*   **Orchestration:** n8n (self-hosted) for routing, scheduling, and JavaScript-based data transformation.
*   **Artificial Intelligence:** Ollama running quantized local models (e.g., qwen2.5:3b, gemma3:4b) for text and vision tasks without external API dependencies.
*   **Database Management:** PostgreSQL utilizing CTEs for data retention and archival tracking.
*   **Deployment:** Docker and Docker Compose for isolated, reproducible environments.

## Getting Started

1.  Navigate to the specific project folder you wish to deploy.
2.  Review the project-level `README.md` for specific webhook setups, API requirements, or database schemas.
3.  Ensure your local Docker environment is running, particularly the Ollama container if the workflow requires local inference.
4.  Import the provided `.json` workflow files directly into your n8n instance.

---
*Maintained by Abhinay*
