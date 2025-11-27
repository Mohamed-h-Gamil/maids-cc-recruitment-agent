# 🤖 Maids.cc AI Recruitment & Operations Automation

**A comprehensive automation system designed to streamline the recruitment lifecycle—from initial AI screening to visa issuance.**

![Project Status](https://img.shields.io/badge/Status-Prototype%20Complete-green)
![Tech Stack](https://img.shields.io/badge/Stack-n8n%20%7C%20Gemini%20%7C%20Notion%20%7C%20Telegram-blue)

## 📋 Executive Summary
This project demonstrates a scalable solution for high-volume recruitment agencies like **Maids.cc**. By integrating **Generative AI (Google Gemini)** with operational tools (**Notion, n8n**), this system eliminates manual bottlenecks in the candidate screening process, optimizes sales team workload via algorithmic assignment, and automates client communication regarding visa status.

## 🏗 System Architecture

The solution is composed of three distinct logical blocks that handle the data flow from "Lead" to "Placement."

<img width="2298" height="932" alt="maidscc demo" src="https://github.com/user-attachments/assets/af5dc1e8-6223-4bb8-b897-4007d03bc25c" />

-----

## 🚀 Key Modules

### Block 1: The AI Recruitment Agent

**Interface:** Telegram | **Brain:** Google Gemini 2.5 Flash
Unlike standard chatbots, this agent possesses **"Context Awareness"** of the live inventory.

  * **Inventory RAG (Retrieval):** The agent uses a custom tool (`get_maids`) to query a mock Google Sheets database in real-time. It only recommends maids who are currently "Available."
  * **Filter Output:** Filters the retrieved information to match the client's preferences.
  * **Respond to Client:** Presents candidates with maids Names, bios, and salaries directly in the chat.

### Block 2: Intelligent Workload Assignment

**Engine:** n8n | **Logic:** Round-Robin / Least-Load
Once a client selects a maid, the system does not randomly assign the lead. It performs an algorithmic lookup of the Sales Team:

1.  **Fetch Team Stats:** Retrieves all sales agents and their current `Active_Leads` count.
2.  **Sort & Select:** Sorts agents by ascending workload (Lowest → Highest).
3.  **Assign:** Picks the agent with the **least workload** to prevent burnout and ensure fast response times.
4.  **Update:** Increments that agent's load count in the database instantly.
5.  **Handoff:** Creates a ticket in **Notion** and emails the specific agent with the lead details.

### Block 3: Visa Lifecycle Tracking

**Engine:** n8n (Notion Trigger)
Automates the post-hiring communication loop.

  * **State Monitoring:** Watches the Notion database for status changes (e.g., `Processing` → `Visa Issued`).
  * **Client Notification:** Automatically triggers an email to the client when the visa status changes, reducing the need for clients to call customer support for updates.

-----

## 🛠️ Tech Stack & Tools

  * **Orchestration:** [n8n](https://n8n.io/) (Workflow Automation)
  * **LLM:** Google Gemini 2.5 Flash (via API)
  * **Database (Inventory):** Google Sheets
  * **CRM / Task Tracker:** Notion
  * **Frontend:** Telegram Bot API
  * **Notifications:** Gmail / SMTP

-----

## ⚙️ How to Run

1.  **Install n8n:** Run locally via `npm run n8n` or use the Cloud version.
2.  **Import Workflow:** Download `main_recruitment_agent.json` from this repo and import it into n8n.
3.  **Setup Credentials:**
      * **Telegram:** Get a token from @BotFather.
      * **Google:** Set up a Service Account for Sheets and an API Key for Gemini.
      * **Notion:** Create an Internal Integration token.
4.  **Prepare Data:** Upload the CSV files to your Google Drive and connect the Sheet nodes in n8n.

## 💼 Business Value Analysis

| Feature | Operational Impact |
| :--- | :--- |
| **AI Inventory Check** | Reduces "false hope" by ensuring clients only see currently available candidates. |
| **Load Balancing** | Increases sales conversion rates by ensuring leads go to agents with capacity to respond quickly. |
| **Auto-CRM Entry** | Eliminates \~10 minutes of manual data entry per lead. |
| **Visa Updates** | Reduces call center volume regarding "What is my visa status?" queries. |
