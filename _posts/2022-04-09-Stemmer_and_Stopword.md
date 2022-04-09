### 표제어 추출


```python
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()

words = ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched','has',
        
        'starting']

print('표제어 추출 전:', words)
print('표제어 추출 후:', [lemmatizer.lemmatize(word) for word in words])
```

    표제어 추출 전: ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
    표제어 추출 후: ['policy', 'doing', 'organization', 'have', 'going', 'love', 'life', 'fly', 'dy', 'watched', 'ha', 'starting']
    


```python
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()

sentence = "This was not the map we found in Billy Bones's chest, but an accurate copy, complete in all things--names and heights and soundings--with the single exception of the red crosses and the written notes."

tokenized_sentence = word_tokenize(sentence)

print('어간 추출 전:', tokenized_sentence)
print('어간 추출 후:', [stemmer.stem(word) for word in tokenized_sentence])
```

    어간 추출 전: ['This', 'was', 'not', 'the', 'map', 'we', 'found', 'in', 'Billy', 'Bones', "'s", 'chest', ',', 'but', 'an', 'accurate', 'copy', ',', 'complete', 'in', 'all', 'things', '--', 'names', 'and', 'heights', 'and', 'soundings', '--', 'with', 'the', 'single', 'exception', 'of', 'the', 'red', 'crosses', 'and', 'the', 'written', 'notes', '.']
    어간 추출 후: ['thi', 'wa', 'not', 'the', 'map', 'we', 'found', 'in', 'billi', 'bone', "'s", 'chest', ',', 'but', 'an', 'accur', 'copi', ',', 'complet', 'in', 'all', 'thing', '--', 'name', 'and', 'height', 'and', 'sound', '--', 'with', 'the', 'singl', 'except', 'of', 'the', 'red', 'cross', 'and', 'the', 'written', 'note', '.']
    


```python
from nltk.stem import PorterStemmer
from nltk.stem import LancasterStemmer

ps = PorterStemmer()
ls = LancasterStemmer()

words = ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']

print('어간 추출 전:', words)
print('포터 스테머의 어간 추출 후:', [ps.stem(word) for word in words])
print('랭커스터 스테머의 어간 추출 후:', [ls.stem(word) for word in words])
```

    어간 추출 전: ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
    포터 스테머의 어간 추출 후: ['polici', 'do', 'organ', 'have', 'go', 'love', 'live', 'fli', 'die', 'watch', 'ha', 'start']
    랭커스터 스테머의 어간 추출 후: ['policy', 'doing', 'org', 'hav', 'going', 'lov', 'liv', 'fly', 'die', 'watch', 'has', 'start']
    

### 불용어


```python
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from konlpy.tag import Okt
```

#### NLTK 에서 불용어 확인하기


```python
stop_words_list = stopwords.words('english')
print('불용어 개수:', len(stop_words_list))
print('불용어 10개 출력:', stop_words_list[:10])
```

    불용어 개수: 179
    불용어 10개 출력: ['i', 'me', 'my', 'myself', 'we', 'our', 'ours', 'ourselves', 'you', "you're"]
    

#### NLTK를 통해서 불용어 제거하기


```python
example = "Family is not an important thing. It's everything"
stop_words = stopwords.words('english')

word_tokens = word_tokenize(example)

result = []
for word in word_tokens:
    if word not in stop_words:
        result.append(word)
        
print('불용어 제거 전:', word_tokens)
print('불용어 제거 후:', result)
```

    불용어 제거 전: ['Family', 'is', 'not', 'an', 'important', 'thing', '.', 'It', "'s", 'everything']
    불용어 제거 후: ['Family', 'important', 'thing', '.', 'It', "'s", 'everything']
    

#### 한국어에서 불용어 제거하기


```python
okt = Okt()

example = "고기를 아무렇게나 구우려고 하면 안 돼. 고기라고 다 같은 게 아니거든. 예컨대 삼겹살을 구울 때는 중요한 게 있지."
stop_words = "를 아무렇게나 구 우려 고 안 돼 같은 게 구울 때 는"

stop_words = set(stop_words.split(' '))
word_tokens = okt.morphs(example)

result = [word for word in word_tokens if not word in stop_words]

print('불용어 제거 전:', word_tokens)
print('불용어 제거 후:', result)
```

    불용어 제거 전: ['고기', '를', '아무렇게나', '구', '우려', '고', '하면', '안', '돼', '.', '고기', '라고', '다', '같은', '게', '아니거든', '.', '예컨대', '삼겹살', '을', '구울', '때', '는', '중요한', '게', '있지', '.']
    불용어 제거 후: ['고기', '하면', '.', '고기', '라고', '다', '아니거든', '.', '예컨대', '삼겹살', '을', '중요한', '있지', '.']
    
