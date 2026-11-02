# CareerSwipe AI: An Intelligent Multi-Agent Career & Job Matcher

[![CI (Linux)](https://github.com/V1KRANT13/CareerSwipe/actions/workflows/test.yml/badge.svg?branch=main)](https://github.com/V1KRANT13/CareerSwipe/actions/workflows/test.yml)
[![Python 3.13+](https://img.shields.io/badge/python-3.13%2B-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**CareerSwipe AI** is a production-oriented, AI-powered career platform designed to modernize the job discovery and matching experience through high-fidelity machine learning and natural language processing. By integrating an intuitive swipe-based interface with Transformer-based embedding search, heuristic scoring, and LLM-driven insight generation, it provides a deterministic and observable pipeline for intelligent job pairing.

> This project prioritizes robust backend architecture, AI model integration, and system scalability over simple wrapper logic, demonstrating a seamless fusion of machine learning pipelines with high-concurrency FastAPI microservices.

---

## 📽️ Demo & Walkthrough

* **Real-Time Job Ingestion**: Aggregating live job data from multiple remote platforms into a normalized, unified schema.
* **Transformer Match Scoring**: The ML engine evaluates candidate resumes against job requirements using vector embedding similarities and logistic scoring.
* **Generative AI Insights**: An integrated LLM pipeline generates clear, explainable match rationale, highlighting skill overlaps and domain gaps.
* **Interactive Interface**: A responsive, card-swiping UI allowing users to seamlessly interact with data-driven career recommendations.

---

## Quick Start

```bash
git clone [https://github.com/V1KRANT13/CareerSwipe.git](https://github.com/V1KRANT13/CareerSwipe.git)
cd CareerSwipe

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run test verification
pytest tests/

# Start backend server
python -m uvicorn app.main:app --reload
