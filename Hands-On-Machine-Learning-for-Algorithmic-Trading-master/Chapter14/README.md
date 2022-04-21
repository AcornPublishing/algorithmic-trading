# Chapter 14: Topic Modeling

This chapter uses unsupervised machine learning to extract latent topics from documents. These themes can produce detailed insights into a large body of documents in an automated way. They are very useful to understand the haystack itself and permit the concise tagging of documents because using the degree of association of topics and documents. 

Topic models permit the extraction of sophisticated, interpretable text features that can be used in various ways to extract trading signals from large collections of documents. They speed up the review of documents, help identify and cluster similar documents, and can be annotated as a basis for predictive modeling. Applications include the identification of key themes in company disclosures or earnings call transcripts, customer reviews or contracts, annotated using, e.g., sentiment analysis or direct labeling with subsequent asset returns. More specifically, In this chapter, we will cover:

- What topic modeling achieves, why it matters and how it has evolved
- How Latent Semantic Indexing (LSI) reduces the dimensionality of the DTM
- How probabilistic Latent Semantic Analysis (pLSA) uses a generative model to extract topics
- How Latent Dirichlet Allocation (LDA) refines pLSA and why it is the most popular topic model
- How to visualize and evaluate topic modeling results
- How to implement LDA using sklearn and gensim
- How to apply topic modeling to collections of earnings calls and Yelp business reviews

## Learning latent topics: goals and approaches

Initial attempts by topic models to improve on the vector space model (developed in the mid-1970s) applied linear algebra to reduce the dimensionality of the document-term matrix. This approach is similar to the algorithm we discussed as principal component analysis in chapter 12 on unsupervised learning. While effective, it is difficult to evaluate the results of these models absent a benchmark model.

In response, probabilistic models emerged that assume an explicit document generation process and provide algorithms to reverse engineer this process and recover the underlying topics.

The below table highlights key milestones in the model evolution that we will address in more detail in the following sections.


| Model                                         | Year | Description                                                                                                   |
|-----------------------------------------------|------|---------------------------------------------------------------------------------------------------------------|
| Latent Semantic Indexing (LSI)                | 1988 | Capture semantic document-term relationship by reducing the dimensionality of the word space                  |
| Probabilistic Latent Semantic Analysis (pLSA) | 1999 | Reverse-engineers a generative a process that assumes words generate a topic and documents as a mix of topics |
| Latent Dirichlet Allocation (LDA)             | 2003 | Adds a generative process for documents: three-level hierarchical, Bayesian model                             |

## Latent semantic indexing (LSI)

Latent Semantic Analysis set out to improve the results of queries that omitted relevant documents containing synonyms of query terms. Its aimed to model the relationships between documents and terms to be able to predict that a term should be associated with a document, even though, because of variability in word use, no such association was observed.

LSI uses linear algebra to find a given number k of latent topics by decomposing the DTM. More specifically, it uses the Singular Value Decomposition (SVD) to find the best lower-rank DTM approximation using k singular values & vectors. In other words, LSI is an application of the unsupervised learning techniques of dimensionality reduction we encountered in chapter 12 (with some additional detail). The authors experimented with hierarchical clustering but found it too restrictive to explicitly model the document-topic and topic-term relationships or capture associations of documents or terms with several topics.

### How to implement LSI using sklearn

The notebook [latent_semantic_indexing](01_latent_semantic_indexing.ipynb) demonstrates how to apply LSI to the BBC new articles we used in the last chapter.

## Probabilistic Latent Semantic Analysis (pLSA)

Probabilistic Latent Semantic Analysis (pLSA) takes a statistical perspective on LSA and creates a generative model to address the lack of theoretical underpinnings of LSA. 

pLSA explicitly models the probability each co-occurrence of documents d and words w described by the DTM as a mixture of conditionally independent multinomial distributions that involve topics t. The number of topics is a hyperparameter chosen prior to training and is not learned from the data.

### How to implement pLSA using sklearn
 
The notebook [probabilistic_latent_analysis](02_probabilistic_latent_analysis.ipynb) demonstrates how to apply LSI to the BBC new articles we used in the last chapter.

- [Relation between PLSA and NMF and Implications](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.70.8839&rep=rep1&type=pdf), Gaussier, Goutte, 2005

## LDA:

Latent Dirichlet Allocation extends pLSA by adding a generative process for topics.
It is the most popular topic model because it tends to produce meaningful topics that humans, can relate to, can assign topics to new documents, and is extensible. Variants of LDA models can include metadata like authors, or include image data, or learn hierarchical topics.

LDA is a hierarchical Bayesian model that assumes topics are probability distributions over words, and documents are distributions over topics. More specifically, the model assumes that topics follow a sparse Dirichlet distribution, which implies that documents cover only a small set of topics, and topics use only a small set of words frequently. 

#### References

- [David Blei Homepage @ Columbia](http://www.cs.columbia.edu/~blei/)
- [Introductory Paper](http://www.cs.columbia.edu/~blei/papers/Blei2012.pdf) and [more technical review paper](http://www.cs.columbia.edu/~blei/papers/BleiLafferty2009.pdf)
- [Blei Lab @ GitHub](https://github.com/Blei-Lab)

### The Dirichlet Distribution
The Dirichlet distribution produces probability vectors that can be used with discrete distributions. That is, it randomly generates a given number of values that are positive and sum to one. It has a parameter 𝜶 of positive real value that controls the concentration of the probabilities.

The notebook [dirichlet_distribution](03_dirichlet_distribution.ipynb) contains a simulation so you can experiment with different parameter values.

### How to evaluate LDA topics

Unsupervised topic models do not provide a guarantee that the result will be meaningful or interpretable, and there is no objective metric to assess the result as in supervised learning. Human topic evaluation is considered the ‘gold standard’ but is potentially expensive and not readily available at scale.

Two options to evaluate results more objectively include perplexity that evaluates the model on unseen documents and topic coherence metrics that aim to evaluate the semantic quality of the uncovered patterns.

#### References

- [Exploring Topic Coherence over many models and many topics](https://www.aclweb.org/anthology/D/D12/D12-1087.pdf)
- [Paper on various Methods](http://www.aclweb.org/anthology/N10-1012)
- [Blog Post - Overview](http://qpleple.com/topic-coherence-to-evaluate-topic-models/)

### How to implement LDA using sklearn

The notebook [lda_with_sklearn](04_lda_with_sklearn.ipynb) shows how to apply LDA to the BBC news articles. We use [sklearn.decomposition.LatentDirichletAllocation](http://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html) to train an LDA model with five topics.

#### Code Examples

The notebook `lda_with_sklearn` contains the code examples used in this section.

### How to visualize LDA results using pyLDAvis

Topic visualization facilitates the evaluation of topic quality using human judgment. pyLDAvis is a python port of LDAvis, developed in R and D3.js. We will introduce the key concepts; each LDA implementation notebook contains examples.

pyLDAvis displays the global relationships among topics while also facilitating their semantic evaluation by inspecting the terms most closely associated with each individual topic and, inversely, the topics associated with each term. It also addresses the challenge that terms that are frequent in a corpus tend to dominate the multinomial distribution over words that define a topic. LDAVis introduces the relevance r of term w to topic t to produce a flexible ranking of key terms using a weight parameter 0<=ƛ<=1. 

- [Talk by the Author](https://speakerdeck.com/bmabey/visualizing-topic-models) and [Paper by (original) Author](http://www.aclweb.org/anthology/W14-3110)
- [Documentation](http://pyldavis.readthedocs.io/en/latest/index.html)

### How to implement LDA using gensim

Gensim is a specialized NLP library with a fast LDA implementation and many additional features. We will also use it in the next chapter on word vectors (see the notebook [lda_with_gensim](05_lda_with_gensim.ipynb) for details.

### Topic modeling for earnings calls

In Chapter 3 on [Alternative Data](../03_alternative_data/02_earnings_calls), we learned how to scrape earnings call data from the SeekingAlpha site. In this section, we will illustrate topic modeling on the harvested data. 

We're using a (small) sample of some 1,000 earnings call transcripts from the second half of 2018. For a practical application, a larger dataset would be highly desirable. 

The directory [earnings_calls](06_earnings_calls) contains several files with examples mentioned below. See the notebook [lda_earnings_calls](06_earnings_calls/lda_earnings_calls.ipynb) for details on loading, exploring, and preprocessing the data, as well as training and evaluating individual models, and the [run_experiments.py](06_earnings_calls/run_experiments.py)) file for the experiments evaluated in the notebook.

### Topic modeling for Yelp business reviews

The notebook [lda_yelp_reviews](07_yelp/lda_yelp_reviews.ipynb) contains an example of LDA applied to six million business review on yelp. Reviews are a more uniform in length than the statements extracted from the earnings call transcript. After cleaning as above, the 10th and 90th percentile range from 14 to 90 tokens.

## Applications

- [Applications of Topic Models](https://mimno.infosci.cornell.edu/papers/2017_fntir_tm_applications.pdf), Jordan Boyd-Graber, Yuening Hu, David Minmo, 2017

- [High Quality Topic Extraction from Business News Explains Abnormal Financial Market Volatility](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3675119/pdf/pone.0064846.pdf)
- [What are You Saying? Using Topic to Detect Financial Misreporting](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2803733)

- [LDA in the browser - javascript implementation](https://github.com/mimno/jsLDA)
- [David Mimno @ Cornell University](https://mimno.infosci.cornell.edu/)


## Topic Modeling Software
- [David Blei's List of Open Source Topic Modeling Software](http://www.cs.columbia.edu/~blei/topicmodeling_software.html)

- [Mallet (MAchine Learning for LanguagE Toolkit (in Java)](http://mallet.cs.umass.edu/)
