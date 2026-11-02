# CareerSwipe AI: Intelligent Multi-Agent Career & Job Matching Platform

[![CI (Linux)](https://github.com/V1KRANT13/CareerSwipe/actions/workflows/test.yml/badge.svg?branch=main)](https://github.com/V1KRANT13/CareerSwipe/actions/workflows/test.yml)
[![Python 3.13+](https://img.shields.io/badge/python-3.13%2B-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**CareerSwipe AI** is an enterprise-grade, asynchronous AI-powered career platform designed to modernize job discovery and matching. By combining a swipe-driven user interface with Transformer-based semantic embeddings, heuristic logistic scoring, and LLM-driven insight generation, CareerSwipe AI delivers a highly accurate, observable, and interactive job application workflow.

Unlike simple wrapper applications, CareerSwipe AI emphasizes robust software engineering, modular clean architecture, and asynchronous MLOps practices to process candidates and job postings at scale.

---

## 📋 Table of Contents

- [Overview & Key Features](#-overview--key-features)
- [System Workflow & Architecture](#-system-workflow--architecture)
- [AI & ML Matching Engine](#-ai--ml-matching-engine)
- [Tech Stack & System Components](#-tech-stack--system-components)
- [API Endpoints Overview](#-api-endpoints-overview)
- [Database & Schema Architecture](#-database--schema-architecture)
- [Installation & Quick Start](#-installation--quick-start)
- [Environment Variables](#-environment-variables)
- [Testing & Quality Assurance](#-testing--quality-assurance)
- [Docker Deployment](#-docker-deployment)
- [License](#-license)

---

## 🌟 Overview & Key Features

### 1. Multi-Source Autonomous Ingestion
- Unified web-scraping pipeline designed to fetch, clean, and normalize job data from multiple remote career platforms (e.g., Remotive, RemoteOK, Jobicy).
- Deduplication and automated keyword extraction ensure clean data insertion into the relational database.

### 2. Hybrid Matching Pipeline (Scikit-Learn + PyTorch)
- **Semantic Vector Search**: Employs Hugging Face Transformers (`all-MiniLM-L6-v2`) and PyTorch to generate high-dimensional embeddings for candidate resumes and job descriptions.
- **Logistic Regression Scoring**: Predicts match probabilities based on candidate interactions, skill overlap percentages, and domain experience alignment.

### 3. Generative AI RAG & Insights Engine
- Integrates Google GenAI SDK (Gemini Pro) / Ollama LLM pipelines to synthesize actionable feedback.
- Solves the "black-box ML" problem by giving candidates clear "Why You Match" summaries, highlighting key technical strengths and skill gaps.

### 4. Automated Resume Parser
- Extracts unstructured text from candidate PDF and DOCX files.
- Automatically isolates contact details, education history, and technical skill stacks to build dynamic user profiles.

### 5. Asynchronous FastAPI Architecture
- Asynchronous routes powered by `uvicorn` and `asyncio` for low-latency job feed generation.
- Decoupled service layers implementing Clean Architecture and Dependency Inversion principles.

---

## 🏗️ System Workflow & Architecture

CareerSwipe AI is structured around an event-driven data and inference pipeline. The system operates through the following core operational flow:

1. **Ingestion & Normalization**: Automated background scrapers pull raw job postings, normalize data fields, and store them securely[cite: 8].
2. **Candidate Profiling**: Users upload resumes which are parsed to extract structured competencies and build embedding profiles[cite: 8].
3. **Hybrid Inference**: The FastAPI backend queries the recommendation engine, combining PyTorch Transformer cosine similarity with logistic scoring[cite: 8].
4. **Insight Generation**: LLMs synthesize explanation rationales on-demand[cite: 8].
5. **State Interaction**: User swipes persist actions (saves, applies, skips) to refine future recommendations[cite: 8].

The following diagram outlines the overall component architecture:

```mermaid
graph TD
    subgraph Ingestion Layer
        A1[RemoteOK API] --> B[Unified Scraper Engine]
        A2[Remotive API] --> B
        A3[Jobicy API] --> B
    end

    subgraph Data & Storage Layer
        B --> C[Data Normalization Pipeline]
        C --> D[(PostgreSQL / SQLite)]
    end

    subgraph Core Backend Services
        D --> E[FastAPI REST Server]
        E --> F[Auth & JWT Service]
        E --> G[Resume Parser Engine]
    end

    subgraph AI & ML Intelligence
        G --> H1[PyTorch & Transformer Embeddings]
        D --> H2[Logistic Match Scorer]
        E --> H3[LLM RAG Insight Generator]
    end

    subgraph API & UI Interaction
        H1 --> I[Scored Recommendation Feed]
        H2 --> I
        H3 --> I
        I --> J[Interactive Swipe UI]
        J --> K[User Action State Persistence]
        K --> D
    end
