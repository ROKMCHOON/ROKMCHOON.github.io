### 정수 인코딩(Integer Encoding)

#### 1) dictionary 사용하기


```python
from nltk.tokenize import sent_tokenize
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
```


```python
raw_text = "A barber is a person. a barber is good person. a barber is huge person. he Knew A Secret! The Secret He Kept is huge secret. Huge secret. His barber kept his word. a barber kept his word. His barber kept his secret. But keeping and keeping such a huge secret to himself was driving the barber crazy. the barber went up a huge mountain."
```


```python
# 문장 토큰화
sentences = sent_tokenize(raw_text)
sentences
```




    ['A barber is a person.',
     'a barber is good person.',
     'a barber is huge person.',
     'he Knew A Secret!',
     'The Secret He Kept is huge secret.',
     'Huge secret.',
     'His barber kept his word.',
     'a barber kept his word.',
     'His barber kept his secret.',
     'But keeping and keeping such a huge secret to himself was driving the barber crazy.',
     'the barber went up a huge mountain.']




```python
vocab = {}
preprocessed_sentence = []
stop_words = set(stopwords.words('english'))

for sentence in sentences:
    # 단어 토큰화
    tokenized_sentence = word_tokenize(sentence)
    result = []
    
    for word in tokenized_sentence:
        word = word.lower() # 모든 단어를 소문자화하여 단어의 개수를 줄인다.
        if word not in stop_words: # 단어 토큰화가 된 결과에 대해서 불용어를 제거한다.
            if len(word) > 2: # 단어의 길이가 2이하인 경우에 대해서 추가로 단어를 제거한다.
                result.append(word)
                if word not in vocab:
                    vocab[word] = 0
                vocab[word] += 1
    preprocessed_sentence.append(result)
print(preprocessed_sentence)
            
    
```

    [['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]
    


```python
print('단어집합 :', vocab)
```

    단어집합 : {'barber': 8, 'person': 3, 'good': 1, 'huge': 5, 'knew': 1, 'secret': 6, 'kept': 4, 'word': 2, 'keeping': 2, 'driving': 1, 'crazy': 1, 'went': 1, 'mountain': 1}
    


```python
print(vocab['barber'])
```

    8
    


```python
print(vocab.items())
```

    dict_items([('barber', 8), ('person', 3), ('good', 1), ('huge', 5), ('knew', 1), ('secret', 6), ('kept', 4), ('word', 2), ('keeping', 2), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)])
    


```python
vocab_sorted = sorted(vocab.items(), key = lambda x: x[1], reverse=True)
print(vocab_sorted)
```

    [('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3), ('word', 2), ('keeping', 2), ('good', 1), ('knew', 1), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)]
    


```python
word_to_index = {}
i = 0
for (word, frequency) in vocab_sorted:
    if frequency > 1:
        i = i + 1
        word_to_index[word] = i
        
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7}
    


```python
vocab_size = 5

# 인덱스가 5 초과인 단어 제거
word_frequency = [word for word, index in word_to_index.items() if index >= vocab_size + 1]

# 해당 단어에 대한 인덱스 정보를 삭제
for w in word_frequency:
    del word_to_index[w]
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}
    


```python
word_to_index['OOV'] = len(word_to_index) + 1
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'OOV': 6}
    


```python
encoded_sentences = []
for sentence in preprocessed_sentence:
    encoded_sentence = []
    for word in sentence:
        try:
            # 단어 집합에 있는 단어라면 해당 단어의 정수를 리턴.
            encoded_sentence.append(word_to_index[word])
        except KeyError:
            # 단어 집합에 없는 단어라면 'OOV'의 정수를 리턴
            encoded_sentence.append(word_to_index['OOV'])
    encoded_sentences.append(encoded_sentence)
print(encoded_sentences)
```

    [[1, 5], [1, 6, 5], [1, 3, 5], [6, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [6, 6, 3, 2, 6, 1, 6], [1, 6, 3, 6]]
    

#### 2) Counter 사용하기


```python
from collections import Counter
```


```python
# words = np.hstack(preprocessed_sentence)으로도 수행 가능.
all_words_list = sum(preprocessed_sentence, [])
print(all_words_list)
```

    ['barber', 'person', 'barber', 'good', 'person', 'barber', 'huge', 'person', 'knew', 'secret', 'secret', 'kept', 'huge', 'secret', 'huge', 'secret', 'barber', 'kept', 'word', 'barber', 'kept', 'word', 'barber', 'kept', 'secret', 'keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy', 'barber', 'went', 'huge', 'mountain']
    


```python
# 파이썬의 Counter 모듈을 이용하여 단어의 빈도수 카운트
vocab = Counter(all_words_list)
vocab
```




    Counter({'barber': 8,
             'person': 3,
             'good': 1,
             'huge': 5,
             'knew': 1,
             'secret': 6,
             'kept': 4,
             'word': 2,
             'keeping': 2,
             'driving': 1,
             'crazy': 1,
             'went': 1,
             'mountain': 1})




```python
vocab['barber']
```




    8




```python
vocab_size = 5
vocab = vocab.most_common(vocab_size)
vocab
```




    [('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3)]




```python
word_to_index = {}
i = 0
for (word,frequency) in vocab:
    i = i+1
    word_to_index[word] = i
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}
    

#### 3) NLTK의 FreqDist 사용하기


```python
from nltk import FreqDist
import numpy as np
```


```python
# np.hstack으로 문장 구분을 제거
vocab = FreqDist(np.hstack(preprocessed_sentence))
vocab
```




    FreqDist({'barber': 8, 'secret': 6, 'huge': 5, 'kept': 4, 'person': 3, 'word': 2, 'keeping': 2, 'good': 1, 'knew': 1, 'driving': 1, ...})




```python
print(vocab['barber'])
```

    8
    


```python
vocab_size = 5
vocab = vocab.most_common(vocab_size)
vocab
```




    [('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3)]




```python
word_to_index = {word[0] : index + 1 for index, word in enumerate(vocab)}
word_to_index
```




    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}




```python
test_input = ['a','b','c','d','e']
for index, value in enumerate(test_input):
    print('value : {} , index : {}'.format(value, index))
```

    value : a , index : 0
    value : b , index : 1
    value : c , index : 2
    value : d , index : 3
    value : e , index : 4
    

### 2. 케라스(Keras)의 텍스트 전처리


```python
from tensorflow.keras.preprocessing.text import Tokenizer
```


```python
preprocessed_sentence
```




    [['barber', 'person'],
     ['barber', 'good', 'person'],
     ['barber', 'huge', 'person'],
     ['knew', 'secret'],
     ['secret', 'kept', 'huge', 'secret'],
     ['huge', 'secret'],
     ['barber', 'kept', 'word'],
     ['barber', 'kept', 'word'],
     ['barber', 'kept', 'secret'],
     ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'],
     ['barber', 'went', 'huge', 'mountain']]




```python
tokenizer = Tokenizer()

# fit_on_texts() 안에 코퍼스를 입력하면 빈도수를 기준으로 단어 집합을 생성.
tokenizer.fit_on_texts(preprocessed_sentence)
```


```python
print(tokenizer.word_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7, 'good': 8, 'knew': 9, 'driving': 10, 'crazy': 11, 'went': 12, 'mountain': 13}
    


```python
print(tokenizer.word_counts)
```

    OrderedDict([('barber', 8), ('person', 3), ('good', 1), ('huge', 5), ('knew', 1), ('secret', 6), ('kept', 4), ('word', 2), ('keeping', 2), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)])
    


```python
print(tokenizer.texts_to_sequences(preprocessed_sentence))
```

    [[1, 5], [1, 8, 5], [1, 3, 5], [9, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [7, 7, 3, 2, 10, 1, 11], [1, 12, 3, 13]]
    


```python
vocab_size = 5
tokenizer = Tokenizer(num_words= vocab_size + 1)
tokenizer.fit_on_texts(preprocessed_sentence)
```


```python
print(tokenizer.word_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7, 'good': 8, 'knew': 9, 'driving': 10, 'crazy': 11, 'went': 12, 'mountain': 13}
    


```python
print(tokenizer.texts_to_sequences(preprocessed_sentence))
```

    [[1, 5], [1, 5], [1, 3, 5], [2], [2, 4, 3, 2], [3, 2], [1, 4], [1, 4], [1, 4, 2], [3, 2, 1], [1, 3]]
    


```python
# 숫자 0과 OOV를 고려해서 집합의 크기는 +2
tokenizer = Tokenizer(num_words= vocab_size + 2, oov_token= 'OOV')
tokenizer.fit_on_texts(preprocessed_sentence)
```


```python
print('단어 OOV의 인덱스 : {}'.format(tokenizer.word_index['OOV']))
```

    단어 OOV의 인덱스 : 1
    


```python
print(tokenizer.texts_to_sequences(preprocessed_sentence))
```

    [[2, 6], [2, 1, 6], [2, 4, 6], [1, 3], [3, 5, 4, 3], [4, 3], [2, 5, 1], [2, 5, 1], [2, 5, 3], [1, 1, 4, 3, 1, 2, 1], [2, 1, 4, 1]]
    

### 패딩(Padding)

#### 1. Numpy로 패딩하기


```python
import numpy as np
from tensorflow.keras.preprocessing.text import Tokenizer 
```


```python
preprocessed_sentence
```




    [['barber', 'person'],
     ['barber', 'good', 'person'],
     ['barber', 'huge', 'person'],
     ['knew', 'secret'],
     ['secret', 'kept', 'huge', 'secret'],
     ['huge', 'secret'],
     ['barber', 'kept', 'word'],
     ['barber', 'kept', 'word'],
     ['barber', 'kept', 'secret'],
     ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'],
     ['barber', 'went', 'huge', 'mountain']]




```python
tokenizer = Tokenizer()
tokenizer.fit_on_texts(preprocessed_sentence)
encoded = tokenizer.texts_to_sequences(preprocessed_sentence)
print(encoded)
```

    [[1, 5], [1, 8, 5], [1, 3, 5], [9, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [7, 7, 3, 2, 10, 1, 11], [1, 12, 3, 13]]
    


```python
max_len = max(len(item) for item in encoded)
print(max_len)
```

    7
    


```python
for sentence in encoded:
    while len(sentence) < max_len:
        sentence.append(0)
        
padded_np = np.array(encoded)
padded_np
```




    array([[ 1,  5,  0,  0,  0,  0,  0],
           [ 1,  8,  5,  0,  0,  0,  0],
           [ 1,  3,  5,  0,  0,  0,  0],
           [ 9,  2,  0,  0,  0,  0,  0],
           [ 2,  4,  3,  2,  0,  0,  0],
           [ 3,  2,  0,  0,  0,  0,  0],
           [ 1,  4,  6,  0,  0,  0,  0],
           [ 1,  4,  6,  0,  0,  0,  0],
           [ 1,  4,  2,  0,  0,  0,  0],
           [ 7,  7,  3,  2, 10,  1, 11],
           [ 1, 12,  3, 13,  0,  0,  0]])



#### 2. 케라스의 전처리 도구로 패딩하기


```python
from tensorflow.keras.preprocessing.sequence import pad_sequences
```


```python
encoded = tokenizer.texts_to_sequences(preprocessed_sentence)
print(encoded)
```

    [[1, 5], [1, 8, 5], [1, 3, 5], [9, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [7, 7, 3, 2, 10, 1, 11], [1, 12, 3, 13]]
    


```python
padded = pad_sequences(encoded)
print(padded)
```

    [[ 0  0  0  0  0  1  5]
     [ 0  0  0  0  1  8  5]
     [ 0  0  0  0  1  3  5]
     [ 0  0  0  0  0  9  2]
     [ 0  0  0  2  4  3  2]
     [ 0  0  0  0  0  3  2]
     [ 0  0  0  0  1  4  6]
     [ 0  0  0  0  1  4  6]
     [ 0  0  0  0  1  4  2]
     [ 7  7  3  2 10  1 11]
     [ 0  0  0  1 12  3 13]]
    


```python
padded = pad_sequences(encoded, padding = 'post')
padded
```




    array([[ 1,  5,  0,  0,  0,  0,  0],
           [ 1,  8,  5,  0,  0,  0,  0],
           [ 1,  3,  5,  0,  0,  0,  0],
           [ 9,  2,  0,  0,  0,  0,  0],
           [ 2,  4,  3,  2,  0,  0,  0],
           [ 3,  2,  0,  0,  0,  0,  0],
           [ 1,  4,  6,  0,  0,  0,  0],
           [ 1,  4,  6,  0,  0,  0,  0],
           [ 1,  4,  2,  0,  0,  0,  0],
           [ 7,  7,  3,  2, 10,  1, 11],
           [ 1, 12,  3, 13,  0,  0,  0]])




```python
(padded == padded_np).all()
```




    True




```python
padded = pad_sequences(encoded, padding='post', maxlen=5)
padded
```




    array([[ 1,  5,  0,  0,  0],
           [ 1,  8,  5,  0,  0],
           [ 1,  3,  5,  0,  0],
           [ 9,  2,  0,  0,  0],
           [ 2,  4,  3,  2,  0],
           [ 3,  2,  0,  0,  0],
           [ 1,  4,  6,  0,  0],
           [ 1,  4,  6,  0,  0],
           [ 1,  4,  2,  0,  0],
           [ 3,  2, 10,  1, 11],
           [ 1, 12,  3, 13,  0]])




```python
padded = pad_sequences(encoded, padding='post', truncating='post', maxlen=5)
padded
```




    array([[ 1,  5,  0,  0,  0],
           [ 1,  8,  5,  0,  0],
           [ 1,  3,  5,  0,  0],
           [ 9,  2,  0,  0,  0],
           [ 2,  4,  3,  2,  0],
           [ 3,  2,  0,  0,  0],
           [ 1,  4,  6,  0,  0],
           [ 1,  4,  6,  0,  0],
           [ 1,  4,  2,  0,  0],
           [ 7,  7,  3,  2, 10],
           [ 1, 12,  3, 13,  0]])




```python
last_value = len(tokenizer.word_index) + 1
print(last_value)
```

    14
    


```python
padded = pad_sequences(encoded, padding='post', value=last_value)
padded
```




    array([[ 1,  5, 14, 14, 14, 14, 14],
           [ 1,  8,  5, 14, 14, 14, 14],
           [ 1,  3,  5, 14, 14, 14, 14],
           [ 9,  2, 14, 14, 14, 14, 14],
           [ 2,  4,  3,  2, 14, 14, 14],
           [ 3,  2, 14, 14, 14, 14, 14],
           [ 1,  4,  6, 14, 14, 14, 14],
           [ 1,  4,  6, 14, 14, 14, 14],
           [ 1,  4,  2, 14, 14, 14, 14],
           [ 7,  7,  3,  2, 10,  1, 11],
           [ 1, 12,  3, 13, 14, 14, 14]])



### 원-핫 인코딩 (One-Hot Encoding)

#### 1. 원-핫 인코딩이란?


```python
from konlpy.tag import Okt
okt = Okt()
tokens = okt.morphs('나는 자연어 처리를 배운다')
print(tokens)
```

    ['나', '는', '자연어', '처리', '를', '배운다']
    


```python
word_to_index = {word : index for index, word in enumerate(tokens)}
print('단어 집합 :', word_to_index)
```

    단어 집합 : {'나': 0, '는': 1, '자연어': 2, '처리': 3, '를': 4, '배운다': 5}
    


```python
def one_hot_encoding(word, word_index):
    one_hot_vector = [0]*(len(word_to_index))
    index = word_to_index[word]
    one_hot_vector[index] = 1
    return(one_hot_vector)
```


```python
one_hot_encoding('자연어', word_to_index)
```




    [0, 0, 1, 0, 0, 0]



#### 2) 케라스를 이용한 원-핫 인코딩


```python
text = "나랑 점심 먹으러 갈래 점심 메뉴는 햄버거 갈래 갈래 햄버거 최고야"
```


```python
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.utils import to_categorical

text = "나랑 점심 먹으러 갈래 점심 메뉴는 햄버거 갈래 갈래 햄버거 최고야"

tokenizer = Tokenizer()
tokenizer.fit_on_texts([text])
print('단어 집합 : ', tokenizer.word_index)
```

    단어 집합 :  {'갈래': 1, '점심': 2, '햄버거': 3, '나랑': 4, '먹으러': 5, '메뉴는': 6, '최고야': 7}
    


```python
sub_text = '점심 먹으러 갈래 메뉴는 햄버거 최고야'
encoded = tokenizer.texts_to_sequences([sub_text])[0]
print(encoded)
```

    [2, 5, 1, 6, 3, 7]
    


```python
one_hot = to_categorical(encoded)
print(one_hot)
```

    [[0. 0. 1. 0. 0. 0. 0. 0.]
     [0. 0. 0. 0. 0. 1. 0. 0.]
     [0. 1. 0. 0. 0. 0. 0. 0.]
     [0. 0. 0. 0. 0. 0. 1. 0.]
     [0. 0. 0. 1. 0. 0. 0. 0.]
     [0. 0. 0. 0. 0. 0. 0. 1.]]
    
