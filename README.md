# LLM-Driven Alpha Extraction from Earnings Call Transcripts

This repository contains the code and research material for a study examining whether modern **Large Language Model (LLM)** embeddings can extract economically meaningful signals from corporate earnings call transcripts and translate them into systematic equity investment strategies.

The central objective of the project is to evaluate whether **unstructured financial language contains information that is not fully incorporated into market prices**, and whether these signals can generate **risk-adjusted alpha**.

---

# Research Motivation

Financial markets process a large volume of **unstructured textual information**, including:

- Earnings call transcripts  
- Management guidance  
- Risk disclosures  
- Analyst Q&A discussions  

Traditional quantitative strategies rely heavily on **structured numerical data**, while classical text-analysis approaches typically use **dictionary-based sentiment scores or bag-of-words models**.

Recent advances in **Large Language Models (LLMs)** allow textual information to be represented through **dense semantic embeddings**, potentially capturing richer contextual signals from financial communication.

This project investigates whether such representations improve the ability to **extract predictive information from financial text**.

---

# Research Question

The study addresses the following central question:

> **Can LLM-derived representations of earnings call transcripts predict future stock returns and generate risk-adjusted alpha in a systematic long–short portfolio strategy?**

---

# Dataset

The dataset consists of **earnings call transcripts from publicly traded U.S. companies** combined with corresponding equity price data.

### Key dataset characteristics

- ~14,000 earnings call events  
- ~2,200 unique companies  
- Coverage period: **2019 – 2023**  
- Multi-sector corporate universe  

For each transcript event:

1. The full transcript text is collected  
2. The transcript date is aligned with market data  
3. Forward stock returns are computed for evaluation  

### Forward return horizons

- 1-month returns  
- 3-month returns  

---

# Methodology Overview

The research pipeline consists of six main stages.

---

## 1. Data Cleaning and Preparation

Transcript text is cleaned to remove **boilerplate content and formatting artifacts**, including:

- operator introductions  
- prepared remarks headers  
- non-informative text patterns  

Stock return data is aligned with transcript release dates to ensure **no look-ahead bias**.

---

## 2. Transcript Chunking

Earnings call transcripts are typically long documents.

To allow transformer models to process the text efficiently:

- transcripts are split into smaller **text chunks**
- each chunk represents a manageable section of the transcript

This allows the embedding model to process the entire document without exceeding context limits.

---

## 3. LLM Embedding Generation

Each transcript chunk is converted into a **semantic embedding vector** using a transformer-based sentence embedding model.

Embeddings capture **contextual meaning and relationships between words**, going beyond simple keyword frequency.

Chunk-level embeddings are then **aggregated into a single transcript-level vector representation** for each earning call event.

---

## 4. Return Prediction Model

Transcript embeddings are used as input features for a **linear regression model** designed to estimate future stock returns.

The dataset is divided using a **time-based split**:

- Training period → historical observations  
- Test period → strictly **out-of-sample** observations  

This prevents **look-ahead bias** and ensures a realistic evaluation of predictive performance.

The model produces a **predicted return for each earnings call event**.

---

## 5. Portfolio Construction

Predicted returns are transformed into investment signals using **cross-sectional ranking**.

For each month:

- Stocks in the **top decile** of predicted returns are selected for the **long portfolio**
- Stocks in the **bottom decile** are selected for the **short portfolio**

A **long–short spread portfolio** is constructed and rebalanced monthly.

Transaction costs and signal lag are incorporated to simulate **realistic trading conditions**.

---

## 6. Strategy Evaluation

Portfolio performance is evaluated using standard **asset-pricing metrics**:

- Annualized Return  
- Annualized Volatility  
- Sharpe Ratio  
- Maximum Drawdown  

Additionally, **Information Coefficient (IC) analysis** measures the correlation between predicted rankings and realized returns.

---

# Benchmark Comparison

To evaluate whether **LLM embeddings provide incremental value**, the entire modeling pipeline is replicated using a **traditional TF-IDF text representation**.

This allows a direct comparison between:

- **Modern semantic embeddings**
- **Classical bag-of-words NLP models**

---

# Key Results

| Model | Annual Return | Volatility | Sharpe Ratio | Max Drawdown |
|------|------|------|------|------|
| **LLM Embeddings** | 34.1% | 23.0% | 1.48 | -7.8% |
| **TF-IDF Baseline** | 7.2% | 17.1% | 0.42 | -9.2% |

The results suggest that **LLM-derived embeddings capture richer information from financial text** compared to traditional NLP methods.

---

# Repository Structure

```
llm-alpha-earnings-calls/

data/
  raw/
  processed/

notebooks/
  01_data_cleaning.ipynb
  02_embedding_generation.ipynb
  03_return_prediction_model.ipynb
  04_portfolio_backtest.ipynb
  05_tfidf_benchmark.ipynb

results/
  figures/
  tables/

paper/
  LLM_Alpha_Earnings_Call_Research.pdf
```

---

# Technologies Used

- Python  
- PyTorch  
- HuggingFace Transformers  
- Sentence-Transformers  
- Scikit-learn  
- Pandas / NumPy  
- Matplotlib  

---

# Disclaimer

This project is intended for **academic research and educational purposes only**.

The results presented in this repository **do not constitute investment advice** and should not be interpreted as a production-ready trading strategy.
