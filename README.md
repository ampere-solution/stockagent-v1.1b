# Stock Agent - Agentic AI

## User Interface - Web Dashboard
StockAgent is a four-agent pipeline that performs stock analysis using local GGUF language models running on CPU via `llama-cpp-python`. The system combines rule-based data collection, LLM powered natural language analysis, ML-based anomaly detection, and deterministic report generation.  
The web dashboard has two pages, both served at http://<your_ip_address>:5005.

### Dashboard
- Ticker input prefilled with AAPL, AMD, GOOGL, NVDA, ARM, TSLA and a Run button that POSTs to /api/run.
- Pipeline strip:  four nodes wired with arrows showing each agent and its current state (IDLE → running → done)
    - Data Collection (rule-based)
    - Data Analysis (local LLM + ML)
    - Decision Engine (local LLM reasoning)
    - Records Officer (rule-based)
- Live terminal: streams log lines from /api/stream (SSE) as the pipeline runs.
- Stat cards: portfolio allocation donut chart, per-ticker sentiment, and pipeline summary card.
- Analysis results: a grid of stock cards showing each ticker’s recommendation, confident gauge, and metrics.

### Report Viewer
- Loaded after a run completes.  Show 3 sections fed by /api/report: Executive Summary, Stock Analysis, and Porttfolio Overview, plus a download button for the raw report.json.
<img width="2091" height="1094" alt="Unknown.png" src="Unknown.png" />

## Architectural Diagram
Below is the architectural diagram which visualize the Stock Agent system from different angles:
- System Overview.
- Four agent pipeline.
- Model management.
- SSE streaming.
- Docker deployment.
- Data flow.
- Dashboard UI layout.
- Report
<img width="2091" height="1094" alt="image-20260504-172917.png" src="image-20260504-172917.png" />

## What This Demo Shows
This is a multi-agent AI system that analyzes stocks and produces investment recommendations. It demonstrates an agentic AI architecture where four specialized agents work sequentially, each using the optimal approach for its task:
<img width="2091" height="1094" alt="image-20260504-174733.png" src="image-20260504-174733.png" />
- Agent 1 gathers real-time financial data from Yahoo Finance (no AI involved)
- Agent 2 analyzes that data using a combination of Ampere Optimized Inference model:  llama-3.1-8b-instruct-Q8R16.gguf, and machine learning models,
- Agent 3 synthesizes everything into per-stock recommendations with confidence scores and portfolio allocations using Ampere Optimized Inference model: Llama-3.2-3B-Instruct-Q4_K_4.gguf
- Agent 4 looks at output json files and put together a presentation summary report.
The default ticker set is **AAPL, AMD, GOOGL, NVDA, ARM, TSLA** (but can be configured to any stock symbols).
- All AI inferences run **locally on CPU** using Ampere Optimized Inference AI models in GGUF-format models via llama-cpp-python. No cloud APIs, no GPU required, no per-request costs.

## Target Audience
- Financial and Investment teams or companies:
    - Retails traders.
    - Portfolio managers.
    - Financial analysts.
    - Investment firms.
    -Financial institutes.

## Key Message - What are we trying to convince of?
- Most people still think AI = single LLM inference.  Agentic AI is a real production workload and pipeline which shows:
    - Multiple cooperative agents
    - Decision synthesis
- On premise or Cloud-native deployment with Agentic AI.  Agentic AI can deploy today - economically - in OCI A4.
- CPU only (No need GPU)
- A practical AI platform.

## Proof Points - How does It Show This?
### Agent 1 - Data Collection Agent
- The agent 1 which is **Data Collection agent**  runs 6 sequential steps using the yfinance API:
| Step | Method | What it fetches |
|:-----|:-------|:----------------|
| 1a   | fetch_realtime_prices() | Current price, previous close, change, market cap, volume, P/E, beta, 52-week high/low |
| 1b   | fetch_historical_data() | 1 year of daily OHLCV (Open/High/Low/Close/Volume) data as pandas DataFrames |






    
<img width="2091" height="1094" alt="Screenshot 2026-02-09 at 9 24 45 AM" src="https://github.com/user-attachments/assets/0cf8199b-6c0d-422e-9a88-861814b166ad" />
