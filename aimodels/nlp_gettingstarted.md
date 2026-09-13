### Text preprocessing
- parsing: Breaking up text based on grammar
- tokenization: Breaks text into list of words
- stopwords removal:
- lemmatization:

### Definitions
- Language models: probabilistic computer models of language, to figure out the likelihood a given sound/letter/word/phrase will be used.
- **Unigram model "bag-of-words"** --> simply has tally count of each instance of words, performed using `Counter()`
- **n-gram model** --> considers sequence of n units, calculating probability of each unit in a body of language given preceding sequence of length n
  - language smoothing can adjust probabilities for unknown words not encountered in training
- **Neural language modeling (NLM)** --> e.g. Long Short Term Memory (LSTM) models, transformer models
<br>

- **Topic modeling** -->used to uncover latent, or hidden topics within body of text
  - **term frequency-inverse document frequency (tf-idf)** model --> words which occur less frequently should be topic
    - latent Dirichlet allocation (LDA) --> model to determine which words occur frequently (ie. usually filter for stop words before LDA, or use tf-idf model before LDA)
- **Text similarity**
  - Levenshtein distance --> minimal edit distance between two words e.g. bees to beans (1 substitution + 1 insertion) = 2
      - ```py
        def is_plagiarized(text1, text2):
        n = 7
        if edit_distance(text1.lower(), text2.lower()) > ((len(text1) + len(text2)) / n):
          return False
        return True
        ```
  - Lexical similarity is the degree to which texts use similar vocabulary
  - Semantic similarity is the degree to which texts contain similar meaning/topics
  - Phonetic similarity is the degree to which two words/phrases sound alike
- **Language prediction**
  - Markov chains (in n-gram model) --> predict statistical likelihood of each following word/character based on training corpus
<br>

- **Naive Bayes classifiers**: Sup ML also which leverage probabilistic theorems to make predictions and classifications


### Packages
- `nltk` as Py lib for NLP
- `gensim` and `sklearn` for tf-idf
- `word2vec` for viz topic models spatially as vectors, so more commonly used = closer together
