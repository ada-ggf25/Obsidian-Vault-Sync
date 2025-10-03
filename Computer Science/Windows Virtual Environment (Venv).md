
#computer_science #virtual_environment


To test and run your Streamlit project, follow these steps:

1. **Set Up a Virtual Environment**
    
    Create and activate a virtual environment to isolate your project dependencies:
    
    ```bash
    
    python -m venv venv
    # Activate the virtual environment:
    # On macOS/Linux:
    source venv/bin/activate
    # On Windows:
    venv\\Scripts\\activate
    
    ```
    

Great! You've successfully created your virtual environment. Now, you'll need to activate it. Since you're using PowerShell on Windows, you can activate your virtual environment with the following command:

```powershell

.\\venv\\Scripts\\Activate.ps1

```

If you encounter an error regarding script execution policies (for example, a message saying that running scripts is disabled), you can temporarily allow script execution by running:

```powershell

Set-ExecutionPolicy RemoteSigned -Scope Process

```

After that, run the activation command again. Once activated, you'll see your prompt change to indicate that you're working inside the virtual environment (usually it prepends `(venv)` to your prompt).

Next steps:

1. **Install the dependencies:**
    
    ```bash
    
    pip install -r requirements.txt
    
    ```