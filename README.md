# MCP Tool Server

A lightweight **MCP (Model Context Protocol) Tool Server** built with **FastMCP (Python)** that exposes multiple reusable tools (time, weather, internet search, and math utilities) over **STDIO transport**. This server is designed to be dynamically discovered and consumed by MCP-compatible clients such as **LangChain / LangGraph agents** and tested using the **MCP Inspector**.

---

## 🚀 Overview

This project demonstrates how to:

* Build an MCP server using **FastMCP**
* Expose Python functions as **MCP tools** using decorators
* Securely manage API keys using **environment variables**
* Test tools interactively using **MCP Inspector**
* Run the server using **STDIO transport** for agent-based integrations

The server acts as a **tool provider**, while the agent (LLM) dynamically discovers and invokes these tools at runtime.

---

## 🧠 Architecture & Approach

**Our approach follows a clean separation of responsibilities:**

1. **Tool Server (this repo)**

   * Hosts well-defined tools
   * Handles external APIs (Weather, Google Search)
   * Exposes tools via MCP protocol

2. **Client / Agent (external)**

   * Discovers tools dynamically from the MCP server
   * Decides *when* and *how* to call tools using an LLM

```
LLM Agent
   │
   ├── MCP Client
   │      │
   │      └── MCP Server (FastMCP)
   │              ├── Time Tool
   │              ├── Weather Tool
   │              ├── Internet Search Tool
   │              └── Math Tools
```

This design makes the system **modular, extensible, and agent-friendly**.

---

## 🛠️ Tools Implemented

### ⏰ Time Tool

* Get current time for any IANA timezone

### 🌦️ Weather Tool

* Fetch real-time weather using OpenWeatherMap API

### 🌐 Internet Search Tool

* Google Custom Search integration
* Returns formatted search results

### ➗ Math Utilities

* Add, subtract, multiply, divide
* Square root calculation

---

## 📂 Project Structure

```
Mcp-server/
│
├── server.py          # MCP Tool Server
├── .env               # API keys (not committed)
├── README.md          # Documentation
```

---

## ⚙️ Prerequisites

* Python **3.10+**
* Google Custom Search API key
* OpenWeatherMap API key

---

## 📦 Installation

```bash
# Clone the repository
git clone https://github.com/your-username/mcp-tool-server.git
cd mcp-tool-server

# Create virtual environment
python -m venv venv
source venv/bin/activate   # Windows: venv\\Scripts\\activate

# Install dependencies
pip install -r requirements.txt
```

---

## 🔐 Environment Variables

Create a `.env` file in the root directory:

```
WEATHER_API_KEY=your_openweathermap_api_key
GOOGLE_API_KEY=your_google_api_key
GOOGLE_CX=your_search_engine_id
```

---

## ▶️ Running the MCP Server

```bash
python server.py
```

Output:

```
Starting MCP server...
```

The server now listens on **STDIO**, ready to be consumed by MCP clients.

---

## 🧪 Testing with MCP Inspector

1. Open **MCP Inspector**
2. Select **STDIO transport**
3. Point it to `server.py`
4. Start the server
5. Invoke tools interactively

Example test:

```json
{
  "name": "get_current_time",
  "arguments": {
    "timezone": "Asia/Kolkata"
  }
}
```

---

## 🧩 Tool Definition Example

```python
@mcp.tool()
def get_current_time(timezone: str = "UTC") -> str:
    utc_now = datetime.datetime.now(datetime.timezone.utc)
    target_tz = ZoneInfo(timezone)
    return utc_now.astimezone(target_tz).isoformat()
```

The `@mcp.tool()` decorator automatically:

* Registers the function as an MCP tool
* Infers schema from type hints
* Makes it discoverable by agents

---

## 💡 Why STDIO Transport?

* Ideal for **local agents** and subprocess-based execution
* Secure (no open ports)
* Simple and reliable for development

Can be later switched to **HTTP transport** if needed.

---

## ✨ Key Benefits

* 🔌 Plug-and-play tools for agents
* 🧠 LLM-driven tool selection
* 🔒 Secure API key handling
* 🧱 Modular and extensible design

---

## 🚧 Future Enhancements

* HTTP transport support
* Authentication / rate limiting
* More tools (DB access, file ops)
* Docker support

---
