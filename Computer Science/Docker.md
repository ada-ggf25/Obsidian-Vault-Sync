
#computer_science #containers #api #images #backend 

[[Pushing to Docker]]
[[Docker - Remapping a Port]]

In Docker, the terms **Containers**, **Images**, **Volumes**, and **Builds** represent fundamental concepts that serve different purposes in the containerization workflow. Here's a comprehensive breakdown of each:

---

## 🐳 1. Docker **Image**

### 📌 What It Is:

A **Docker Image** is a **read-only template** used to create containers. It includes everything needed to run an application:

- Code or binaries
- Runtime (e.g., Python, Node.js)
- Libraries and dependencies
- File system snapshot
- Environment variables

Think of an image as a _blueprint_ for containers.

### 📦 Characteristics:

- Immutable: Once built, cannot be changed.
- Layered: Built in layers using a `Dockerfile`, where each command (like `RUN`, `COPY`, `ENV`) creates a new image layer.
- Stored in registries (e.g., Docker Hub, GitHub Container Registry, private registries).

### 📋 Example:

```bash

docker pull python:3.11

```

This downloads an image of Python 3.11 from Docker Hub.

---

## 🚢 2. Docker **Container**

### 📌 What It Is:

A **Container** is a **runtime instance** of an image — a running process isolated from the host and other containers.

### 🧠 Analogy:

If an image is the class in programming, a container is the instantiated object.

### ⚙️ Characteristics:

- Mutable: You can write to its file system during runtime (though changes are lost unless persisted).
- Ephemeral: Typically designed to be stopped and replaced, not modified.
- Isolated: Has its own network, filesystem, and process space.

### 📋 Example:

```bash
bash
CopiarEditar
docker run -it python:3.11 bash

```

This creates and runs a container from the `python:3.11` image and gives you an interactive shell.

---

## 🧱 3. Docker **Build**

### 📌 What It Is:

A **Build** is the process of **creating a Docker image** from a `Dockerfile`.

### 🔨 How It Works:

Docker reads the `Dockerfile` line-by-line and executes the instructions to build an image in layers.

### 🧾 Typical Dockerfile:

```
Dockerfile
CopiarEditar
FROM python:3.11
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]

```

### 📋 Build Command:

```bash
bash
CopiarEditar
docker build -t my-python-app .

```

This builds an image named `my-python-app` based on the current directory (`.`).

---

## 🗂 4. Docker **Volume**

### 📌 What It Is:

A **Volume** is a **persistent storage mechanism** used by containers to store data **outside the container’s lifecycle**.

### 💾 Purpose:

- Preserve data across container restarts and rebuilds.
- Share data between containers.
- Prevent loss of data when the container is removed.

### 🔍 Characteristics:

- Managed by Docker.
- Stored in the host filesystem but abstracted from containers.
- Can be declared in `docker-compose.yml` or CLI.

### 📋 Example:

```bash
bash
CopiarEditar
docker volume create mydata
docker run -v mydata:/data myimage

```

This mounts the volume `mydata` into the container at `/data`.

---

## 🔁 Comparison Table

|Feature|Image|Container|Build|Volume|
|---|---|---|---|---|
|Definition|Blueprint for containers|Running instance of an image|Process of creating an image|Persistent data storage|
|Mutable|❌ (Read-only)|✅ (Ephemeral and runtime-writable)|❌ (One-time operation)|✅ (Persists across containers)|
|Lifecycle|Static|Dynamic and temporary|One-time or repeated process|Independent and long-lived|
|Use case|App template|App runtime|Image creation|Store user data / DB / logs|
|Analogy|Class|Object|Compiler step|External hard drive|

---

## 🔚 Summary

- **Image** = Recipe (immutable, built from Dockerfile)
- **Build** = Process of making the recipe into a finished image
- **Container** = The running application (created from an image)
- **Volume** = Persistent storage used by containers to keep data safe and reusable

Would you like a diagram of how these relate in a typical development cycle?

4o

### 🐳 **Comandos Gerais**

- `docker version`
    
    Exibe a versão do Docker (cliente e servidor).
    
- `docker info`
    
    Mostra informações detalhadas sobre a instalação do Docker.
    
- `docker help`
    
    Lista de comandos disponíveis ou ajuda sobre um comando específico.
    

---

### 📦 **Imagens**

- `docker pull <imagem>`
    
    Baixa uma imagem do Docker Hub.
    
- `docker build -t <nome-da-imagem> .`
    
    Cria uma imagem a partir de um `Dockerfile`.
    
- `docker images`
    
    Lista todas as imagens locais.
    
- `docker rmi <imagem>`
    
    Remove uma imagem.
    

---

### 🚢 **Containers**

- `docker run <imagem>`
    
    Roda um container baseado em uma imagem.
    
- `docker run -it <imagem> bash`
    
    Roda um container interativo com bash.
    
- `docker run -d <imagem>`
    
    Roda o container em segundo plano (detached mode).
    
- `docker ps`
    
    Lista containers em execução.
    
- `docker ps -a`
    
    Lista todos os containers (inclusive os parados).
    
- `docker start <id/nome>`
    
    Inicia um container parado.
    
- `docker stop <id/nome>`
    
    Para um container em execução.
    
- `docker restart <id/nome>`
    
    Reinicia um container.
    
- `docker rm <id/nome>`
    
    Remove um container parado.
    
- `docker exec -it <id/nome> bash`
    
    Acessa o shell de um container em execução.
    

---

### 🗂️ **Volumes e Persistência**

- `docker volume create <nome>`
    
    Cria um volume.
    
- `docker volume ls`
    
    Lista volumes.
    
- `docker volume rm <nome>`
    
    Remove um volume.
    
- `docker run -v <volume>:/caminho/dentro/do/container <imagem>`
    
    Usa um volume no container.
    

---

### 🛠️ **Dockerfile e Build**

- `docker build -t <nome-da-imagem> .`
    
    Cria imagem com base no `Dockerfile` no diretório atual.
    
- `docker history <imagem>`
    
    Mostra o histórico de camadas da imagem.
    

---

### 📁 **Redes**

- `docker network ls`
    
    Lista redes.
    
- `docker network create <nome>`
    
    Cria uma nova rede.
    
- `docker run --network=<rede> <imagem>`
    
    Conecta container a uma rede específica.
    

---

### 🧹 **Limpeza e Manutenção**

- `docker system prune`
    
    Remove tudo que não está sendo usado (containers, volumes, imagens, redes).
    
- `docker container prune`
    
    Remove containers parados.
    
- `docker image prune`
    
    Remove imagens não utilizadas.
    