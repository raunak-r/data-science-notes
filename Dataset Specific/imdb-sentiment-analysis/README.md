## Accuracy Conclusions
1. CountVector - 84.02%
2. TfIDf - 85.79% 
3. Deep Learning (using Single-Label Binary Classification) - 88.3%

## Data Preprocessing

Algorithm Summary:
```
Size - 25000 x 3

1. Columns - ['id', 'labels', 'text']
                      
2. Imputation -
        id        0
        labels    0
        text      0
        
3. Tokenization
    from nltk.tokenize import word_tokenize, RegexpTokenizer
    from nltk.corpus import stopwords
    from nltk import stem

        '''
        Split the para.
        Get words.
        convert to lower case
        lemmatize them.
        join them back.
        Now the paragraph is clean.
        '''
4. train.text = [clean_paragraph(para) for para in train.text]
    Time taken - 48 seconds for 17500 rows.
    RegexpTokenizer took the least time.
    
        tokenizer = RegexpTokenizer(r'\w+')
        p = ' '.join([lmtzr.lemmatize(token.lower()) for token in tokenizer.tokenize(para)])
```

## Count Vectorizer
```
import sklearn, nltk
from sklearn.feature_extraction.text import CountVectorizer

import sklearn, nltk
from sklearn.feature_extraction.text import CountVectorizer

17500 docs, 34325 unique tokens

Accuracy
    CountVector - 84.02%
```

## Count Vector written from Scratch
```
1. Clean Paragraph
2. Create classes and assign probs
    0    8764
    1    8736
    
    Name: labels, dtype: int64
    0    0.5008
    1    0.4992
3. create word matrix
    58535 unique tokens
4. Initialize Matrix
    3x58535 sparse matrix of type '<class 'numpy.float64'>'
5. Fill Matrix


6. Clean Test Paragraph and predict target_class
   ```
       for each row in test:
           initialize pred_class_prob = 0
           
           for each target_class:
               for each token in text data:
                   prob_value = get count of word from matrix for that target_class / get total count of that word across all labels.
                   product = prob_value * prob_of_target_class
                   
                update pred_class_prob_value and pred_class
   ```
```
## TfIdf Vector
```
1. from sklearn.feature_extraction.text import TfidfVectorizer, TfidfTransformer
tfidf = TfidfVectorizer()
training_set_counts = tfidf.fit_transform(train.text)

```
The count vectorizer will extract all unique tokens and makes a matrix of 17500 rows and 58531 unique tokens.
Each row is the vector representation of that document.
```

2. Accuracy
from sklearn.naive_bayes import MultinomialNB
    TfIDf - 85.79% 
```

## Deep Learning

```
from sklearn.feature_extraction.text import CountVectorizer
vectorizer = CountVectorizer(min_df=2, tokenizer=nltk.word_tokenize)
training_vectorized_data = vectorizer.fit_transform(train.text).toarray()
    17500 docs, 34325 unique tokens
Test Data - (7500, 34325)


Model
    ```
    network = models.Sequential()
    network.add(layers.Dense(16, activation='relu', input_shape=(34325,)))
    network.add(layers.Dense(16, activation='relu'))
    network.add(layers.Dense(1, activation='sigmoid'))

    network.compile(optimizer = 'rmsprop',
                 loss = 'binary_crossentropy',
                 metrics = ['accuracy'])

    history = network.fit(training_vectorized_data, train.labels, epochs=4, batch_size=512)

    test_loss, test_acc = network.evaluate(test_vectorized_data, test.labels)
    test_accuracy:  0.8869333333333334
    ```
```