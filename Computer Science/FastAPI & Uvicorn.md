#backend #api #computer_science #deploy 

why do I should use uvicorn when I creat a pyhton API suing fast api?

### Dito pelo ChatGPT:

[](https://tse3.mm.bing.net/th?id=OIP.Dcz9f_JiutSJty_95S6zngAAAA&w=387&h=387&c=7)

[](https://tse3.mm.bing.net/th?id=OIP.IH1TuOpgS6SQmHXRRjDixQHaC3&w=183&h=183&c=7)

[](https://tse4.mm.bing.net/th?id=OIP.U7pVvDtvEUnh42fhv6fDpQHaEN&w=269&h=269&c=7)

[](https://tse1.mm.bing.net/th?id=OIP.0HJ3yozGXQF7xW9eqQwz1gHaHa&w=474&h=474&c=7)

When building a Python API with FastAPI, using **Uvicorn** is essential because FastAPI is an ASGI (Asynchronous Server Gateway Interface) framework, and it requires an ASGI-compatible server to run. Uvicorn is a high-performance ASGI server that serves this purpose effectively.

---

### 🔍 Why Use Uvicorn with FastAPI?

1. **ASGI Compatibility**: FastAPI is built on the ASGI specification, which supports asynchronous programming. Uvicorn is an ASGI server that can handle both HTTP and WebSocket protocols, making it suitable for running FastAPI applications.
2. **High Performance**: Uvicorn is designed for speed. It uses `uvloop` and `httptools` under the hood, which are optimized for handling asynchronous I/O operations, resulting in faster request handling and lower latency.
3. **Development Convenience**: During development, Uvicorn's `-reload` option allows the server to automatically restart upon code changes, enhancing the development workflow. [GeeksforGeeks+1GeeksforGeeks+1](https://www.geeksforgeeks.org/fastapi-uvicorn/?utm_source=chatgpt.com)
4. **Production Readiness**: While Uvicorn can be used in production, it's common to pair it with a process manager like Gunicorn using Uvicorn workers. This setup allows for better scalability and management of multiple worker processes.

---

### 🚀 Getting Started with Uvicorn and FastAPI

1. **Install Uvicorn**:
    
    ```bash
    bash
    CopiarEditar
    pip install uvicorn
    
    ```
    
2. **Create a FastAPI Application**:
    
    ```python
    python
    CopiarEditar
    # main.py
    from fastapi import FastAPI
    
    app = FastAPI()
    
    @app.get("/")
    async def read_root():
        return {"message": "Hello, World!"}
    
    ```
    
3. **Run the Application with Uvicorn**:
    
    ```bash
    bash
    CopiarEditar
    uvicorn main:app --reload
    
    ```
    

This command tells Uvicorn to run the `app` instance from the `main.py` file and enables auto-reloading for development.

---

In summary, Uvicorn is the recommended server for running FastAPI applications due to its compatibility with ASGI, high performance, and ease of use in both development and production environments.

For a visual explanation, you might find this video helpful:

Fontes

[](https://www.google.com/s2/favicons?domain=https://www.geeksforgeeks.org&sz=32)

[](https://www.google.com/s2/favicons?domain=https://fastapi.tiangolo.com&sz=32)

[](https://www.google.com/s2/favicons?domain=https://leapcell.medium.com&sz=32)

[](https://www.google.com/s2/favicons?domain=https://sentry.io&sz=32)