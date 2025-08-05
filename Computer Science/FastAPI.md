
#api #computer_science #containers #deploy #images 

Em FastAPI, os decoradores são usados para vincular funções a rotas HTTP ou a eventos do ciclo de vida da aplicação. A seguir, explico os principais decoradores utilizados e dou exemplos de cada um:

---

### 1. Decoradores de Rota

Esses decoradores associam funções a endpoints que serão acessíveis via requisições HTTP. Eles informam ao FastAPI qual método HTTP (GET, POST, PUT, DELETE, etc.) deve ser usado para acessar aquela função.

### **@app.get**

- **O que faz:**
    
    Define um endpoint que responde a requisições HTTP GET, que geralmente são usadas para leitura ou consulta de dados, sem causar efeitos colaterais (ou seja, sem alterar dados no servidor).
    
- **Exemplo:**
    
    ```python
    from fastapi import FastAPI
    
    app = FastAPI()
    
    @app.get("/items/{item_id}")
    def read_item(item_id: int):
        return {"item_id": item_id, "description": "Detalhes do item"}
    ```
    
    _Neste exemplo, quando um cliente faz uma requisição GET para `/items/42`, a função `read_item` é chamada e retorna um dicionário com informações sobre o item._
    

### **@app.post**

- **O que faz:**
    
    Define um endpoint que responde a requisições HTTP POST. Esse método é normalmente utilizado para criar novos recursos no servidor, enviando dados no corpo da requisição.
    
- **Exemplo:**
    
    ```python
    from fastapi import FastAPI
    from pydantic import BaseModel
    
    app = FastAPI()
    
    class Item(BaseModel):
        name: str
        description: str = None
        price: float
    
    @app.post("/items/")
    def create_item(item: Item):
        return {"message": "Item criado com sucesso", "item": item}
    ```
    
    _Aqui, o endpoint `/items/` recebe dados (no formato JSON) que são validados pelo modelo Pydantic `Item`. A função cria (ou simula a criação) de um novo item e retorna uma mensagem._
    

### **Outros Métodos (PUT, DELETE, etc.)**

- **@app.put:**
    
    Usado para atualizar um recurso existente.
    
- **@app.delete:**
    
    Usado para remover um recurso.
    

_Esses decoradores funcionam de forma semelhante aos exemplos de GET e POST, porém indicam operações de atualização ou exclusão._

---

### 2. Decoradores de Evento

Estes decoradores não definem endpoints HTTP, mas registram funções que serão executadas em momentos específicos do ciclo de vida da aplicação, como no startup (inicialização) ou shutdown (encerramento).

### **@app.on_event**

- **O que faz:**
    
    Permite executar funções automaticamente durante eventos do ciclo de vida da aplicação.
    
- **Exemplos:**
    
    - **Startup:**
        
        Executa uma função assim que a aplicação inicia, ideal para carregar configurações, conexões com bancos de dados ou modelos de machine learning.
        
        ```python
        from fastapi import FastAPI
        
        app = FastAPI()
        
        @app.on_event("startup")
        def startup_event():
            print("Aplicação iniciada. Executando configurações iniciais...")
        ```
        
    - **Shutdown:**
        
        Executa uma função quando a aplicação está prestes a encerrar, útil para liberar recursos ou fechar conexões.
        
        ```python
        @app.on_event("shutdown")
        def shutdown_event():
            print("Aplicação encerrada. Limpando recursos...")
        ```
        

---

### 3. Decoradores para Outras Finalidades

Além dos decoradores de rota e de evento, o FastAPI oferece outros decoradores para funcionalidades específicas:

### **@app.middleware**

- **O que faz:**
    
    Permite definir uma função que intercepta todas as requisições e respostas, possibilitando a execução de lógica antes ou depois do processamento dos endpoints. Útil para logging, autenticação global, entre outros.
    
- **Exemplo:**
    
    ```python
    from fastapi import FastAPI, Request
    
    app = FastAPI()
    
    @app.middleware("http")
    async def log_requests(request: Request, call_next):
        print(f"Nova requisição: {request.method} {request.url}")
        response = await call_next(request)
        return response
    ```
    

### **@app.websocket**

- **O que faz:**
    
    Define endpoints que utilizam o protocolo WebSocket para comunicação bidirecional em tempo real.
    
- **Exemplo:**
    
    ```python
    from fastapi import FastAPI, WebSocket
    
    app = FastAPI()
    
    @app.websocket("/ws")
    async def websocket_endpoint(websocket: WebSocket):
        await websocket.accept()
        while True:
            data = await websocket.receive_text()
            await websocket.send_text(f"Mensagem recebida: {data}")
    ```
    

---

### Resumo das Diferenças

- **Decoradores de rota (@app.get, @app.post, etc.):**
    
    Associam funções a endpoints HTTP específicos, determinando como a aplicação responde a diferentes métodos (GET para leitura, POST para criação, etc.).
    
- **Decoradores de evento (@app.on_event):**
    
    Executam funções automaticamente em eventos do ciclo de vida da aplicação (startup, shutdown).
    
- **Decoradores de middleware (@app.middleware):**
    
    Interceptam requisições e respostas para aplicar lógica global a todas as rotas.
    
- **Decoradores de websocket (@app.websocket):**
    
    Criam endpoints para comunicação via WebSocket, possibilitando interações em tempo real.
    

Cada tipo de decorador tem seu papel específico, permitindo organizar e modularizar a aplicação conforme a funcionalidade desejada.