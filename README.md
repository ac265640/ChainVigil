# ⛓️ ChainVigil — Cross-Channel Fraud Intelligence System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![PyTorch](https://img.shields.io/badge/PyTorch-2.5-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Neo4j](https://img.shields.io/badge/Neo4j-5.x-008CC1?style=for-the-badge&logo=neo4j&logoColor=white)

**A graph-native financial crime detection system that identifies cross-channel money mule networks in near real-time.**

Integrates multi-source transaction logs (UPI, ATM, Web, Mobile App) into a **Unified Entity Graph (UEG)** and applies **Graph Neural Networks** to detect high-velocity fund movement and mule-ring clusters.

</div>

---

## 🏗️ Architecture

```
Multi-Channel Logs  →  Data Ingestion (FastAPI)
  → Unified Entity Graph (NetworkX + Neo4j)
    → Temporal GNN Scorer (GraphSAGE + GAT)
      → Risk Intelligence Engine
        → XAI Audit Reports
          → Dashboard (React + Vite)
```

---

## 🛠️ Tech Stack

| Component        | Technology                                |
|------------------|-------------------------------------------|
| Backend / API    | FastAPI (Python 3.10+)                    |
| Graph Database   | Neo4j 5.x + NetworkX (in-memory fallback) |
| Machine Learning | PyTorch Geometric — GraphSAGE + GAT       |
| XAI              | Gradient × Input / SHAP                   |
| Frontend         | React 19 + Vite 5                         |
| Graph Viz        | react-force-graph-2d                      |
| Deployment       | Docker · Render · Railway                 |

---

## 📁 Project Structure

```
ChainVigil/
├── README.md
├── ChainVigil_Documentation.md    # Full technical documentation
├── Dockerfile                     # Container definition
├── Procfile                       # Heroku / Render process file
├── railway.json                   # Railway deployment config
├── render.yaml                    # Render deployment config
├── requirements.txt               # Root-level Python deps (points to backend/)
├── main.py                        # Entrypoint — runs uvicorn
├── test.py                        # Quick smoke tests
├── test_pipeline.py               # End-to-end pipeline tests
│
├── backend/
│   ├── config.py                  # All settings (read from env vars)
│   ├── main.py                    # FastAPI app — 15+ endpoints
│   ├── requirements.txt           # Backend Python dependencies
│   ├── data/
│   │   └── generator.py           # Synthetic multi-channel data generator
│   ├── graph/
│   │   ├── builder.py             # Unified Entity Graph (NetworkX)
│   │   └── neo4j_client.py        # Optional Neo4j integration
│   ├── gnn/
│   │   ├── features.py            # 20+ account feature engineering
│   │   ├── dataset.py             # NetworkX → PyTorch Geometric
│   │   ├── model.py               # GraphSAGE + GAT hybrid model
│   │   ├── train.py               # Training loop (class-balanced)
│   │   └── predict.py             # Inference & risk scoring
│   ├── risk/
│   │   └── engine.py              # Cluster detection & action engine
│   ├── intelligence/              # LLM / NLP explainability layer
│   ├── xai/
│   │   ├── explainer.py           # Gradient×Input feature attribution
│   │   └── report.py             # SAR / Audit report generator
│   ├── realtime/                  # Streaming & real-time hooks
│   ├── observability/
│   │   └── metrics.py             # Prometheus-compatible metrics
│   └── experiments/               # Research notebooks & ablations
│
└── frontend-app/
    ├── package.json
    ├── vite.config.js
    └── src/
        ├── main.jsx               # React entry point
        ├── App.jsx                # Dashboard — 6 tabs
        ├── index.css              # Design system & dark theme
        ├── LoadingScreen.jsx      # Animated boot screen
        ├── DecryptedText.jsx      # Cyberpunk text animation
        └── GridScan.jsx           # Animated graph scanner
```

---

## ✅ Prerequisites

| Tool | Version |
|------|---------|
| **Python** | 3.10 or higher |
| **Node.js** | 18 or higher |
| **Git** | Any recent version |

> **Neo4j is optional.** The system automatically falls back to in-memory NetworkX if Neo4j is unavailable.

---

## 🚀 Local Setup & Run

### 1. Clone the repository

```bash
git clone https://github.com/ac265640/ChainVigil.git
cd ChainVigil
```

### 2. Configure environment variables

Copy the example env file and fill in your values — **never commit `.env` to git**:

```bash
cp .env.example .env
# Edit .env with your Neo4j credentials (optional)
```

### 3. Install backend dependencies

```bash
pip install -r backend/requirements.txt
```

> **⚠️ PyTorch Note:** If the install fails, install PyTorch manually first:
> ```bash
> pip install torch --index-url https://download.pytorch.org/whl/cpu
> pip install torch-geometric
> pip install -r backend/requirements.txt
> ```

### 4. Start the backend

Run from the **project root** (not inside `backend/`):

```bash
uvicorn backend.main:app --reload --port 8000
```

| URL | Description |
|-----|-------------|
| `http://localhost:8000` | Backend API |
| `http://localhost:8000/docs` | Swagger / OpenAPI docs |
| `http://localhost:8000/health` | Health check |

### 5. Start the frontend

Open a **second terminal**:

```bash
cd frontend-app
npm install
npm run dev
```

✅ Dashboard live at → **`http://localhost:5173`**

### 6. Run the pipeline

1. Open `http://localhost:5173`
2. Click **"Run Full Pipeline"** (green button)
3. ~1–2 min for all 4 phases:
   - ✅ Generate synthetic data (900 accounts, 4 500+ transactions)
   - ✅ Build the Unified Entity Graph
   - ✅ Train the GNN model (40 epochs, GraphSAGE + GAT)
   - ✅ Run risk analysis & cluster detection
4. Explore: **Graph · Accounts · Clusters · Intelligence · XAI Auditor · Reports**

---

## 🐳 Docker

```bash
docker build -t chainvigil .
docker run -p 8000:8000 chainvigil
```

---

## 📡 API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/generate` | Generate synthetic transaction data |
| `POST` | `/api/ingest` | Build graph from generated data |
| `POST` | `/api/train` | Train GNN mule detection model |
| `POST` | `/api/analyze` | Run risk analysis & clustering |
| `POST` | `/api/pipeline/run` | Run entire pipeline end-to-end |
| `GET` | `/api/graph/stats` | Graph statistics |
| `GET` | `/api/graph/visual` | Graph data for visualization |
| `GET` | `/api/accounts` | List accounts with risk scores |
| `GET` | `/api/accounts/{id}` | Account details + neighbors |
| `GET` | `/api/clusters` | Detected mule ring clusters |
| `GET` | `/api/clusters/{id}` | Specific cluster details |
| `GET` | `/api/explain/{id}` | XAI explanation for an account |
| `GET` | `/api/report` | Generate full audit report |
| `GET` | `/api/export/anonymized` | Privacy-preserving anonymized export |
| `GET` | `/health` | Service health check |
| `GET` | `/metrics` | Observability metrics |

---

## 🧠 How It Works

| Step | Module | What Happens |
|------|--------|-------------|
| 1 | `data/generator.py` | Generates realistic multi-channel transactions with embedded mule ring patterns (chain hops, hub-spoke, circular flows, smurfing) |
| 2 | `graph/builder.py` | Builds a Unified Entity Graph with Account, Device, IP, ATM, and Transaction nodes with temporal edges |
| 3 | `gnn/features.py` | Computes 20+ features per account: velocity, centrality, clustering, channel diversity, shared devices/IPs |
| 4 | `gnn/model.py` | Hybrid GraphSAGE + GAT with class-balanced focal loss for semi-supervised mule detection |
| 5 | `risk/engine.py` | Post-processing: cluster detection, velocity metrics, action recommendations |
| 6 | `xai/explainer.py` | Gradient-based feature attribution with human-readable explanations |
| 7 | `api/export` | SHA-256 hashed anonymized data sharing for inter-bank collaboration |

---

## 🔒 Privacy-Preserving Export

ChainVigil includes an anonymization layer for safe inter-bank data sharing:

| Removed | Preserved |
|---------|-----------|
| Account holder names | Graph topology |
| Bank names & countries | Risk scores & recommended actions |
| Device IDs & IP addresses | Behavioral features (degree, velocity, channel diversity) |
| ATM locations | Cluster memberships |
| Exact transaction amounts | Amount buckets (e.g. `5K–10K`) |

Account IDs are SHA-256 hashed with a secret salt → `ACC-001` becomes `a3f8c2e1b9d04752…`

**Endpoint:** `GET /api/export/anonymized`

---

## 🌐 Deployment

### Render (recommended)

1. Connect your GitHub repo to [render.com](https://render.com)
2. Render auto-detects `render.yaml`
3. Set env vars in the Render dashboard (see **Environment Variables** below)

### Railway

1. Connect your GitHub repo to [railway.app](https://railway.app)
2. Railway uses `railway.json` (Dockerfile build)
3. Set env vars in the Railway dashboard

---

## 🔑 Environment Variables

All secrets must be set via the platform dashboard or a local `.env` file that is **never committed**:

| Variable | Default | Description |
|----------|---------|-------------|
| `NEO4J_URI` | `bolt://localhost:7687` | Neo4j connection URI |
| `NEO4J_USER` | `neo4j` | Neo4j username |
| `NEO4J_PASSWORD` | *(none)* | **Set via dashboard / `.env` only** |
| `NUM_ACCOUNTS` | `900` | Synthetic account count |
| `NUM_TRANSACTIONS` | `4500` | Synthetic transaction count |
| `PORT` | `8000` | Server port (auto-set by platform) |

---

## ⚠️ Troubleshooting

| Problem | Solution |
|---------|----------|
| `ModuleNotFoundError: No module named 'backend'` | Run `uvicorn` from the **project root**, not from inside `backend/` |
| PyTorch install fails | Install PyTorch separately first (see Step 3 above) |
| Frontend shows CORS errors | Make sure the backend is running on port `8000` |
| Neo4j connection warning | Normal — system works fine without Neo4j |
| `ENOSPC: System limit for file watchers` | `echo fs.inotify.max_user_watches=524288 \| sudo tee -a /etc/sysctl.conf` (Linux only) |

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">
Built with ❤️ · <a href="https://github.com/ac265640/ChainVigil">github.com/ac265640/ChainVigil</a>
</div>
