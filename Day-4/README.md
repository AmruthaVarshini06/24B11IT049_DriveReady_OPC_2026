# OPC_Day-4: Tools, Structured Output, and Google Sheets (MediCare Health Clinic)

AI Agents Workshop - Level 2 Single Agent (Day 4)  
File: `OPC_Day-4.ipynb` / `OPC_Day-4.py`  
Environment: Google Colab  

---

## 1. Task Description

In Day 4, the assistant moves from conversation to **autonomous action** using tools and external persistence:
1. **Tool Definition (`@tool`):** Equipping the assistant with functions to query live data (`get_service_fee`, `check_clinic_hours`) and execute side effects (`log_appointment`).
2. **Structured Output Validation (Pydantic):** Ensuring appointment records adhere to strict schemas (`Appointment(BaseModel)`) with `Literal` types, preventing malformed records from reaching storage.
3. **Google Sheets Integration (`gspread`):** Connecting to a real Google Sheet (`MediCare Clinic Appointments`) to persist confirmed patient bookings across sessions.
4. **Agent Orchestration (`create_agent`):** Letting the agent decide when and which tools to call dynamically based on the patient's conversation.

---

## 2. Implementation Overview

### Tool 1: `get_service_fee(service: str) -> str`
- Reads live pricing from `SERVICES` dictionary.
- Handles unknown services with a helpful message listing available options instead of throwing exceptions.

### Tool 2: `check_clinic_hours(day: str) -> str`
- Queries `HOURS` dictionary.
- Accurately reports weekday timings (9 AM – 6 PM) and Sunday closures.

### Tool 3: `log_appointment(name: str, service: str, date: str, is_emergency: bool) -> str`
- Instantiates `Appointment` Pydantic model to validate inputs before touching the sheet.
- Calculates fee and appends a structured row: `[name, service, date, is_emergency, fee]`.
- Returns a human-readable confirmation string for the agent to convey back to the patient.

### Agent Loop & Memory
- Initialized with `create_agent(model=llm, tools=[...], system_prompt=SYSTEM)`.
- Multi-turn conversation updates `history` with complete tool call traces and final responses.
- Protected by `try/except` for API rate limits and connection resilience.

---

## 3. Google Colab Instructions

1. Open Google Colab and create a new notebook named `OPC_Day-4`.
2. Add your `GROQ_API_KEY` to Colab Secrets (the 🔑 icon) and turn on **Notebook access**.
3. Paste the code into a single cell and run it.
4. When prompted by Google Colab, authorize access to your Google Drive/Sheets account so `gspread` can create and update the `MediCare Clinic Appointments` sheet.
