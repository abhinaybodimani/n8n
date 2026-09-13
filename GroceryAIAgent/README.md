# AI Grocery Agent: eTilbudsavis Automated Pipeline

An automated, containerized n8n workflow designed to extract, process, and archive active Danish grocery flyers from eTilbudsavis. The system utilizes local vision-language models to parse promotional pricing directly from dynamically signed flyer images and stores the structured data in PostgreSQL.

## System Architecture & Tech Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Orchestration** | n8n | Workflow automation, HTTP fetching, and JavaScript data transformation |
| **AI / OCR** | Ollama (Qwen2.5-VL / Gemma 3) | Local vision model inference for parsing item names and prices |
| **Database** | PostgreSQL | Relational storage utilizing CTEs for archiving and latest deals |
| **Infrastructure**| Docker | Containerized deployment to manage local APIs and eliminate token costs |

## Core Features

*   **Dynamic Metadata Extraction:** Parses embedded `<app-data>` JSON blocks from raw HTML to bypass blocked undocumented APIs and retrieve cryptographically signed page images.
*   **Intelligent Publication Filtering:** Automatically identifies active weekly circulars (e.g., "Uge" flyers) while strictly filtering out irrelevant categories like "Nonfood".
*   **Local AI Vision Parsing:** Leverages local LLMs via multi-message prompting to read heavily compressed, mixed-content images, distinguishing valid food/beverage items from household goods.
*   **Idempotent Database Routing:** Manages data lifecycles using `GroceryDeals_Latest` and `GroceryDeals_Archive` tables, automatically clearing outdated promotions before inserting new batch records.

## Workflow Pipeline

1.  **Store Trigger:** Iterates over a predefined list of target supermarkets.
2.  **Publication Retrieval:** Fetches flyer metadata, verifies active dates, and filters for grocery-specific IDs.
3.  **Image URL Extraction:** Parses the web payload to locate the `pageCount` and extracts the uniquely signed URLs for every individual flyer page.
4.  **AI Vision Processing:** Feeds binary image streams alongside highly explicit extraction prompts into the local vision model to identify specific products and prices.
5.  **Database Insertion:** Flattens the structured JSON output and executes conditional SQL updates/inserts into PostgreSQL.

## Database Management

The pipeline maintains state across two primary tables:
*   `GroceryDeals_Latest`: Holds the active deals for the current week. Queryable by `store` and `publicationID`.
*   `GroceryDeals_Archive`: Stores historical pricing data for long-term tracking.

## Future Roadmap

*   Migration of the PostgreSQL database to a managed cloud provider.
*   Development of a native Android application frontend for mobile deal browsing.
*   Implementation of `pg_trgm` (trigram) search for fuzzy-matching grocery items natively via API.

---
*Maintained by Abhinay*
