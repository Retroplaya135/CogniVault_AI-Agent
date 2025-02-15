# CogniVault_AI-Agent

CogniVault AI Agent Prototype is a conceptual demonstration of an AI agent that uses a SQL database (via SQLAlchemy) as its “brain” for storing tasks, memories, and logs. The agent processes tasks by “consulting” a simulated language model and persists outcomes for future reference. In a production system, you might replace the dummy language model with a cutting-edge LLM (or a suite of models) and expand the database schema and orchestration logic.

---

## Disclaimer

This is a conceptual prototype meant to illustrate one way to “ground” an AI agent in persistent storage. In production you will likely want to add robust error handling, security (for database access and API keys), comprehensive testing, and integration with real language model APIs or modules.

---

## Features

- **Persistent Storage:**  
  Uses SQLAlchemy to persist tasks, memories, and logs in a SQL database.

- **REST API:**  
  A Flask-based API to create new tasks and retrieve task, memory, and log data.

- **Background Task Processing:**  
  A background worker continuously polls for pending tasks, processes them using a dummy language model, updates task statuses, and stores results.

- **Robust Logging:**  
  Logs agent operations to both the console and a rotating log file (`ai_agent.log`).

- **Configurable Settings:**  
  Easily configure database URL, polling interval, server host/port, and debug mode via environment variables.

---

## Use Cases & Examples

### 1. Automated Task Processing
**Example:**  
A team uses CogniVault to automatically analyze incoming business tasks. Each task (e.g., “Analyze user engagement trends for Q4 and suggest improvements”) is processed, and the generated analysis is stored in the agent's memory. This enables the team to review past analyses and track the evolution of business insights over time.

### 2. Persistent AI Memory
**Example:**  
An AI assistant integrated with CogniVault can reference previous tasks and analyses stored in its “memory” to provide context-aware responses. For instance, if asked about previous user engagement metrics, the assistant can retrieve logged insights and provide a detailed history.

### 3. Logging & Auditing for Decision Support
**Example:**  
Organizations can use CogniVault to maintain an audit trail of all automated decisions and analyses. This is useful for compliance, troubleshooting, or further refining the AI agent’s performance by analyzing trends in the logs.

---

## Configuration

The following environment variables can be set to customize the behavior of the agent:

- **`DATABASE_URL`**: The SQLAlchemy database URL.  
  _Default_: `sqlite:///ai_agent.db`

- **`POLL_INTERVAL`**: Seconds between each background polling.  
  _Default_: `5`

- **`HOST`**: Flask server host.  
  _Default_: `127.0.0.1`

- **`PORT`**: Flask server port.  
  _Default_: `5000`

- **`DEBUG_MODE`**: Flask debug mode.  
  _Default_: `False`

---

---

## Requirements

- Python 3.8 or higher
- [SQLAlchemy](https://www.sqlalchemy.org/)

---

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Retroplaya135/CogniVault_AI-Agent.git
   cd CogniVault_AI-Agent

2. Create and Activate a Virtual Environment (Optional but Recommended):

```
python -m venv venv
```
```
source venv/bin/activate   # On Windows: venv\Scripts\activate
```

3. Install Required Packages:
   ```
   pip install flask sqlalchemy

   ```

Usage

Running the Agent
To start the AI agent (which launches both the REST API server and the background worker), run:

```
python ai_agent_full.py
```


The agent will:

>>> Create (or use) the specified database (by default, ai_agent.db).
>>> Start a background worker thread that polls for pending tasks every 5 seconds.
>>> Launch the Flask REST API server on the configured host and port.
>>> Interacting with the REST API
>>> You can use tools like curl, Postman, or your browser to interact with the API.


Create a New Task

Submit a new task using a POST request:

```
curl -X POST http://127.0.0.1:5000/tasks \
  -H "Content-Type: application/json" \
  -d '{"description": "Analyze market trends for Q4."}'
```

Response Example:

```
{
  "message": "Task created.",
  "task_id": 1
}
```

List All Tasks

Retrieve all tasks with:

```
curl http://127.0.0.1:5000/tasks
```


Response Example:

```
[
  {
    "id": 1,
    "description": "Analyze market trends for Q4.",
    "status": "completed",
    "result": "Analyzed data: Found significant trends in user engagement.",
    "created_at": "2025-02-09T10:00:00",
    "updated_at": "2025-02-09T10:00:05"
  }
]
```

Get Details of a Specific Task

Retrieve a task by its ID:

```
curl http://127.0.0.1:5000/tasks/1
```

List All Memories

View all stored memories:
```
curl http://127.0.0.1:5000/memories
```

List All Logs

View all log entries:

```
curl http://127.0.0.1:5000/logs
```


Code Overview

The project is structured as a single Python script (ai_agent_full.py) that includes:

--->Configuration & Logging:
Sets up environment-based configuration and robust logging (both console and rotating file).
--->Database Models:
SQLAlchemy ORM models for Task, Memory, and LogEntry.
--->Dummy Language Model:
A simulated generate_response function that imitates processing delays and returns random responses. Replace this with a call to a real LLM.
--->AI Agent Class:
The core class that continuously polls for pending tasks, processes them, updates statuses, stores results in the database, and logs the outcomes.

