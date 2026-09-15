# A Practical Guide to Measuring Text Similarity

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-Ready-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![NLP](https://img.shields.io/badge/NLP-Text%20Similarity-purple.svg)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A hands-on look at how text can be compared by words, structure, and meaning, and how NLP techniques can be used to evaluate variation in AI-generated answers.

This repository contains the Python notebook developed for my **Marketing Data Science** article, **A Practical Guide to Measuring Text Similarity**.

## Overview

Two pieces of text can look very different while expressing essentially the same idea. They can also look nearly identical while making substantially different claims.

That makes text similarity more complicated than simply asking whether two strings contain the same words.

This notebook explores several ways to measure similarity:

1. Exact string matching
2. Levenshtein similarity
3. Jaccard similarity
4. TF-IDF with cosine similarity
5. Sentence embeddings with cosine similarity

The notebook begins with controlled examples and then applies the same techniques to multiple AI-generated answers to the same marketing question.

## Why Text Similarity Matters

Text comparison has many practical applications in marketing, analytics, and AI, including:

- comparing AI-generated answers to the same prompt
- identifying duplicate or near-duplicate customer reviews
- comparing versions of marketing copy
- analyzing survey responses
- grouping similar support requests
- comparing competitor messaging
- detecting changes in documents or product descriptions
- measuring consistency across repeated AI API queries

A central theme of the notebook is that there is no single definition of "similar."

Different methods measure different things.

## Methods Covered

### Exact Match

The simplest comparison asks whether two strings are completely identical.

This is useful for duplicate detection and reproducibility, but even a one-character difference causes the texts to be treated as different.

### Levenshtein Similarity

Levenshtein distance measures the number of character insertions, deletions, or substitutions required to transform one string into another.

A normalized version allows the result to be expressed as a similarity score between 0 and 1.

### Jaccard Similarity

Jaccard similarity compares the overlap between sets of words:

\[
J(A,B) = \frac{|A \cap B|}{|A \cup B|}
\]

It is simple and intuitive, but it ignores word order, frequency, and context.

### TF-IDF with Cosine Similarity

TF-IDF gives more weight to terms that provide useful information within a collection of documents.

Once text is converted into TF-IDF vectors, cosine similarity measures the angle between those vectors.

This provides a more sophisticated measure of lexical similarity than simple word overlap.

### Sentence Embeddings with Cosine Similarity

Sentence embeddings convert text into numerical vectors designed to capture patterns associated with semantic meaning.

This notebook uses:

`sentence-transformers/all-MiniLM-L6-v2`

Embedding-based comparisons can recognize paraphrases and semantically related sentences even when they share relatively few words.

## Controlled Examples

The notebook uses several deliberately constructed examples to demonstrate where similarity measures succeed and where they can be misleading.

Examples include:

- exact duplicates
- different wording with similar meaning
- nearly identical wording with opposite meaning
- related concepts expressed with different vocabulary
- nearly identical text containing materially different numbers
- unrelated text

Two examples are particularly important.

### The Contradiction Trap

Consider:

> The campaign increased conversions by 15 percent.

and:

> The campaign decreased conversions by 15 percent.

The sentences are almost identical, but one word reverses the conclusion.

Despite that difference, the notebook produces relatively high similarity scores:

| Method | Similarity |
|---|---:|
| Levenshtein | 0.959 |
| Jaccard | 0.750 |
| TF-IDF + Cosine | 0.752 |
| Embeddings + Cosine | 0.830 |

### The Numbers Trap

Consider:

> Revenue increased by 10 percent last quarter.

and:

> Revenue increased by 100 percent last quarter.

Only one character changes, but the second result is ten times larger.

The similarity scores remain high:

| Method | Similarity |
|---|---:|
| Levenshtein | 0.978 |
| Jaccard | 0.750 |
| TF-IDF + Cosine | 0.752 |
| Embeddings + Cosine | 0.841 |

These examples highlight an important principle:

> **Similarity is not the same thing as factual agreement, logical agreement, numerical equivalence, or correctness.**

## Comparing AI-Generated Answers

The notebook also compares eight answers to the same prompt:

> **What is marketing attribution, and why does it matter?**

The answers vary in wording, structure, detail, and emphasis.

One answer deliberately includes an overly strong claim about last-click attribution to demonstrate that a response can still be semantically similar to other answers while containing a questionable or incorrect assertion.

The notebook compares the responses using both lexical and semantic methods.

Across all unique AI-response pairs, the average similarities were:

| Method | Mean Similarity |
|---|---:|
| Levenshtein | 0.309 |
| Jaccard | 0.154 |
| TF-IDF + Cosine | 0.228 |
| Embeddings + Cosine | 0.839 |

The wording varies substantially, while the overall semantic content remains much more consistent.

## Pairwise Similarity

The notebook creates an embedding for each AI response and calculates a full pairwise cosine-similarity matrix.

This makes it possible to identify:

- the most similar responses
- the least similar responses
- outliers
- clusters
- the response most representative of the group

In the example dataset:

- **Most similar pair:** Answer 1 and Answer 7, similarity = `0.958`
- **Least similar pair:** Answer 6 and Answer 8, similarity = `0.718`
- **Most representative response:** Answer 1, average similarity = `0.881`

Being representative does not mean an answer is necessarily the most accurate or highest quality. It only means it is most similar, on average, to the other answers.

## Scaling to Larger AI Experiments

The same approach can be extended to repeated API queries.

For `n` responses, the number of unique response pairs is:

\[
\frac{n(n-1)}{2}
\]

For example:

| Responses | Unique Comparisons |
|---:|---:|
| 2 | 1 |
| 10 | 45 |
| 100 | 4,950 |
| 1,000 | 499,500 |

At that scale, automated similarity analysis becomes much more useful than manually reading every response pair.

Future experiments could compare:

- repeated runs of the same prompt
- different AI models
- different model settings
- changes in temperature
- prompt variations
- response consistency over time
- factual consistency
- numerical consistency
- citation or source consistency
- semantic outliers

## Notebook

The primary notebook is:

`practical_guide_to_measuring_text_similarity.ipynb`

The notebook is designed to run in **Google Colab** using a standard CPU runtime.

### Open in Google Colab

After uploading this repository to GitHub, you can open the notebook directly in Colab using a URL in this format:

```text
https://colab.research.google.com/github/joedom99/text-similarity-guide/blob/main/practical_guide_to_measuring_text_similarity.ipynb
```

## Requirements

The notebook uses the following Python packages:

- `numpy`
- `pandas`
- `matplotlib`
- `scikit-learn`
- `rapidfuzz`
- `sentence-transformers`

The notebook installs the additional packages it needs when run in Google Colab.

A GPU is not required for the examples in this notebook.

## Important Limitation

Text similarity should not be treated as a truth detector.

A high similarity score does not necessarily mean two texts:

- agree
- contain the same facts
- use the same numbers
- reach the same conclusion
- are equally accurate
- are equally useful

For AI evaluation, text similarity is best treated as one measurement within a broader evaluation framework.

## Related Reading

This project builds on concepts I previously discussed in:

**A Marketer's Guide to NLP: How Machines Actually Process and Understand Language**  
https://blog.marketingdatascience.ai/a-marketers-guide-to-nlp-how-machines-actually-process-and-understand-language-3d452febb3de

**Sentiment Analysis of Online Reviews Using R**  
https://blog.marketingdatascience.ai/sentiment-analysis-of-online-reviews-using-r-e2afbc9fcc68

Additional references and resources are included inside the notebook.

## Author

**Joe Domaleski**

Marketing Data Science  
https://blog.marketingdatascience.ai/

Country Fried Creative  
https://countryfriedcreative.com/

GitHub  
https://github.com/joedom99

I write about practical applications of marketing, analytics, data science, machine learning, and artificial intelligence.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

**Technology is a tool. People are the mission.**
