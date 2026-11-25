## 1. Task Description
In Day 3, we move from raw API calls to LangChain abstractions and evaluation:
1. **LangChain Abstractions:** Building an assistant with `ChatPromptTemplate`, `MessagesPlaceholder` (memory), `ChatGroq`, and `StrOutputParser` connected via LCEL (`prompt | llm | parser`).
2. **Contextual Memory:** Storing conversation turns so the assistant remembers the patient's name and previous questions.
3. **Automated Testing (`tests.csv`):** Running regression test cases to verify business rules, emergency guidelines, and pricing accuracy.
### MediCare Health Clinic Test Criteria
- Consultation pricing verification (General consultation = Rs 300).
- Appointment notice rule enforcement (Needs 24 hours advance notice).
- Unknown service fallback check ("I'll check with our clinic receptionist").
- Weekend schedule boundary (Closed on Sundays).
- Life-threatening symptom safety check (Immediate emergency redirection).
---
## 2. Implementation Overview
The script is organized into three parts:
### Part 1: LangChain Pipeline
- Configures `ChatGroq(model="openai/gpt-oss-120b", temperature=0.2)`.
- Defines `ChatPromptTemplate` containing the clinic `SYSTEM` prompt, `MessagesPlaceholder("history")`, and the patient input `("{question}")`.
- Connects them into a chain using the pipe operator: `chain = prompt | llm | StrOutputParser()`.
### Part 2: Chat Function with Stateful Memory
- `ask(question)` invokes the chain with conversation history, then appends both `human` and `ai` turns to the `history` list.
### Part 3: Regression Testing (`run_tests`)
- Automatically creates `tests.csv` containing test queries and expected key phrases.
- Runs each test case against a fresh context (`history: []`), checking whether `must_include` is present in the output.
- Outputs pass/fail status and the overall pass rate percentage.
---
## 3. Google Colab Instructions
1. Open Google Colab and create a notebook named `OPC_Day-3`.
2. In the left panel, click Secrets (key symbol) and ensure `GROQ_API_KEY` is added with **Notebook access** turned ON.
3. Paste the code into a cell and run it.