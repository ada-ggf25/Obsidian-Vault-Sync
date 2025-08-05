
#computer_science #deploy #frontend 

[[UV & Streamlit]]
[[Tailwind & Streamlit]]
[[Theme of Streamlit]]

## 1. Understanding the Technologies

### Streamlit

Streamlit is a Python library designed for building data apps quickly. It comes with its own built‑in server (based on Tornado) that automatically launches when you run your app. The normal way to start a Streamlit app is by executing:

bash

Copy

`streamlit run your_app.py`

This command:

- Starts a Tornado-based web server.
    
- Opens your browser to view the app (by default on port 8501).
    
- Takes care of reloading your app when changes are detected.
    

### Uvicorn

Uvicorn is an ASGI server commonly used for running asynchronous Python web frameworks (such as FastAPI or Starlette). It is built on top of uvloop and is designed for high-performance asynchronous operations.