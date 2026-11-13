# Lab 02: Web Scraping, Feature Engineering and Exploratory Data Analysis (EDA)

**Author:** Mohsin Raza  
**SAP ID:** 70177136  
**Email:** 70177136@student.uol.edu.pk  
**Institution:** The University of Lahore (UOL)  
**Course:** Introduction to Artificial Intelligence (AI-101L)  

---

## 📌 Overview
This repository contains the complete implementation of **Laboratory 02**, focusing on:
1. **NumPy Benchmarking:** Vectorized array operations vs native interpreted Python loops.
2. **Web Scraping:** Extracting unstructured article contents from online blog sources using `requests` and `BeautifulSoup`.
3. **Linguistic Feature Engineering:** Utilizing `spaCy` to compute tokens, sentences, parts of speech (nouns, verbs), and named entities.
4. **Sentiment Extraction:** Evaluating document polarity and subjectivity with `TextBlob`.
5. **Exploratory Data Analysis (EDA):** Visualizing feature distributions, pairwise correlation matrices, and TF-IDF terms.
6. **Two-Source Blog Comparison (Home Assignment):** Evaluating 20 articles from 2 distinct sources on sentiment polarity and noun density.

---

## 📁 Repository Structure
```
Lab02_Web_Scraping_and_EDA/
├── Lab_02_Web_Scraping_and_EDA.ipynb   # Complete, runnable Jupyter Notebook
├── articles_raw.csv                     # Raw cached scraped data
├── articles.csv                         # Engineered dataset with all linguistic features
├── two_source_corpus.csv                # 20-article dataset for home assignment
├── report.md                            # Comprehensive laboratory report
├── requirements.txt                     # Python package dependencies
├── figures/                             # Saved visualizations and figures
│   └── two_source_grouped_comparison.png
└── README.md                            # Project documentation
```

---

## 🚀 Getting Started

### 1. Prerequisites
Ensure Python 3.10+ or 3.11 is installed on your system.

### 2. Install Dependencies
```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

### 3. Run the Notebook
Open in Jupyter Lab or VS Code:
```bash
jupyter notebook Lab_02_Web_Scraping_and_EDA.ipynb
```
or run directly via CLI:
```bash
python -m jupyter nbconvert --to notebook --execute Lab_02_Web_Scraping_and_EDA.ipynb
```

---

## 📊 Summary of Findings
- **Vectorization Speedup:** NumPy vectorization executed **~95x faster** than native Python loops on 10 million elements.
- **Sentiment & Subjectivity:** Tech promotional articles averaged mildly positive polarity (+0.18) with moderate subjectivity (0.44), whereas academic science articles maintained objective neutrality (+0.048).
- **Stopwords impact:** Discarding stop words in TF-IDF highlighted domain-specific keywords (`ai`, `tools`, `content`, `video`) whereas keeping them led to dominant functional stop words (`the`, `and`, `to`).
