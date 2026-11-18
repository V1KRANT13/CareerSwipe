# CareerSwipe | Supervised ML Pipeline for Job Matching & Recommendation

**CareerSwipe** is an end-to-end Machine Learning system designed to modernize job discovery. It frames job matching as a supervised binary classification problem, combining Transformer-based semantic embeddings with engineered tabular features to predict candidate-job fit. The system includes a robust data ingestion pipeline, a Scikit-Learn inference engine, and an LLM-driven explanation module.

---

## 🧠 ML Model & Feature Engineering

The core recommendation engine is framed as a **supervised binary classification task**. User-job pairs are labeled based on user swipe actions (apply/save = 1, skip = 0). 

### Engineered Features (7)
1.  **Skill Overlap:** Jaccard similarity between candidate skills and job requirements.
2.  **Semantic Similarity:** Cosine similarity between Transformer embeddings (`all-MiniLM-L6-v2`) of the resume and job description.
3.  **Location Match:** Binary indicator if the job location aligns with candidate preferences.
4.  **Seniority Match:** Alignment between candidate experience level and job seniority.
5.  **Title Keyword Match:** Overlap between candidate's current title and job title.
6.  **Text Lengths:** Length of the resume and job description (proxy for information density).
7.  **Domain Alignment:** Indicator of industry/domain overlap between candidate and company.

### Training & Inference
*   **Algorithm:** Scikit-Learn Logistic Regression.
*   **Training Pipeline:** Supervised learning using a stratified 80/20 train-test split to ensure class balance.
*   **Inference:** The model outputs a calibrated probability score (0.0 to 1.0) which is used to rank and generate the personalized job feed.

---

## 📊 Model Evaluation & Metrics

To ensure the model generalizes well and does not suffer from data leakage or overfitting, the following evaluation methodologies are applied:

*   **Metrics:** Model performance is tracked using **Accuracy** and **ROC-AUC** (Receiver Operating Characteristic - Area Under Curve), which is critical for evaluating classification models on potentially imbalanced datasets.
*   **Validation Strategy:** Stratified K-Fold Cross-Validation is used during the training phase to ensure every class is represented proportionally in each fold.
*   **Feature Importance:** Logistic Regression coefficients are analyzed to interpret which features (e.g., skill overlap vs. semantic similarity) contribute most to a successful match, allowing for continuous improvement of the feature engineering pipeline.

---

## 🏗️ System Workflow & Architecture

CareerSwipe operates through an event-driven data and inference pipeline:

1.  **Ingestion & Normalization:** Automated background scrapers pull raw job postings, normalize data fields, and store them in a relational database.
2.  **Candidate Profiling:** Users upload resumes which are parsed to extract structured competencies and build embedding profiles.
3.  **Hybrid Inference:** The FastAPI backend queries the recommendation engine, combining PyTorch Transformer cosine similarity with Logistic Regression scoring.
4.  **Insight Generation:** LLMs synthesize explanation rationales on-demand.
5.  **State Interaction:** User swipes persist actions (saves, applies, skips) to act as ground truth labels for future model retraining.

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
        G --> H1[Resume & JD Embedding Generation]
        D --> H2[Feature Engineering Pipeline]
        H1 --> H2
        H2 --> H3[Logistic Regression Classifier]
        H3 --> H4[Probability Score & Ranking]
        E --> H5[LLM RAG Insight Generator]
    end

    subgraph API & UI Interaction
        H4 --> I[Scored Recommendation Feed]
        H5 --> I
        I --> J[Interactive Swipe UI]
        J --> K[User Action State Persistence]
        K --> D
    end
