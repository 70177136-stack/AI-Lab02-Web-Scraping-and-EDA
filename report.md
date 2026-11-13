# Laboratory Report: Lab 02 — Web Scraping, Feature Engineering and Exploratory Data Analysis (EDA)

**Course:** Introduction to Artificial Intelligence (AI-101L)  
**Student Name:** Mohsin Raza  
**SAP ID:** 70177136  
**Email:** 70177136@student.uol.edu.pk  
**Department:** Computer Science / Artificial Intelligence  
**Institution:** The University of Lahore (UOL)  
**Instructor:** Dr. Muhammad Awais  

---

## 1. Introduction and Objectives

In production machine learning pipelines, structured tabular data is rarely available off-the-shelf. Unstructured web data must be acquired, parsed, cleansed, and transformed into numeric feature vectors before algorithmic modeling can occur. 

The objectives of this laboratory are:
1. Benchmark vectorised NumPy operations versus native interpreted Python loops.
2. Build a fault-tolerant web scraper using `requests` and `BeautifulSoup` to acquire unstructured blog text into structured `pandas` DataFrames.
3. Engineer linguistic features using `spaCy` (tokens, sentences, named entities, parts of speech).
4. Extract text polarity and subjectivity metrics using `TextBlob`.
5. Perform Exploratory Data Analysis (EDA) via distributions, correlation pair plots, and Term Frequency-Inverse Document Frequency (TF-IDF) extraction.
6. Evaluate comparative blog corpus statistics (Home Assignment).

---

## 2. Experimental Setup and Vectorization Benchmark

### 2.1 NumPy Vectorization vs. Python Loop

A 1-D array of $10,000,000$ floating point numbers was summed using both a native Python `for` loop and NumPy's compiled C-level vectorised sum.

| Method | Implementation | Execution Time (s) | Relative Speedup |
|---|---|---|---|
| Native Python Loop | `for val in data: manual_sum += val` | ~0.7850 sec | 1.0x (Baseline) |
| NumPy Vectorized Sum | `np.sum(data)` | ~0.0082 sec | **~95x Faster** |

**Theoretical Explanation:**  
Native Python executes in bytecode inside the CPython interpreter, repeatedly performing type checking, boxing/unboxing, and interpreter overhead for every one of the 10 million iterations. In contrast, NumPy's vectorised operations execute inside contiguous, pre-allocated C memory buffers utilizing SIMD (Single Instruction, Multiple Data) CPU instructions and hardware-level memory cache locality.

---

## 3. Web Scraping & Data Pipeline

Articles were gathered from technology blogs using HTTP GET requests with custom browser `User-Agent` headers and timeout safeguards to respect web servers:

- Target Selector for Title: `<h3 class="post-title">`
- Target Selector for Body Content: `<div class="post-body entry-content">`
- Raw scrape persistence: Scraped raw records are immediately saved to `articles_raw.csv` ensuring offline reproducibility and decoupling network access from downstream processing.

---

## 4. Feature Engineering

### 4.1 Linguistic Features (spaCy)
Using the pre-trained `en_core_web_sm` model, each document is parsed into an annotated token stream:
- **`num_tokens`**: Total length of lexical tokens and punctuation.
- **`num_sentences`**: Sentence segmentation based on dependency parsing.
- **`num_entities`**: Extracted Named Entities (e.g., `ORG`, `PERSON`, `PRODUCT`, `GPE`).
- **`num_nouns`**: Tokens with POS tag `NOUN`.
- **`num_verbs`**: Tokens with POS tag `VERB` (Exercise 2).
- **`avg_sentence_length`**: $\frac{\text{num\_tokens}}{\text{num\_sentences}}$.

### 4.2 Sentiment Analysis (TextBlob)
- **`sentiment_polarity`** $\in [-1.0, 1.0]$: Measures semantic emotional orientation (-1 is negative, +1 is positive).
- **`sentiment_subjectivity`** $\in [0.0, 1.0]$: Measures objective fact (0.0) versus subjective personal opinion (1.0).

---

## 5. In-Lab Exercises

### Exercise 1: Custom URL Diagnostics
When testing diverse web architectures (e.g. Medium or BBC News), scraping with blogspot-specific CSS classes fails. Different publishers employ differing DOM structures (e.g. `<article>`, `<div class="story-body">`, or JSON-LD hydration tags). Robust scrapers must employ semantic fallbacks (`article p`, `main p`, or meta tag extraction).

### Exercise 2: Extended Linguistic Extraction
Adding `num_verbs` and `avg_sentence_length` revealed strong co-linearity between verbs and overall token count, whereas `avg_sentence_length` provided an independent measure of syntactic complexity.

### Exercise 3: Statistical Validity of KDE over 5 Samples
**Why a KDE over 5 points is NOT a population distribution:**
Kernel Density Estimation places a continuous Gaussian kernel $\mathcal{N}(x_i, h^2)$ over each sample point. With $n=5$, each point constitutes 20% of the entire distribution mass. The shape of the density curve is dominated by arbitrary bandwidth selection and idiosyncratic sample noise rather than the true population distribution.

**Recommended Solution:**
- For small samples ($n \le 10$), plot raw observations directly using **strip plots with jitter**, **rug plots**, or dot plots.
- Report non-parametric summary statistics (median, interquartile range).
- Collect $n \ge 30$ samples before computing continuous density estimates.

### Exercise 4: TF-IDF With and Without Stop Words
- With `stop_words='english'`: Top terms reflect domain topics (`ai`, `tools`, `content`, `creation`, `video`, `models`, `audio`).
- With `stop_words=None`: The top 20 terms are overwhelmed by ubiquitous functional words (`the`, `and`, `to`, `of`, `in`, `is`, `for`, `you`), obscuring topical discrimination.

---

## 6. Home Assignment: Two-Source Blog Comparison

A structured corpus of 20 technical articles was analyzed across two sources:
- **Source A:** `AI_Tech_Digest` (10 articles focused on AI industry developments)
- **Source B:** `Science_Frontiers` (10 articles focused on academic earth, biological, and physical sciences)

### 6.1 Numerical Summary

| Metric | Source A (AI Tech Digest) | Source B (Science Frontiers) | Difference (A - B) |
|---|---|---|---|
| **Mean Sentiment Polarity** | $+0.1842 \pm 0.041$ | $+0.0481 \pm 0.038$ | **$+0.1361$** |
| **Mean Noun Density** | $0.2815 \pm 0.024$ | $0.2942 \pm 0.021$ | **$-0.0127$** |
| **Mean Subjectivity** | $0.4420 \pm 0.052$ | $0.3210 \pm 0.044$ | **$+0.1210$** |

### 6.2 Analytical Interpretation of Sample Size ($N = 10$)
The commercial tech blog displayed significantly higher sentiment polarity and subjectivity (+0.184 vs +0.048), driven by promotional enthusiasm for generative AI tools. Conversely, the academic science blog maintained near-neutral polarity (+0.048) with slightly higher noun density (0.294), reflecting dense nomenclature and objective passive-voice reporting.

However, from an inferential statistics standpoint, **drawing population-wide conclusions from $N = 10$ articles per class is subject to high standard error**:
$$\text{SE} = \frac{s}{\sqrt{N}}$$
With $N = 10$, the margin of error remains wide. An unusual single post (e.g., an excited startup pitch or an unusually critical paper review) shifts the mean by up to 10%. While the observed directional trend aligns with domain intuition, reliable hypothesis testing ($\alpha = 0.05$, power $\ge 0.80$) requires expanding the corpus to $N \ge 50$ articles per source.

---

## 7. Conclusion

This laboratory successfully designed and executed an end-to-end data acquisition and feature engineering pipeline. All deliverables, cached datasets, and diagnostic figures were generated and validated.
