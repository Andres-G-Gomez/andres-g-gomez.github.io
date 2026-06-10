---
layout: page
title: Graph-Based Fraud Detection for Bitcoin Transactions
description:
img: assets/img/12_project/bitcoin_fraud_thumbnail.svg
importance: 1
category: Independent
---

**Stack:** Python · pandas · PySpark · PyTorch · PyTorch Geometric · GCN · GraphSAGE · FastAPI · Docker · MLflow

## Overview

An end-to-end machine learning system that identifies illicit Bitcoin transactions by modelling the transaction network as a graph. The project spans the full lifecycle: distributed data pipeline engineering, Graph Neural Network modelling, a production FastAPI microservice, and complete MLOps infrastructure. It uses the Elliptic dataset — over 200,000 Bitcoin transactions, each with 166 features, connected by a directed edge list describing fund flows.

What makes this problem interesting is that transactions don't exist in isolation. Money flows between wallets, through mixers, across chains of activity. A single transaction might look unremarkable on its own but reveal clear patterns of illicit behaviour when you look at who sent funds to it and where it sends funds next. The core design decision in this project is to exploit that structure directly, rather than treating each transaction as an independent row of features.

---

## Data Pipeline

Raw data arrives as three CSV files: transaction features, class labels, and a directed edge list. The preprocessing pipeline loads and validates all three, removes transactions with unknown labels — including them silently corrupts the training signal since you cannot learn from examples you cannot verify — and encodes class labels into a clean binary target.

The edge list is filtered so only edges connecting two labelled nodes remain. The final graph object is constructed in PyTorch Geometric's `Data` format with node features, edge indices, and labels aligned. The processed graph is serialised to disk so the pipeline does not need to rerun on every training iteration.

The pipeline is implemented in both **pandas and PySpark**, reflecting the reality that datasets of this kind often live in distributed storage at production scale.

---

## Modelling

The modelling approach uses two Graph Neural Network architectures: a **Graph Convolutional Network (GCN)** and a **GraphSAGE** model. Both architectures allow each transaction node to incorporate information from its neighbourhood before making a classification decision — the key capability that separates them from any tabular model. A standard neural network or gradient boosting model can use the 166 node features, but has no mechanism to incorporate the fact that a transaction's immediate neighbours are themselves suspicious. GNNs propagate that signal across the graph.

GraphSAGE performed better in practice. It samples a fixed-size neighbourhood and learns an aggregation function over it, making it more scalable and better at generalising to subgraphs not seen during training.

**Handling class imbalance** was essential since illicit transactions are the minority class. Standard accuracy is meaningless — a model that always predicts "licit" scores well while flagging nothing. Three techniques address this:

- **Focal loss**, which down-weights easy negative examples so the model focuses on rare fraud cases
- **Class-weighted training**, which penalises missed detections more heavily during gradient updates
- **Threshold optimisation**, where the classification threshold is tuned on the validation set to find the right tradeoff between recall and precision, rather than defaulting to 0.5

---

## Evaluation

The project focuses on **Precision-Recall AUC** rather than ROC-AUC. ROC-AUC can look deceptively strong when the negative class dominates; PR-AUC specifically measures performance on the rare positive class, which is the only class that matters here.

| Metric | Score |
|---|---|
| PR-AUC | **0.87** |
| ROC-AUC | **0.97** |
| Recall | **84%** |
| Precision | **81%** |

In practical terms: of every 100 fraud cases present in the network, 84 are correctly flagged, and 4 in 5 alerts raised are genuine.

---

## API and Deployment

The trained model is exposed as a production microservice via **FastAPI**. The endpoint `POST /v1/predict_subgraph` accepts a JSON payload representing a small transaction subgraph — the central node's features, its neighbours' features, and the edges between them — runs GNN inference, and returns a fraud probability score with a `triage_status` field.

When the score exceeds a configurable threshold (default `0.70`), a background task triggers a downstream agentic workflow for SAR (Suspicious Activity Report) generation. The API returns the score immediately; the calling system is never blocked waiting for that workflow to complete.

The service is containerised using a **multi-stage Dockerfile**: dependencies installed in a build stage, only the artefacts copied into a minimal production image, application running as a non-root user. The fraud threshold is configurable via environment variable without rebuilding the image.

A `docker-compose.yml` brings up both the inference API and a local **MLflow** tracking server in a single command, with experiment runs and model artefacts persisted to a named volume.

---

## Engineering Decisions Worth Noting

**Graph structure over tabular features.** The strongest modelling decision was choosing GNNs over gradient boosting. A random forest over the 166 node features would be competitive but blind to neighbourhood context — the information that actually distinguishes a mixer node from an ordinary relay. That context requires a model that can reason over the graph.

**PR-AUC as the primary metric.** The conventional choice is ROC-AUC. It is the wrong metric here. On a heavily imbalanced dataset, a model with ROC-AUC of 0.97 might still miss most of the fraud cases it encounters. PR-AUC measures exactly the behaviour that matters in a fraud detection context: how well the model finds the rare positive class.

**Threshold tuning, not default 0.5.** The optimal classification threshold is a hyperparameter, not a constant. Tuning it on the validation set to maximise F1, or to hit a target recall at acceptable precision, is a necessary step that most implementations skip.

**Background SAR generation.** The API never makes the caller wait for downstream compliance workflows. The fraud score is synchronous; the SAR generation is fire-and-forget. This keeps API latency predictable and decouples the model service from the orchestration layer.

**Non-root container.** Security hardening in the Dockerfile is not an afterthought. Running as a non-root user and separating build from production image reduces attack surface and image size without changing how the service behaves.
