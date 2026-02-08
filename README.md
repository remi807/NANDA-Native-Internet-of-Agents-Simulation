# OneClick Agents (Hackathon Quickstart)

Minimal instructions to get the registry, supplier, logistics, buyer agent, and UIs running with an LLM in under 5 minutes.

## Prereqs
- Python 3.10+
- `pip install -r requirements.txt`
- Optional (recommended for real AI planning): [Ollama](https://ollama.com) running locally with a small model, e.g. `ollama pull llama3.2:latest` and `ollama serve`.

## Run all services
```bash
# from oneclick-agents/
./run.sh
```
This starts:
- Registry :8000
- Supplier :8001
- Logistics :8002
- LLM Buyer :8005

## Run the UIs
Open two terminals:
```bash
streamlit run ui/buyer_ui.py
streamlit run ui/suppliers_ui.py
```

## LLM setup (recommended)
- By default `run.sh` exports:
  - `LLM_BASE_URL=${LLM_BASE_URL:-http://127.0.0.1:11434/v1}`
  - `LLM_MODEL=${LLM_MODEL:-llama3.2:latest}`
- If Ollama is running with that model, the buyer agent will use the LLM for BOM planning. If the LLM is down, it transparently falls back to a built-in local planner so the app still works.
- You can verify connectivity:
```bash
curl -s http://127.0.0.1:8005/llm_status
```
Expect `{"llm_enabled":true,"reachable":true,...}` when the model is available.

## Troubleshooting fast
- LLM not reachable: ensure `ollama serve` is running and the model is pulled; adjust `LLM_BASE_URL`/`LLM_MODEL`, then `./run.sh` again.
- Ports busy: stop old processes using `lsof -i :8000-8005` and kill them, then rerun.
- UI not updating: Ctrl+C the Streamlit terminal and start it again.
