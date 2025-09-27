# SEO Keyword Opportunity Agent

This project is a fully autonomous AI agent designed and built as a solution for the AI Agent Developer competency assessment from Callus Company Inc.

The agent automates the entire SEO keyword discovery process. It takes a single "seed" keyword as input, uses a multi-step process of AI-powered ideation and live data enrichment, and produces a final, prioritized list of the top 50 keyword opportunities. The results are strategically ranked to identify keywords with the best balance of high search volume and low competition, helping strategists find topics they can realistically rank for on Google.

---

## Core Features

*   **Semantic Deconstruction:** The agent doesn't just look at keywords; it first uses an LLM to understand the core concepts, user personas, and motivations behind the seed topic.
*   **Strategic Keyword Expansion:** It leverages the semantic analysis to brainstorm a diverse list of ~100 candidate keywords, covering multiple user intents (informational, commercial, transactional).
*   **Live Data Enrichment:** Each candidate keyword is enriched with real-world SEO metrics by calling a professional-grade API (DataForSEO). The agent intelligently pivots to related, higher-value keywords discovered by the API.
*   **Robust Data Parsing:** A custom data cleaning module with built-in fallback logic handles messy or incomplete API responses, ensuring the agent is resilient and reliable.
*   **Strategic Opportunity Scoring:** A custom formula, `Score = (Volume^2) / (Difficulty + 1)`, is used to rank the final list, mathematically prioritizing keywords that represent the best return on investment.

---

## Technology Stack

*   **Orchestration:** **n8n.io** - Used to visually build, connect, and automate the agent's multi-stage workflow.
*   **AI Ideation:** **OpenAI API** - Used for the initial semantic analysis and the strategic generation of keyword candidates.
*   **Data Enrichment:** **DataForSEO API** - Used as the source of truth for factual SEO metrics like monthly search volume and keyword difficulty.

---

## Workflow Architecture

The agent is designed as a sequential pipeline that mimics an expert's thought process:

1.  **Deconstruction:** An LLM deconstructs the seed keyword into a rich JSON object of concepts and user personas.
2.  **Expansion:** A second LLM uses this JSON context to generate a diverse list of ~100 keyword ideas.
3.  **Splitting:** A `Code` node splits the raw text from the LLM into an array of individual items.
4.  **Enrichment (`API Pull`):** The workflow iterates through each keyword, calling the DataForSEO API to get a rich report of related keyword ideas and their metrics.
5.  **Parsing (`Data Checkpoint`):** A custom `Code` node parses the complex API response for each item, extracting the key data points (search volume, difficulty) and handling any missing values.
6.  **Scoring & Sorting (`Result`):** A final `Code` node gathers the list of ~100 clean keyword objects, calculates the "Opportunity Score" for each, sorts the entire list, and returns the top 50.

---

## Getting Started

### Prerequisites

*   An active **n8n** instance (self-hosted via Docker or on n8n Cloud).
*   API Keys for **OpenAI** and **DataForSEO**.

### Installation & Setup

1.  **Clone the Repository (Optional):**
    ```bash
    git clone [URL of your new repository]
    ```

2.  **Import the Workflow:**
    *   In your n8n canvas, go to `File` > `Import from File...`.
    *   Select the `n8n_script.json` file included in this repository. The complete agent workflow will appear on your canvas.

3.  **Add Credentials:**
    *   **OpenAI:** Double-click the two **`Message a model`** nodes. In the "Credential to connect with" dropdown, add your OpenAI API key.
    *   **DataForSEO:** Double-click the **`API Pull`** (`HTTP Request`) node. In the `Authentication` section, create a `Basic Auth` credential using your DataForSEO email as the **User** and your API Key as the **Password**.

## How to Run the Agent

1.  Click the main **"Execute workflow"** button at the top of the n8n canvas.
2.  An **`Edit Fields`** dialog will appear, prompting you for a `seed_keyword`. Enter any topic (e.g., "Digital Marketing Course") and click "Execute".
3.  The agent will run through all the phases. This may take a few minutes as it makes ~100 API calls.
4.  When the execution is complete, click on the final node named **`Result`** to view the final, sorted list of the top 50 keyword opportunities in the output panel.

---
