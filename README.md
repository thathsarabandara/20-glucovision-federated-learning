<div align="center">

# 🌐 GlucoVision Federated Learning

**Privacy-preserving collaborative model training — models improve without patient data leaving devices.**  
*Flower (flwr) · FedAvg/FedProx · Differential privacy (Opacus) · Secure aggregation · gRPC*

[![Flower](https://img.shields.io/badge/Flower-FL%20Framework-FF6F61?style=for-the-badge)](#)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=for-the-badge&logo=pytorch)](#)
[![Opacus](https://img.shields.io/badge/Opacus-DP-412991?style=for-the-badge)](#)
[![gRPC](https://img.shields.io/badge/gRPC-Communication-244c5a?style=for-the-badge)](#)
[![Docker](https://img.shields.io/badge/Docker-Containerised-2496ED?style=for-the-badge&logo=docker)](#)
[![Status](https://img.shields.io/badge/Status-In%20Development-f59e0b?style=for-the-badge)](#)

</div>

---

## 📌 Purpose

GlucoVision Federated Learning enables **privacy-preserving collaborative model training** — multiple hospitals or research institutions train a shared glucose prediction model without sharing raw patient records. Using Flower with differential privacy and secure aggregation, raw patient data **never leaves the institution**.

> **Regulatory basis:** GDPR Article 89 (research exemption with safeguards) + HIPAA Safe Harbor — raw patient data never transmitted.

---

## 📁 Project Structure

```
20-glucovision-federated-learning/
└── (Git repository initialised — structure to be scaffolded)
```

---

## ✨ Planned Features (by phase)

### Phase 1 — FL Server
- [ ] Flower FL server — coordinate training rounds
- [ ] FedAvg aggregation strategy
- [ ] Client enrollment REST API
- [ ] FL round metrics tracking

### Phase 2 — Privacy
- [ ] Differential privacy via Opacus (ε-DP with Gaussian noise)
- [ ] Privacy budget tracking (Rényi DP accountant)
- [ ] Secure aggregation (encrypted gradients)
- [ ] FedProx for non-IID patient data handling

### Phase 3 — Distribution & Integration
- [ ] Distributable client SDK (`pip install glucovision-fl-client`)
- [ ] Model distribution to MLflow registry → `09`, `12`
- [ ] FL round monitoring dashboard in `04` web dashboard
- [ ] Mutual TLS for all FL communication

---

## 🚀 Getting Started

### Prerequisites

- Python ≥ 3.11, PyTorch, Flower (flwr), Docker & Docker Compose

### FL Server

```bash
pip install -r requirements.txt

# Start FL server
python server.py --num-rounds 50 --min-clients 3

# Or via Docker
docker compose up --build
```

### FL Client (at each institution)

```bash
pip install glucovision-fl-client

# Run local training client
glucovision-fl-client train \
  --server-address fl.glucovision.health:8080 \
  --data-path /local/patient_data/ \
  --model glucose_prediction
```

---

## 🏗️ Planned Tech Stack

| Layer | Technology |
|---|---|
| FL Framework | Flower (flwr) 1.x |
| Deep Learning | PyTorch 2.x |
| Differential Privacy | Opacus |
| Secure Aggregation | OpenMined PySyft / custom SecAgg |
| Experiment Tracking | MLflow |
| Communication | gRPC (Flower built-in) |
| Backend API | FastAPI (enrollment + metrics) |
| Containerisation | Docker |

---

## 🔗 Backend Dependencies

| Service | Interaction |
|---|---|
| `09` food-recognition | Source model for food classification FL |
| `12` glucose-prediction | Source model for glucose forecasting FL |
| MLflow | Aggregated model registry |
| `04` web-dashboard | FL round monitoring UI |
| `05` api-gateway | Client connections to FL server |

---

## 🔐 Security Notes

- **No raw data transmission** — gradient updates only
- Mutual TLS for FL server ↔ client authentication
- Secure aggregation: server sees only sum of encrypted gradients
- Differential privacy: mathematical guarantee of individual privacy
- Full audit trail: clients participated, rounds completed, ε budget spent
- GDPR / HIPAA compliant — data minimisation enforced

---

## 📊 Privacy Budget Tracking

```json
{
  "client_id": "hospital-uuid",
  "model": "glucose_prediction_v2",
  "total_rounds": 50,
  "epsilon_spent": 0.85,
  "delta": 1e-5,
  "noise_multiplier": 1.1,
  "budget_limit": 2.0,
  "status": "active"
}
```

---

## 🧪 Testing (Planned)

```bash
pytest tests/simulation/   # FedAvg converges in 10 rounds
pytest tests/dp/           # Opacus accuracy within 2% of non-private
pytest tests/secagg/       # Server cannot reconstruct individual gradients
pytest tests/sdk/          # Client SDK roundtrip: install → train → submit
pytest tests/budget/       # 100 rounds → ε budget tracked correctly
```

---

<div align="center">

*Part of the [GlucoVision Platform](../01-glucovision-platform-architecture) — 21-Repo AI Diabetes Management System*

</div>
