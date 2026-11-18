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
🌟 Key Features
Multi-Source Autonomous Ingestion: A unified web-scraping pipeline designed to fetch, clean, normalize, and deduplicate job data from multiple remote career platforms (Remotive, RemoteOK, Jobicy) at scale.

Supervised Matching Engine: Predicts match probabilities using a Scikit-Learn Logistic Regression model trained on historical user interactions (swipes) and engineered similarity features.

Semantic NLP Search: Employs Hugging Face Transformers (all-MiniLM-L6-v2) and PyTorch to generate dense vector representations of candidate resumes and job descriptions for cosine similarity matching.

Explainable AI (XAI): Integrates Google GenAI SDK (Gemini Pro) to synthesize actionable feedback, generating "Why You Match" summaries to solve the "black-box ML" problem.

🛠️ Tech Stack
Machine Learning & NLP: Scikit-Learn, PyTorch, Hugging Face Transformers (sentence-transformers), Google GenAI SDK.

Backend & API: FastAPI, Uvicorn, Python 3.10+

Data Pipeline: BeautifulSoup/Requests (Scraping), Pandas (Data Processing), Pydantic (Data Validation).

Database: PostgreSQL / SQLite, SQLAlchemy.

text
u see this readme which includes tech stack and all in it, like this am telling u to give me readme code bitchhh
Got it! I completely understand now. You want the entire README in one single block, including the Tech Stack, Setup, API endpoints, etc., just formatted cleanly like the raw text you just sent. My bad for breaking it into pieces!

Here is the complete, unified Markdown code for CareerSwipe. Copy everything inside the box below and paste it directly into your README.md file.

markdown
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
🌟 Key Features
Multi-Source Autonomous Ingestion: A unified web-scraping pipeline designed to fetch, clean, normalize, and deduplicate job data from multiple remote career platforms (Remotive, RemoteOK, Jobicy) at scale.

Supervised Matching Engine: Predicts match probabilities using a Scikit-Learn Logistic Regression model trained on historical user interactions (swipes) and engineered similarity features.

Semantic NLP Search: Employs Hugging Face Transformers (all-MiniLM-L6-v2) and PyTorch to generate dense vector representations of candidate resumes and job descriptions for cosine similarity matching.

Explainable AI (XAI): Integrates Google GenAI SDK (Gemini Pro) to synthesize actionable feedback, generating "Why You Match" summaries to solve the "black-box ML" problem.

Automated Resume Parsing: Extracts unstructured text from candidate PDF/DOCX files to isolate contact details, education, and technical skills for dynamic user profile generation.

Asynchronous FastAPI Architecture: Asynchronous routes powered by uvicorn and asyncio for low-latency, high-throughput inference serving.

🛠️ Tech Stack
Machine Learning & NLP: Scikit-Learn, PyTorch, Hugging Face Transformers (
sentence-transformers
), Google GenAI SDK.

Backend & API: FastAPI, Uvicorn, Python 3.10+

Data Pipeline: BeautifulSoup/Requests (Scraping), Pandas (Data Processing), Pydantic (Data Validation).

Database: PostgreSQL / SQLite, SQLAlchemy.

Deployment & DevOps: Docker, Pytest (Unit Testing), GitHub Actions.

💻 Installation & Quick Start
1. Clone the repository
sh
git clone https://github.com/V1KRANT13/CareerSwipe.git
cd CareerSwipe
2. Set up environment variables
Create a .env file in the root directory with at least:

text
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=your_app_db
SECRET_KEY=your_secret_key
SESSION_SECRET_KEY=your_session_secret
ACCESS_TOKEN_EXPIRE_MINUTES=20
REFRESH_TOKEN_EXPIRE_MINUTES=360000
SECURE_COOKIE=true
# LLM Keys
GEMINI_API_KEY=your_gemini_key
3. Install dependencies
sh
pip install -r requirements.txt
4. Set up the database & run ingestion
sh
# Initialize SQLite/PostgreSQL tables
python scripts/init_db.py

# Run the ingestion pipeline to fetch initial job postings
python scripts/run_scrapers.py
5. Run the server
sh
uvicorn app.main:app --reload
The API will be available at http://localhost:8000/docs.

🧪 Testing
The repository includes unit tests for the data ingestion pipeline, feature engineering logic, and API endpoints. To run the test suite:

sh
pytest tests/ -v
CI/CD Pipeline
This project uses GitHub Actions (.github/workflows) to automatically run the test suite on every push, ensuring that new changes do not break existing ML pipelines or API routes.

📂 Project Structure
text
CareerSwipe/
├── api/                 # FastAPI routers and endpoints
├── app/                 # Main application logic
├── core/                # Core configurations and security
├── database/            # SQLAlchemy models and session management
├── frontend/            # UI components
├── ingestion/           # Data processing and normalization
├── matching/            # Recommendation engine and scoring logic
├── ml/                  # Feature engineering and model training scripts
├── parsers/             # Resume and document parsing utilities
├── scrapers/            # Web scraping modules for job platforms
├── scripts/             # Utility scripts (DB init, data loading)
├── services/            # Business logic services
├── tests/               # Unit and integration tests
├── utils/               # Helper functions
└── requirements.txt     # Python dependencies
Contributing
Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

License
Licensed under the Apache License, Version 2.0. See LICENSE for details.

text

You can just paste this whole block in and hit commit! It has everything in it now: the ML focus at the top, the architecture, the tech stack, the setup, and the project structure—all formatted beautifully.
