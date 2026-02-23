# 🐳 Docker

## Docker Instructions Explained

### `FROM` 
```dockerfile
FROM python:3.12-slim
```
Defines the base image.  
We start from a lightweight Python 3.12 environment.

---

### `WORKDIR`
```dockerfile
WORKDIR /app
```
Creates and sets `/app` as the working directory inside the container.  
All following instructions execute from this directory.

---

### `RUN` 
```dockerfile
RUN pip install uv
```
Installs `uv` inside the container.

```dockerfile
RUN uv sync --frozen
```
Installs dependencies exactly as defined in `uv.lock`.  
`--frozen` ensures locked versions are used for reproducible builds.

---

### `COPY`
```dockerfile
COPY pyproject.toml uv.lock ./
```
Copies dependency files first to optimize Docker layer caching.

```dockerfile
COPY . .
```
Copies the rest of the project into `/app`.

---

### `EXPOSE` 
```dockerfile
EXPOSE 8000
```
Documents that the application runs on port `8000`.  
(Note: This does not publish the port automatically.)

---

### `CMD`
```dockerfile
CMD ["uv", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Starts the FastAPI application when the container runs.

- `uv run` → runs inside the project environment  
- `uvicorn app.main:app` → loads `app` from `app/main.py`  
- `--host 0.0.0.0` → allows access from outside the container  
- `--port 8000` → binds the application to port 8000  

---

## Build the Image

```bash
docker build -t demo-9 .
```

Builds the Docker image and tags it as `demo-9`.

---

## Run the Container

```bash
docker run -p 8000:8000 demo-9
```

Then open:

```
http://localhost:8000
```

---

Simple. Reproducible. Consistent.
