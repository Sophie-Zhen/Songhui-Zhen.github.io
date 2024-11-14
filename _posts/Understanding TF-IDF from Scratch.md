---
layout: post
title: "Understanding TF-IDF from Scratch"
date: 2024-11-14
categories: posts
---

# Understanding TF-IDF from Scratch
_Sophie Zhen_ 14 Nov, 2024

In my work on a tweetQA system, I’ve been exploring text representation in Natural Language Processing (NLP). For tasks like information retrieval, it’s essential to evaluate the importance of specific terms and assign appropriate weights to each element in a document. This approach enables us to identify identical phrases and assess similarity between sentences and documents, even when there are differences in grammar or phrasing. In building a system like tweetQA, the fundamental task is to do text representation, and the challenge is to ensure that relevant information is captured even when expressions vary.
## Term Frequency (TF)
A simple and intuitive approach to assessing the importance of words is to count the frequency of each unique word in the documents we want to compare. For example, if we have a document containing 100 words and a corpus with 100 documents, we can count how often each word appears in each document. The more often a word appears in a specific document, the higher its term frequency (TF) score for that document. However, TF alone doesn’t capture the entire significance of a term, especially for distinguishing content across multiple documents.

To manage scale and avoid overly high counts, we typically apply log normalization, which smooths the values. This prevents overly frequent terms from dominating and helps moderate the weight distribution across terms. Here’s a breakdown of a few TF calculation methods:

1. __Raw Count:__ The raw count of a term t in document $d$.
2. __Binary (Boolean) TF:__ Binary TF is a simplified approach that considers only the presence (1) or absence (0) of a term in a document, regardless of how many times it appears. If a term occurs at least once in the document, set  $tf = 1$ ; if it doesn’t,  $tf = 0$ .
3. __Log Normalization:__ For terms occurring multiple times, the formula is: $\text{tf} = 1 + \log(f_{t,d})$, where  $f_{t,d}$  is the raw count of term  $t$  in document  $d$ .
## Inverse Document Frequency (IDF)
Counting term frequency alone isn’t enough. Common words like “the,” “is,” and “I” will naturally appear more often across documents, but they provide little meaningful information for differentiating between documents. To address this, we introduce __Inverse Document Frequency (IDF)__, which reduces the weight of common terms while increasing the weight of rare ones.

The typical formula for IDF is(where also applies log normalization):
$\text{idf} = \log_{10}\left(\frac{N}{df_t}\right)$

where:
- $N$ is the total number of documents
-  $df_t$  is the number of documents containing term  $t$ .


But this formula will cause issue if $df = 0$, so we often add 1 to the denominator in practice: $\text{idf} = \log_{10}\left(\frac{N}{df_t+1}\right)$


This calculation implies that the more documents a term appears in, the lower its IDF score, reflecting its lower specificity.
## TF-IDF: Combining TF and IDF
Now that we have two measurements—one that boosts high-frequency terms (TF) and one that reduces the weight of overly common terms (IDF)—we can combine them for a balanced approach. For each term in a document, we multiply TF and IDF to obtain the TF-IDF score. This creates a vector of TF-IDF scores for each document, where each score reflects the importance of a term in that specific document relative to the entire corpus.

$tf-idf_{t,d} = tf_{t,d} \times idf_t$

The resulting tf-idf score for a term represents its importance within the specific document in the context of the entire corpus.

This combination also creates a matrix representing each document as a vector, with each dimension corresponding to a unique term’s tf-idf score. The vectorized representations allow us to compare documents using similarity measures, such as cosine similarity, to assess how close or distinct documents are.

## Conclusion:
TF-IDF offers a balanced scoring method for terms based on their frequency and rarity, making it suitable for tasks like search engines, document clustering, and recommendation systems. By combining TF and IDF, we can reduce weights for generic terms and emphasize those that distinguish a document’s content.
- __Pros:__
  - **Simplicity and Efficiency:** TF-IDF is relatively simple to compute and easy to understand. Its calculations are straightforward, involving basic operations like term counting and logarithmic scaling.
  - **Effectiveness for Keyword-Based Scoring:** By down-weighting common terms and up-weighting rare ones, TF-IDF highlights keywords that are specific to a document, making it useful in search engines and for identifying important words in a document.
  - **Works Well with Sparse Data:** In many NLP tasks, data is often sparse (like documents with many unique words and few repetitions), and TF-IDF handles this effectively by ignoring terms that appear in all documents or by assigning them low importance.
  - **Widely Used Baseline:** It is a strong baseline in many NLP tasks, providing competitive performance for basic tasks like document retrieval, making it useful as a starting point for evaluating other models.
- __Cons:__
  - **Lack of Semantic Understanding:** TF-IDF doesn’t capture semantic meanings or relationships between words. Words with similar meanings but different spellings (e.g., “happy” and “joyful”) are treated as entirely separate terms. It also ignores synonyms and word context, meaning it can’t detect that “Apple” (the company) and “apple” (the fruit) are different without preprocessing. It also treats each word independently, missing out on context-based nuances like phrase meanings.
  - **Sensitivity to Vocabulary Size and Document Length:** While TF-IDF works well on smaller corpora, it struggles with scalability when applied to large datasets, particularly in real-time or interactive applications.
  - **Sparse and High-Dimensional Output:** While TF-IDF works well on smaller corpora, it struggles with scalability when applied to large datasets, particularly in real-time or interactive applications.

So I probably will not use it in my QA project, as it is limited understanding word semantics, context, and relationships. And tweet texts often have many slangs, abbreviations and other unstandardized expression, it will be better to choose more advanced techniques like word embeddings(unsupervised) (e.g., Word2Vec, GloVe) or contextual embeddings(supervised) (e.g., BERT) handle more effectively.
