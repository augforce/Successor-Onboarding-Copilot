# 🤖 SuccessorAgent
**Role:** IT Operations Transition Assistant  
**Stack:** Microsoft Foundry | GPT-4.1-mini | Azure AI Search (RAG)

## Governance & Privacy
This repository contains the system architecture and deployment logic. No internal company data, proprietary KBs, or private credentials are included in this public documentation.

## Project Overview
An intelligent RAG-based agent built on Microsoft Foundry designed to automate IT support onboarding and preserve institutional knowledge in high-growth technical environments.

## The Problem & Business Case
**The Challenge**: Establishing the first "Technology Support Engineer" role from scratch with no prior documentation.

**The Solution**: A hybrid RAG agent that "onboards" itself by indexing personal KBs, freeing up 15%+ of an engineer's weekly capacity by handling routine policy/procedure queries.

## 🛠️ Technical Implementation
- **LLM:** GPT-4.1-mini
- **RAG Architecture:** Integrated Azure AI Search with a manual Vector Store for document retrieval.
- **Prompt Engineering:** Designed a system persona to act as a mentor, citing specific KB documentation for every answer. If there is no documentation available for the support question, the agent will search the web for applicable steps/procedures, making sure to remind the requestor that they need to verify the instructions for accuracy and get clearance from their supervisor before implimenting.

## **Security and Identity:** 
To ensure the "SuccessorAgent" meets corporate security standards, the architecture follows the Principle of Least Privilege (PoLP):

- **Passwordless Authentication**: Leveraged Azure Managed Identities to facilitate secure communication between Azure AI Foundry, Azure AI Search, and Blob Storage. This eliminates the need for hardcoded API keys or connection strings within the codebase.

- **Granular Access Control**: Implemented Azure RBAC (Role-Based Access Control) to restrict resource access. The AI Agent is assigned specific roles (e.g., Search Index Data Reader) rather than broad Administrative permissions, ensuring data sovereignty and reducing the attack surface.

- **Data Privacy**: All grounding data remains within the company's Azure tenant, inheriting existing enterprise-grade encryption and compliance boundaries.

## 📈 Cost Optimization & Resource Efficiency
As this project utilizes a corporate Azure subscription, fiscal responsibility was a primary design requirement.

- **Proactive Monitoring**: Leveraged Azure Cost Management + Billing to set automated alerts, ensuring the project remained within a strictly defined "Sandbox" budget. Created a budget alert in Azure to trigger an email notice for each $5 increment.
- **Inference Optimization**: Selected GPT-4.1-mini for routine queries to balance high-reasoning capabilities with low token costs.
- **Storage Efficiency**: Utilized a Basic/Standard tier Azure AI Search index, optimized with specific indexing schedules to prevent unnecessary compute charges during off-hours.
- **Sustainability**: Designed the RAG pipeline to be "Plug-and-Play," meaning the expensive compute resources can be paused or scaled down when the agent is not in active use.
