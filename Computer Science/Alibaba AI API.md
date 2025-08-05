#AI #computer_science #LLM #tensorflow #pytorch #data_science #api 

Here's a comprehensive guide to using **Alibaba Cloud's AI APIs** as of mid-2025, focusing on the **Model Studio** and **Platform for AI (PAI)** services.

---

## 🚀 Getting Started

### 1. **Create an Alibaba Cloud Account**

- Sign up at [Alibaba Cloud](https://www.alibabacloud.com/).
    

### 2. **Activate Model Studio**

- Navigate to the [Model Studio Console](https://www.alibabacloud.com/help/en/model-studio/getting-started/activate-model-studio).
    
- Activate the service to obtain a free quota.([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/user-guide/api-key-management?utm_source=chatgpt.com "Alibaba Cloud Model Studio:API key management"), [AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/first-api-call-to-qwen?utm_source=chatgpt.com "Make your first API call to Qwen - Alibaba Cloud Model Studio"))
    

### 3. **Obtain an API Key**

- Go to the [API Key Management](https://www.alibabacloud.com/help/en/model-studio/get-api-key) page.
    
- Click **Create My API Key**.
    
- Click **View** in the Actions column to see your API key.
    
- Store your API key securely; it's required for all API requests.([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/user-guide/api-key-management?utm_source=chatgpt.com "Alibaba Cloud Model Studio:API key management"), [AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/first-api-call-to-qwen?utm_source=chatgpt.com "Make your first API call to Qwen - Alibaba Cloud Model Studio"))
    

### 4. **Set the API Key as an Environment Variable**

- For security, set your API key as an environment variable.
    
- For example, on Linux or macOS:([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/get-api-key?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Obtain an API key"))
    

```bash
  export DASHSCOPE_API_KEY='your_api_key'
```

- On Windows Command Prompt:
    

```cmd
  set DASHSCOPE_API_KEY=your_api_key
```

- This avoids hardcoding the API key in your code.([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/first-api-call-to-qwen?utm_source=chatgpt.com "Make your first API call to Qwen - Alibaba Cloud Model Studio"))
    

---

## 🧠 Model Studio Overview

Model Studio provides access to various AI models, including the **Qwen** series.([AlibabaCloud](https://www.alibabacloud.com/help/doc-detail/2587658.html?utm_source=chatgpt.com "Alibaba Cloud Model Studio:FAQ"))

### **Qwen Models**

- **Qwen-Plus**: General-purpose language model.
    
- **Qwen-VL**: Multimodal model supporting images and videos.
    
- **Qwen-Max**: Enhanced capabilities for complex tasks.([AlibabaCloud](https://www.alibabacloud.com/help/doc-detail/2587658.html?utm_source=chatgpt.com "Alibaba Cloud Model Studio:FAQ"))
    

For a complete list of models and their capabilities, refer to the [Model Studio Documentation](https://www.alibabacloud.com/help/en/model-studio/getting-started/models).([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/use-qwen-by-calling-api?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Qwen API reference"))

---

## 📬 Making API Calls

### **1. Using OpenAI-Compatible API**

Model Studio offers an OpenAI-compatible API endpoint.([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/use-qwen-by-calling-api?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Qwen API reference"))

- **Endpoint**: `https://dashscope-intl.aliyuncs.com/compatible-mode/v1/chat/completions`([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/use-qwen-by-calling-api?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Qwen API reference"))
    

**Python Example**:

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1"
)

response = client.chat.completions.create(
    model="qwen-plus",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is the capital of France?"}
    ]
)

print(response.choices[0].message.content)
```

### **2. Using DashScope SDK**

Alternatively, use the DashScope SDK for more features.([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/use-qwen-by-calling-api?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Qwen API reference"))

**Python Example**:

```python
import os
import dashscope

dashscope.base_http_api_url = 'https://dashscope-intl.aliyuncs.com/api/v1'

response = dashscope.Generation.call(
    api_key=os.getenv('DASHSCOPE_API_KEY'),
    model="qwen-plus",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is the capital of France?"}
    ],
    result_format='message'
)

print(response.output.text)
```

---

## 🧰 Advanced Features

### **1. Function Calling**

Define functions that the model can call with structured arguments.

- Useful for integrating external tools or APIs.
    

### **2. Temporary Authentication Tokens**

For enhanced security, generate temporary tokens valid for 60 seconds.([AlibabaCloud](https://www.alibabacloud.com/help/doc-detail/2882062.html?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Obtain temporary authentication token"))

**Example**:

```bash
curl -X POST https://dashscope-intl.aliyuncs.com/api/v1/tokens \
-H "Authorization: Bearer $DASHSCOPE_API_KEY"
```

This approach reduces the risk of long-term API key exposure.([AlibabaCloud](https://www.alibabacloud.com/help/doc-detail/2882062.html?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Obtain temporary authentication token"))

---

## 📚 Additional Resources

- **Model Studio Documentation**: [https://www.alibabacloud.com/help/en/model-studio](https://www.alibabacloud.com/help/en/model-studio)
    
- **PAI Documentation**: [https://www.alibabacloud.com/help/en/pai](https://www.alibabacloud.com/help/en/pai)
    
- **DashScope SDK**: [https://www.alibabacloud.com/help/en/model-studio/developer-reference/sdk-overview](https://www.alibabacloud.com/help/en/model-studio/developer-reference/sdk-overview)([AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/application-calling-guide?utm_source=chatgpt.com "Alibaba Cloud Model Studio:Application calling"), [AlibabaCloud](https://www.alibabacloud.com/help/en/pai/user-guide/api-eas-2021-07-01-createappservice?utm_source=chatgpt.com "Platform For AI:CreateAppService - Alibaba Cloud"), [AlibabaCloud](https://www.alibabacloud.com/help/en/model-studio/developer-reference/configure-api-key-through-environment-variables?utm_source=chatgpt.com "Set API key as an environment variable in Alibaba Cloud Model Studio"))
    

---

If you need assistance with specific integrations, examples in a particular programming language, or further details on any feature, feel free to ask!