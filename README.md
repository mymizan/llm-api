# LLM API with Ollama and Docker 

This repository provides a Docker-based setup to run [Ollama](https://ollama.com/). It automatically loads a model on container startup, and persists model files across restarts.

---

## 🐳 Getting Started

### Start the Container

```bash
docker compose up -d
```

This will:
- Start the REST API on port `4567`
- Automatically download and run `llama3:2`
- Persist model data in a volume

---

## 🧠 Switching Models

To load a different model:

1. Find the model name from [https://ollama.com/search](https://ollama.com/search).
   Examples: `llama2`, `mistral`, `gemma`, `codellama:7b`.

2. Edit the `docker-compose.yml` file:

```yaml
entrypoint: >
  /bin/bash -c "ollama serve & sleep 3 && ollama run mistral"
```

3. Restart the container:

```bash
docker compose down
docker compose up -d
```

The new model will be pulled and cached automatically.

---

## 🔀 Changing the API Port

By default, the Ollama API is mapped to port **4567**.

To change it, edit the `ports` section in `docker-compose.yml`:

```yaml
ports:
  - "12345:11434"  # External:Internal
```

Then restart Docker:

```bash
docker compose down
docker compose up -d
```

---

## ✅ Testing the API

After starting the container, use `curl` or any HTTP client (like Postman):

```bash
curl http://localhost:4567/api/generate -d '{
  "model": "llama3:2",
  "prompt": "What is the capital of Japan?"
}'
```

Expected response:

```json
{
  "response": "The capital of Japan is Tokyo.",
  ...
}
```

---

## 📁 Model Persistence

Downloaded models are stored in a Docker volume named `ollama_data`, so they won’t be redownloaded every time the container restarts.

---

## 🧰 Useful Links

- [Ollama Official Site](https://ollama.com/)
- [Ollama CLI Docs](https://ollama.com/library)
- [Ollama Docker Hub](https://hub.docker.com/r/ollama/ollama)

---

## 🛠 Maintainer

Built by [Your Name](https://github.com/your-username) — feel free to fork and customize!
