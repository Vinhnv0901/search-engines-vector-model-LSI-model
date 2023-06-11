<h1 align="center">search-engines-vector-model-LSI-model</h1>

# I. dataset
* Cranfield data is widely used in research and experimentation in the fields of artificial intelligence and information search. This data includes a collection of scientific texts, technical reports, conference papers and other references. In total, the Cranfield data includes approximately 1,400 documents from various fields such as computer science, physics, biology, chemistry and engineering.  
* We also have 225 queries along with 225 result files of the corresponding queries. From there we can evaluate the query model.  

# II. model
* We choose 2 models to test the query with Cranfield documents: vector model and LSI model (Latent Semantic Indexing).  
  
## 1. Vector model
* In the vector model, documents and queries are represented as numeric vectors. In this model we use TF-IDF (Term Frequency-Inverse Document Frequency). This method calculates a weight value for each word in the document based on the frequency of occurrence of the word in the document and the entire document collection. We use **TfidfVectorizer** from **sklearn** to build model.   
* Here we use the cosine formula to calculate the similarity of two vectors. We also use cosine_similarity of **sklearn** to compute similarity.  
![cosine](/image/cosine.png)  
The formula for weighting in this model is:  
$TF-IDF(t,d) = \frac{TF(t,d)*IDF(t)}{norm(d)}$

# III. data processing.
* Because we don't know how to determine the appropriate term for the model, we decided to experiment with preprocessors such as removing special characters and numbers, removing stopwords, and stemming.
## 1. stopwords.
* stopword is a term for words that are common but carry little or no special meaning in semantic analysis or text classification. For example, the words “a”, “an”, “the”, “and”…  
* We use the stopwords from the nltk library.
## 2. stemming.
* Stemming is a method of converting a word back to its original or basic form called "stem". For example, when applying stemming to the words "running", "runs", "ran", "runner", the results will be converted to the original form "run".
* We use the Porter's Stermmer method of the nltk library.
## 3. posting list.
* After preprocessing and putting into the vector model, we will have a posting list like that:  
  ![posting list](/image/posting.png)  
on the left is the term and on the right is the array containing the files where those terms appear.

# IV. Evaluation.
* To evaluate the model we use Recall, Precision and MAP (Mean Average Precision) with 11 interpolation points from 0 to 1.  
## 1. vector model.
* In the test with defining term we will have the following cases: remove characters (1), remove characters and numbers (2), handle stemming(3), remove stopwords(4).

|  | (1) | (2) | (2) (3) | (2) (4) | (1) (3) | (1) (4) | (1) (3) (4) | (2) (3) (4) |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| MAP | 0.32 | 0.33 | 0.15 | 0.327 | 0.13 | 0.31 | 0.14 | 0.16 |
| Recall | 0.47 | 0.51 | 0.3 | 0.45 | 0.22 | 0.4 | 0.24 | 0.33 |
| Precision | 0.22 | 0.19 | 0.12 | 0.24 | 0.14 | 0.29 | 0.12 | 0.1 |
| Time | 2s | 2s | 2s | 3s | 3s | 2s | 3s | 3s |
  
* From the above results, we can see that Porter's stemmer treatment is not suitable because in cases where (3) stemming occurs, the rating scales are low. Removing digit values and not removing doesn't affect the results too much either, but it seems to remove numbers slightly better. And removing stopwords has not changed much.
