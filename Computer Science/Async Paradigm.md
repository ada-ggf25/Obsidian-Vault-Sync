#computer_science 

[[Concurrency and async  await - FastAPI]]

### What is the async paradigm?

The **asynchronous (async) programming paradigm** is a way of structuring code so that tasks that take time (like reading a file, querying a database, or making an API call) don't block the rest of the program from running. Instead of waiting for a task to finish before moving on, the program continues executing and comes back to the task once it's complete.

---

### Synchronous vs. Asynchronous

- **Synchronous programming**:
    
    Tasks are executed **one after another**. The program waits for each task to finish before moving on. This can lead to delays, especially when a task takes a long time (e.g., waiting for a network request).
    
- **Asynchronous programming**:
    
    Tasks are executed **independently**, and the program can **keep running** while waiting for a task to finish. Once the task is done, a callback, promise, or similar mechanism is used to handle the result.
    

---

### Benefits of async programming

- ✅ **Non-blocking**: Keeps your app responsive.
- ⚡ **Faster performance** in I/O-heavy applications.
- 📱 **Better user experience** in mobile/web apps (no freezing UI).
- 🧠 **Efficient use of resources** in multitasking environments.

---

### Common features used in async programming

- **Callbacks**: Functions called when a task completes.
- **Promises/Futures**: Objects representing eventual completion or failure.
- **`async`/`await`**: Modern syntax in languages like JavaScript and Python to write async code that looks like sync code (cleaner and easier to follow).

---

### Example in JavaScript

```jsx
javascript
CopyEdit
async function fetchData() {
  const response = await fetch('<https://api.example.com/data>');
  const data = await response.json();
  console.log(data);
}

console.log('Before fetch');
fetchData();
console.log('After fetch');
// The program continues while waiting for data

```

---

Let me know if you'd like examples in another language like Python or want to see how async applies in a specific context like web servers or trading systems!

Certainly! Here's a simple example of asynchronous programming in Python using the `asyncio` library:

```python

import asyncio

async def greet():
    print("Hello")
    await asyncio.sleep(1)  # Simulate a delay
    print("World!")

asyncio.run(greet())

```

### How It Works

- **`async def greet()`**: Defines an asynchronous function (coroutine).
- **`await asyncio.sleep(1)`**: Pauses the coroutine for 1 second without blocking the entire program.
- **`asyncio.run(greet())`**: Runs the coroutine, managing the event loop for you.[Medium](https://medium.com/%40piyushkashyap045/async-programming-in-python-unlocking-efficiency-in-your-code-0e428061988a?utm_source=chatgpt.com)

This approach is beneficial for I/O-bound tasks, such as web requests, file operations, or database queries, where waiting for a response can be done without halting the entire program.


Below is a more complex example of an asynchronous workflow in Python. In this example, we'll use the `asyncio` library together with the `aiohttp` package to fetch data concurrently from multiple URLs, process the fetched data, and handle potential errors gracefully.

First, make sure you have installed the `aiohttp` package:

```bash

pip install aiohttp

```

Now, consider the following code:

```python

import asyncio
import aiohttp

async def fetch(url, session):
    """
    Fetch the content from a given URL using the provided session.
    This function handles HTTP errors and returns the text content.
    """
    try:
        async with session.get(url) as response:
            response.raise_for_status()  # Raise exception for HTTP errors
            text = await response.text()
            print(f"Successfully fetched {url}")
            return text
    except Exception as e:
        print(f"Error fetching {url}: {e}")
        return None

async def fetch_all(urls):
    """
    Fetch data from all URLs concurrently.
    """
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(url, session) for url in urls]
        # asyncio.gather concurrently runs the fetch tasks and waits for all to finish
        responses = await asyncio.gather(*tasks)
    return responses

async def process_data(data_list):
    """
    Process the list of data by simulating a transformation, such as extracting a summary.
    """
    processed_results = []
    for data in data_list:
        if data:
            # Simulate a processing delay
            await asyncio.sleep(1)
            # As a simple transformation, we could take the first 100 characters
            summary = data[:100]
            processed_results.append(summary)
        else:
            processed_results.append("No data to process.")
    return processed_results

async def main():
    # List of URLs to fetch data from
    urls = [
        "<https://www.example.com>",
        "<https://httpbin.org/get>",
        "<https://jsonplaceholder.typicode.com/todos/1>"
    ]

    print("Starting concurrent fetching of URLs...")
    responses = await fetch_all(urls)

    print("\\nProcessing fetched data...")
    processed = await process_data(responses)

    print("\\nResults:")
    for idx, result in enumerate(processed):
        print(f"\\nResult from URL {urls[idx]}:")
        print(result)
        print("-" * 80)

if __name__ == "__main__":
    asyncio.run(main())

```

### How This Example Works

1. **Fetching Data Concurrently**:
    - The `fetch` function makes an HTTP GET request to a URL using an `aiohttp.ClientSession`.
    - It uses `await` to handle the asynchronous nature of the HTTP request, ensuring that the program doesn’t block while waiting for the response.
    - The function also uses error handling to capture and report any issues (for instance, network errors or non-success HTTP statuses).
2. **Running Multiple Tasks**:
    - The `fetch_all` function creates a list of tasks (one for each URL) and uses `asyncio.gather` to run them concurrently.
    - This approach dramatically improves efficiency compared to fetching each URL sequentially.
3. **Processing Data**:
    - After fetching the responses, the `process_data` function simulates additional processing (e.g., extracting a summary) by sleeping asynchronously for a short period, then cutting the response to the first 100 characters.
    - If any fetch operation failed (resulting in `None`), it handles this by outputting a default message.
4. **Coordinating the Workflow**:
    - The `main` function orchestrates the workflow by first fetching all URLs concurrently, then processing the results, and finally outputting the processed data.
    - The entire asynchronous process is kicked off with `asyncio.run(main())`.

This example demonstrates a robust asynchronous workflow in Python, ideal for I/O-bound tasks such as multiple HTTP requests, where concurrency can lead to significant performance gains.


``` python


print("hello")


```