#computer_science #containers #api #images #backend 

Because you started the container without mapping its internal port to your host, Docker isn’t forwarding any traffic to `localhost`. You have two options:

---

### 1. Re-run with a port mapping

Stop & remove your existing container:

```bash
docker stop jolly_hawking && docker rm jolly_hawking
```

Then run it again, binding container port 8080 to your host’s port 8080:

```bash
docker run -d \
  --name jolly_hawking \
  -p 8080:8080 \
  eco-ai.ly_ci_rp_api:latest
```

Now you can hit your FastAPI at

> **[http://localhost:8080/](http://localhost:8080/)**

(and docs at `/docs`).

---

### 2. Use Docker Desktop’s port-forwarding GUI

1. Open Docker Desktop → **Containers**
    
2. Find your `jolly_hawking` container
    
3. Click the “⋮” → **Edit** (or the “port” icon)
    
4. Add a host-to-container rule:
    
    - Host port: `8080`
        
    - Container port: `8080`
        
5. Save & restart
    

Again, visit **[http://localhost:8080/](http://localhost:8080/)** in your browser.

---

Either way, once the port’s mapped, you’ll be able to access your app locally.