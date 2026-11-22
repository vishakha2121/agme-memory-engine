<div align="center">

# 🧠 AGME — Artificial General Memory Engine

**Give your AI Agents a Real Brain. Not Just a Context Window.**

[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/Frontend-React-61DAFB?style=for-the-badge&logo=react)](https://reactjs.org/)
[![Gemini API](https://img.shields.io/badge/AI-Gemini%20API-4285F4?style=for-the-badge&logo=google)](https://aistudio.google.com/)
[![SQLite](https://img.shields.io/badge/Database-SQLite-003B57?style=for-the-badge&logo=sqlite)](https://www.sqlite.org/)
[![ChromaDB](https://img.shields.io/badge/Vector%20DB-ChromaDB-FF6B6B?style=for-the-badge)](https://www.trychroma.com/)

**A CPU-friendly, biologically-inspired memory engine for AI agents.**  
*Episodic • Semantic • Procedural • Working Memory*  
*Built for practice. Designed to impress.*

</div>

---

## 📖 Table of Contents
1. [The Problem](#-the-problem)
2. [The Solution (AGME)](#-the-solution-agme)
3. [The 4 Memory Types](#-the-4-memory-types)
4. [System Architecture](#-system-architecture)
5. [Core Features](#-core-features)
6. [Tech Stack](#-tech-stack)
7. [Database Schema](#-database-schema)
8. [API Endpoints](#-api-endpoints)
9. [Memory Lifecycle (How it works)](#-memory-lifecycle-how-it-works)
10. [Installation & Setup](#-installation--setup)
11. [Project Folder Structure](#-project-folder-structure)
12. [Demo Flow](#-demo-flow)
13. [Interview Preparation (Design Decisions)](#-interview-preparation-design-decisions)
14. [Future Scope](#-future-scope)

---

## ❌ The Problem

Modern AI agents (ChatGPT, Gemini, Claude) suffer from **context amnesia**. 
Once you close the chat window, the AI forgets everything about you. Every new session starts from absolute zero. Why? Because LLMs only have a temporary **Context Window**, not a permanent memory system.

Current solutions like massive context windows or basic RAG (Retrieval-Augmented Generation) are flawed:
- **Big Context Windows:** Expensive, slow, and still temporary.
- **Basic Vector Search:** Flat, unorganized, and cannot "forget" irrelevant data.

---

## ✅ The Solution (AGME)

**AGME (Artificial General Memory Engine)** is a middleware memory layer that sits between the user and the LLM. 
It mimics **human cognitive psychology** by implementing 4 distinct types of memory. It automatically extracts facts from conversations, organizes them, consolidates them over time, and intelligently **forgets** irrelevant information.

**Why "CPU-Friendly"?**
Most AI memory projects require heavy GPUs to run local models (like Llama-3 or Sentence-BERT). AGME offloads all heavy AI computation to the **Gemini API (Cloud)**. Your local machine only runs lightweight orchestration (FastAPI, SQLite, ChromaDB). This means the entire system runs smoothly on an average 8GB RAM laptop with **zero GPU requirements**.

---

## 🧩 The 4 Memory Types

Inspired by cognitive psychology (Atkinson-Shiffrin model), AGME classifies memories just like a human brain:

| Memory Type | What it stores | Example | Lifetime | Decay Speed |
| :--- | :--- | :--- | :--- | :--- |
| 🎬 **Episodic** | Time-stamped events & experiences | *"On Monday, user asked to debug a Python error."* | Medium | Fast |
| 📚 **Semantic** | General facts & concepts (Timeless) | *"User's name is Rahul. User is learning Python."* | Long | Slow |
| 🛠️ **Procedural** | How-to knowledge & skills | *"User prefers short answers. Use 'pdb' for debugging."* | Very Long | Very Slow |
| ⚡ **Working** | Immediate short-term context | *"Current topic: React hooks."* | Seconds/Mins | Instant |

---

## 🏗️ System Architecture

```text
┌─────────────────┐
│   React UI      │ (Frontend Dashboard & Chat)
│  (Vite+Tailwind)│
└────────┬────────┘
         │ REST API (Axios)
         ▼
┌─────────────────┐      ┌──────────────────────┐
│  FastAPI Backend│◄────►│  Gemini API (Cloud)  │
│  (Python 3.10+) │      │  - LLM (gemini-flash)│
└────────┬────────┘      │  - Embeddings        │
         │               └──────────────────────┘
         ├─────────────────────────┐
         ▼                         ▼
┌─────────────────┐      ┌──────────────────────┐
│    SQLite DB    │      │      ChromaDB        │
│ (SQLAlchemy ORM)│      │ (Vector Store for    │
│ - Users         │      │  Semantic Search)    │
│ - Memories      │      └──────────────────────┘
│ - Logs          │
└─────────────────┘

git clone https://github.com/vishakha2121/agme-memory-engine.git
cd agme-memory-engine

agme-memory-engine/
├── backend/
│   ├── app/
│   │   ├── api/v1/endpoints/    # Chat, Memory, Analytics routes
│   │   ├── core/                # Config, logging
│   │   ├── db/                  # SQLAlchemy setup
│   │   ├── memory_engine/       # ⭐ Core Logic (Consolidation, Decay)
│   │   ├── models/              # ORM Tables
│   │   ├── schemas/             # Pydantic models
│   │   ├── services/            # Gemini API, Embeddings
│   │   ├── vector_store/        # ChromaDB setup
│   │   └── workers/             # APScheduler background jobs
│   ├── data/                    # SQLite DB & Chroma files
│   ├── scripts/                 # DB init scripts
│   └── main.py                  # Entry point
├── frontend/
│   ├── src/
│   │   ├── components/          # UI components, Chat, Memory Cards
│   │   ├── pages/               # Dashboard, Chat, Memories
│   │   ├── hooks/               # Custom React hooks
│   │   ├── api/                 # Axios API calls
│   │   └── utils/               # Helpers
└── docs/                        # Architecture diagrams

cd backend
python -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\activate
pip install -r requirements.txt


cd ../frontend
npm install
npm run dev