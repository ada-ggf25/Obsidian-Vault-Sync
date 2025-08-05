
#computer_science #ide

Happy coding with uv and Streamlit!>)](<Below is a detailed walkthrough on how to run a Streamlit app using uv by Astral. In this context, uv is a fast, Rust‑based Python package manager and project manager that can replace pip and virtualenv/venv. By leveraging uv, you can more quickly set up an isolated environment, manage dependencies (including Streamlit), and run your app without the extra hassle of manually activating your virtual environment.

Below are the key steps:

---

## 1. Install uv

You install uv using the installer script provided by Astral. Open your terminal and run:

```bash
bash
Copy
curl -LsSf https://astral.sh/uv/install.sh | sh

```

This downloads the uv binary and installs it to a location that you can add to your PATH (if it isn’t already). Verify your installation by running:

```bash
bash
Copy
uv --version

```

You should see something like:

`uv 0.6.x (commit hash date)`

---

## 2. Initialize a New Project

Create a new directory for your Streamlit app and initialize the project with uv. For example:

```bash
bash
Copy
mkdir my-streamlit-app
cd my-streamlit-app
uv init --app

```

*Notes:*

- The `-app` flag tells uv you are building an application (as opposed to a library), which can affect the auto‑generated file layout and default scripts.
- uv will generate a basic project structure, including a `pyproject.toml` file where your dependencies are declared and a hidden virtual environment (by default in a `.venv` folder).

---

## 3. Add Streamlit as a Dependency

Use uv to add Streamlit to your project’s dependencies. This updates your `pyproject.toml` (and creates or updates the lock file `uv.lock`):

```bash
bash
Copy
uv add streamlit

```

This command automatically selects an appropriate version and installs Streamlit (along with any additional dependencies) into the project's virtual environment.

---

## 4. Create Your Streamlit App Script

Create an application script (for example, `app.py`) in your project’s root directory:

```python
python
Copy
# app.py
import streamlit as st

st.title("My Streamlit App")
st.write("Hello from Streamlit running with uv!")

```

Make sure the file is saved in the same directory where your `pyproject.toml` and `uv.lock` reside.

---

## 5. Run Your Streamlit App with uv

The main advantage of uv is that it automatically handles your virtual environment. Rather than manually activating it, you can run your command within that environment using the `uv run` command.

To run your Streamlit app, simply execute:

```bash
bash
Copy
uv run streamlit run app.py

```

*What happens here?*

- **uv run**: This command ensures that the specified command (in this case, `streamlit run app.py`) is executed inside the virtual environment that uv manages for your project.
- **streamlit run app.py**: This is the standard command to start a Streamlit app.

If everything is configured properly, uv will start the Streamlit server, and you’ll see output indicating that the app is running (typically on [http://localhost:8501](http://localhost:8501/)).

---

## 6. Further Tips and Considerations

- **Hot Reload and Workspaces:**
    
    uv supports fast dependency resolution and environment management. If you set up a multi‑package workspace, note that you may need to configure your PYTHONPATH or work with uv’s workspace features so that changes in your locally developed modules (outside the app directory) are detected by Streamlit’s hot reload system. (See [Using streamlit watch in uv workspaces](https://github.com/astral-sh/uv/issues/12668) for community discussions on this topic.)
    
- **Deployment on Streamlit Community Cloud:**
    
    Since Streamlit Cloud is increasingly using uv under the hood, using uv locally can simplify your deployment workflow. Make sure your `pyproject.toml` (and `uv.lock`) is kept up-to-date. Some users have shared template repositories (e.g., [data-sloth/uv-streamlit-setup](https://github.com/data-sloth/uv-streamlit-setup)) that serve as a good starting point.
    
- **Virtual Environment Management:**
    
    One of the benefits of uv is that it automatically creates and uses a virtual environment for you. This means you don’t have to remember to manually activate it—you simply use `uv run` to execute any command within the controlled environment.
    

---

## Summary

1. **Install uv:**
    
    ```bash
    bash
    Copy
    curl -LsSf https://astral.sh/uv/install.sh | sh
    
    ```
    
2. **Initialize your project:**
    
    ```bash
    bash
    Copy
    mkdir my-streamlit-app
    cd my-streamlit-app
    uv init --app
    
    ```
    
3. **Add Streamlit:**
    
    ```bash
    bash
    Copy
    uv add streamlit
    
    ```
    
4. **Create your app (app.py):**
    
    ```python
    python
    Copy
    import streamlit as st
    
    st.title("My Streamlit App")
    st.write("Hello from Streamlit running with uv!")
    
    ```
    
5. **Run your app:**
    
    ```bash
    bash
    Copy
    uv run streamlit run app.py
    
    ```
    

This process helps you enjoy faster dependency resolution and a more streamlined development workflow compared to traditional methods using pip and manual virtual environment management.

---

### References

[blog.streamlit.io](https://blog.streamlit.io/python-pip-vs-astral-uv/) – Blog post on “pip vs. uv: How Streamlit Cloud sped up app load times by 55%”.

[github.com](https://github.com/data-sloth/uv-streamlit-setup/) – GitHub repository “data-sloth/uv-streamlit-setup” template repository.>)

