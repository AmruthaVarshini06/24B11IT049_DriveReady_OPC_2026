# OPC_Day-2: AI Customer Support Assistant (MediCare Health Clinic)

AI Agents Workshop - Level 1 Fundamentals (Day 2)  
File: `OPC_Day-2.ipynb` / `OPC_Day-2.py`  
Environment: Google Colab  

---

## 1. Task Description

The objective of Day 2 is to build a stateful AI Customer Support Assistant for MediCare Health Clinic using Groq and Python.

The assistant covers:
- Answering patient questions using clinic facts (consultations, checkups, doctor appointments, clinic hours).
- Enforcing medical safety boundaries (no medical diagnosis, no drug prescriptions, emergency redirection).
- Classifying patient message intent using few-shot in-context learning.
- Parsing unstructured patient booking requests into structured JSON data.
- Maintaining multi-turn conversation memory with sliding-window history trimming.
- Handling network and rate-limit errors safely using try/except blocks.

---

## 2. Implementation Overview

The script is organized into the following components:

### Environment and Setup
- Installs `langchain_groq`, `groq`, and `openai`.
- Loads the Groq API key from Google Colab Secrets (`GROQ_API_KEY` or `GROK_API_KEY`) with fallback handling.
- Initializes the OpenAI client pointed to Groq (`https://api.groq.com/openai/v1`) and LangChain's `ChatGroq`.

### Service Catalog and Helper Function
- Stores clinic services and prices in a Python dictionary (`SERVICES_CATALOG`).
- Defines `calculate_service_fee(service_name, count)` with type hints and a docstring.
- Uses a `@log_call` decorator to log function calls and outputs.

### System Prompt
- Follows the Role, Facts, Rules, and Format structure.
- Grounds answers in the clinic catalog, working hours (Monday to Saturday, 9 AM to 6 PM), and safety guidelines.

### Intent Classification (Few-Shot)
- `classify_patient_message(message)` uses 3 example pairs (appointment, pricing, emergency) to classify incoming patient inquiries into a single category.

### Structured Booking Extraction (JSON)
- `extract_booking_json(user_text)` instructs the model to extract `patient_name`, `service_requested`, `preferred_date`, and `is_emergency` into a valid JSON object.

### Stateful Chat Loop with Memory
- `chat(user_text)` appends user questions and assistant replies to a `history` list.
- Implements sliding-window trimming (`MAX_TURNS = 6`) so recent context is retained without exceeding token limits.
- Wraps API calls in `try/except` to catch `RateLimitError` and `APIConnectionError` with user-friendly messages.

---

## 3. Google Colab Instructions

1. Open Google Colab and create a new notebook named `OPC_Day-2`.
2. In the left panel, click the Secrets icon (key symbol).
3. Add a new secret:
   - Name: `GROQ_API_KEY`
   - Value: Your Groq API key
   - Enable **Notebook access**.
4. Paste the code into a cell and run it.