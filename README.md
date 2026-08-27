# SimpleAPI — FastAPI Server & Client Demo

A minimal, hands-on example of building and consuming a **REST API** with **FastAPI**. The project simulates a small delivery workflow — shipping goods from a warehouse to a client — using a backend API server and a separate frontend client app.

## Overview

This project has three main parts:

| File                       | Role                                                                                  |
| -------------------------- | -------------------------------------------------------------------------------------- |
| `msnz_server.py`           | A basic FastAPI server that manages a warehouse inventory (dictionary-based)          |
| `msnz_advanced_server.py`  | The same server, refactored with **Pydantic** models for proper data validation       |
| `kgl_client.py`            | A FastAPI client app with an HTML form (Jinja2 templates) that calls the server's API |

The server exposes a `/warehouse/{product}` endpoint. Given a product name and an order quantity, it checks stock, deducts the order from inventory, and returns a shipping confirmation — or an error if there isn't enough stock.

The client provides a simple web form where a user picks a product and quantity, sends the request to the server, and sees the result rendered as an HTML page.

## Project Structure

```
SimpleAPI/
├── msnz_server.py            # Basic API server
├── msnz_advanced_server.py   # API server with Pydantic validation
├── kgl_client.py             # Client app (form + result pages)
├── requirements.txt          # Python dependencies
├── static/
│   └── style.css             # Client page styling
├── templates/
│   ├── index.html            # Order form
│   └── result.html           # Order confirmation / error page
```

## Requirements

- Python 3.10 or newer
- pip (comes with Python)

## Installation (Windows / PowerShell)

1. **Open PowerShell** and navigate to the project folder:

   ```powershell
   cd path\to\SimpleAPI
   ```

2. **Create a virtual environment:**

   ```powershell
   python -m venv venv
   ```

3. **Activate the virtual environment:**

   ```powershell
   .\venv\Scripts\Activate.ps1
   ```

   > If PowerShell blocks this with an execution-policy error, run this once, then try again:
   >
   > ```powershell
   > Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
   > ```

4. **Install the dependencies:**
   ```powershell
   pip install -r requirements.txt
   ```

## Running the Project

You'll need **two PowerShell windows** open at the same time — one for the server, one for the client.

**Terminal 1 — start the API server:**

```powershell
uvicorn msnz_server:app --reload
```

The server runs at `http://127.0.0.1:8000`.

You can test it directly in your browser:

- Interactive API docs: `http://127.0.0.1:8000/docs`
- Direct endpoint call: `http://127.0.0.1:8000/warehouse/tomatoes?order_qty=30`

**Terminal 2 — start the client app:**

```powershell
uvicorn kgl_client:app --reload --port 8001
```

The client runs at `http://127.0.0.1:8001`.

Open that address in your browser, choose a product and quantity from the form, and click **Send** to see the order result.

> **Tip:** Once you understand the basic server, look at `msnz_advanced_server.py` to see the same logic rebuilt with Pydantic models — a more robust, production-style approach to data validation.

## Example Response

Requesting `30` units of tomatoes returns:

```json
{
  "product": "tomatoes",
  "order_qty": "30",
  "units": "boxes",
  "remaining_qty": 970
}
```

If you request more units than are in stock, the API responds with a `400` error and a clear message instead of crashing.

## Available Products

| Product  | Unit    | Starting Quantity |
| -------- | ------- | ----------------- |
| tomatoes | boxes   | 1000               |
| wine     | bottles | 500                |

## Dependencies

- **FastAPI** — web framework for building the API
- **Uvicorn** — ASGI server used to run both apps
- **Requests** — used by the client to call the server
- **Jinja2** — HTML templating for the client's pages
- **python-multipart** — required for parsing HTML form submissions
- **Pydantic** — used in `msnz_advanced_server.py` for data validation (installed automatically with FastAPI)