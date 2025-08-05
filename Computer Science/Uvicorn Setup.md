#backend #api #pyhton #computer_science #deploy 

[](https://tse2.mm.bing.net/th?id=OIP.JziQAB_Vvko941EYH1L9wwHaEa&w=282&h=282&c=7)

[](https://tse2.mm.bing.net/th?id=OIP.yahCkR7fO1wRnzJ17bkbbwHaDP&w=207&h=207&c=7)

[](https://tse1.mm.bing.net/th?id=OIP.0HJ3yozGXQF7xW9eqQwz1gHaHa&w=474&h=474&c=7)

[](https://tse2.mm.bing.net/th?id=OIP.eHRT1PkRXgVsKJW-ERlXqgHaFa&w=346&h=346&c=7)

Setting up and using Uvicorn to run a FastAPI application is straightforward and efficient. Here's a detailed guide to help you through the process:

---

## 🛠️ Step 1: Install FastAPI and Uvicorn

Begin by installing FastAPI and Uvicorn using pip. It's recommended to include the `[standard]` extras for Uvicorn to leverage performance optimizations like `uvloop` and `httptools`.[GeeksforGeeks+1deadbearcode+1](https://www.geeksforgeeks.org/install-fastapi-and-run-your-first-fastapi-server-on-windows/?utm_source=chatgpt.com)[Stack Overflow](https://stackoverflow.com/questions/77001129/how-to-configure-fastapi-logging-so-that-it-works-both-with-uvicorn-locally-and?utm_source=chatgpt.com)

```bash
bash
CopiarEditar
pip install "fastapi[all]" "uvicorn[standard]"

```

This command installs FastAPI along with all its optional dependencies, and Uvicorn with its standard performance enhancements.[GitHub](https://github.com/fastapi/fastapi/discussions/6851?utm_source=chatgpt.com)

---

## 📝 Step 2: Create a FastAPI Application

Create a file named `main.py` and define a simple FastAPI application:

```python
python
CopiarEditar
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def read_root():
    return {"message": "Hello, World!"}

```

This sets up a basic API endpoint at the root URL (`/`) that returns a JSON response.

---

## 🚀 Step 3: Run the Application with Uvicorn

To start the FastAPI application using Uvicorn, execute the following command in your terminal:

```bash
bash
CopiarEditar
uvicorn main:app --reload

```

Here's a breakdown of the command:

- `main:app` refers to the `app` instance in the `main.py` file.
- `-reload` enables auto-reloading of the server when code changes are detected. This is useful during development but should be avoided in production environments.

Once the server is running, you can access your API at `http://127.0.0.1:8000/`.

FastAPI also provides interactive API documentation at `http://127.0.0.1:8000/docs` (Swagger UI) and `http://127.0.0.1:8000/redoc` (ReDoc).

---

## ⚙️ Step 4: Customize Uvicorn Settings

Uvicorn offers various command-line options to customize its behavior:

- **Specify Host and Port**: To run the server on a specific host and port:
    
    ```bash
    bash
    CopiarEditar
    uvicorn main:app --host 0.0.0.0 --port 8080
    
    ```
    
- **Set Log Level**: To adjust the verbosity of logs:[Stack Overflow+1CSDN Blog+1](https://stackoverflow.com/questions/77001129/how-to-configure-fastapi-logging-so-that-it-works-both-with-uvicorn-locally-and?utm_source=chatgpt.com)
    
    ```bash
    bash
    CopiarEditar
    uvicorn main:app --log-level debug
    
    ```
    
- **Run with Multiple Workers**: For production, you might want to run multiple worker processes:[fastapi.tiangolo.com](https://fastapi.tiangolo.com/deployment/server-workers/?utm_source=chatgpt.com)
    
    ```bash
    bash
    CopiarEditar
    uvicorn main:app --workers 4
    
    ```
    

For a full list of options, you can run:

```bash
bash
CopiarEditar
uvicorn --help

```

---

## 🧪 Step 5: Programmatic Execution (Optional)

You can also run Uvicorn programmatically within a Python script:

```python
python
CopiarEditar
import uvicorn

if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=8000, reload=True)

```

This approach is useful when integrating the server startup within a larger Python application.

---

## 🛡️ Step 6: Production Deployment

For deploying FastAPI applications in a production environment, it's recommended to use Uvicorn in conjunction with a process manager like Gunicorn. This setup allows for better management of multiple worker processes and improved scalability.

Here's how you can run your application using Gunicorn with Uvicorn workers:

```bash
bash
CopiarEditar
gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app

```

This command starts Gunicorn with 4 worker processes, each running the FastAPI application using Uvicorn.

For more detailed information on deploying FastAPI applications, you can refer to the FastAPI deployment documentation.

---

By following these steps, you can effectively set up and run your FastAPI application using Uvicorn, both in development and production environments.