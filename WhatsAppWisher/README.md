# WhatsappWishAgent (`WhatsappWisher`)

An automated n8n workflow project designed to run daily, read contact lists with mixed or standardized date formats, and send personalized birthday and anniversary greetings via WhatsApp using a local LLM (Ollama).

## Features
* **Automated Daily Scheduling:** Runs daily to check upcoming events against today's date.
* **Flexible Date Formats:** Seamlessly processes dates in `DD-MMM` or full date strings.
* **Local AI Personalization:** Integrates with local open-source models (via Ollama) to craft casual, natural, and emoji-rich greeting messages tailored to specific relationships and nicknames without incurring cloud API rate limits or costs.
* **WhatsApp Integration:** Automatically formats international country codes and phone numbers to dispatch messages seamlessly.

## Project Structure
```text
WhatsappWishAgent/
├── contacts.json         # Contact dataset (Name, Code, PhoneNumber, Relation, NickName, Birthday, Anniversary, Group)
├── workflow.json         # n8n workflow definition
└── README.md             # Project documentation
```

## Sample Data Structure
```json
[
  {
    "Name": "Sadhana",
    "Code": "+91",
    "PhoneNumber": "95737123456",
    "Relation": "Spouse",
    "NickName": "Sadhana",
    "Birthday": "25-Jun",
    "Anniversary": "02-Dec",
    "Group": "Family"
  },
  {
    "Name": "Atharva",
    "Code": "+45",
    "PhoneNumber": "91721234",
    "Relation": "Son",
    "NickName": "Kannaya",
    "Birthday": "05-Sep",
    "Anniversary": "NA",
    "Group": "Family"
  }
]
```

## Setup & Configuration
1. **Import Workflow:** Import the n8n workflow JSON into your local n8n instance.
2. **Configure Ollama Node:** Ensure your local Ollama container is running and accessible (e.g., via `http://host.docker.internal:11434`), and select your preferred model (`qwen2.5:3b`, `llama3.1`, etc.).
3. **Set Up Loop & WhatsApp Nodes:** Configure a Loop node with a batch size of 1 to prevent hardware bottlenecks, and link your WhatsApp API integration (such as WAHA) using dynamic field mapping for country codes and phone numbers.
