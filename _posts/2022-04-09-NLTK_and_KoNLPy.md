### 토큰화(Tokenization)

#### 1. 토큰화 중 생기는 선택의 순간


```python
from nltk.tokenize import word_tokenize
from nltk.tokenize import WordPunctTokenizer
from tensorflow.keras.preprocessing.text import text_to_word_sequence
```


```python
print('단어 토큰화1:', word_tokenize("Don't be fooled by the dark sounding name, Mr.Jone's Orphanage is as cheery as cheery goes for a pastry shop."))
```

    단어 토큰화1: ['Do', "n't", 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', ',', 'Mr.Jone', "'s", 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop', '.']
    


```python
print('단어 토큰화2:', WordPunctTokenizer().tokenize("Don't be fooled by the dark sounding name, Mr.Jone's Orphanage is as cheery as cheery goes for a pastry shop."))
```

    단어 토큰화2: ['Don', "'", 't', 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', ',', 'Mr', '.', 'Jone', "'", 's', 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop', '.']
    


```python
print('단어 토큰화3:', text_to_word_sequence("Don't be fooled by the dark sounding name, Mr.Jone's Orphanage is as cheery as cheery goes for a pastry shop."))
```

    단어 토큰화3: ["don't", 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', 'mr', "jone's", 'orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop']
    

#### 2. 표준 토큰화 예제


```python
from nltk.tokenize import TreebankWordTokenizer

tokenizer = TreebankWordTokenizer()

text = "Starting a home-based restaurant may be an ideal. it doesn't have a food chain or restaurant of their own."
print('트리뱅크 워드토크나이저:', tokenizer.tokenize(text))
```

    트리뱅크 워드토크나이저: ['Starting', 'a', 'home-based', 'restaurant', 'may', 'be', 'an', 'ideal.', 'it', 'does', "n't", 'have', 'a', 'food', 'chain', 'or', 'restaurant', 'of', 'their', 'own', '.']
    


```python
from nltk.tokenize import sent_tokenize

text = "His barber kept his word. But keeping such a huge secret to himself was driving him crazy. Finally, the barber went up a mountain and almost to the edge of a cliff. He dug a hole in the midst of some reeds. He looked about, to make sure no one was near."
print('문장 토큰화1:', sent_tokenize(text))
```

    문장 토큰화1: ['His barber kept his word.', 'But keeping such a huge secret to himself was driving him crazy.', 'Finally, the barber went up a mountain and almost to the edge of a cliff.', 'He dug a hole in the midst of some reeds.', 'He looked about, to make sure no one was near.']
    


```python
text = "I'm actively looking for Ph.D. students. and you are a Ph.D student."
print('문장 토큰화2:', sent_tokenize(text))
```

    문장 토큰화2: ["I'm actively looking for Ph.D. students.", 'and you are a Ph.D student.']
    


```python
import kss

text = '딥 러닝 자연어 처리가 재미있기는 합니다. 그런데 문제는 영어보다 한국어로 할 때 너무 어렵습니다. 이제 해보면 알걸요?'
print('한국어 문장 토큰화 :', kss.split_sentences(text))
```

    [Korean Sentence Splitter]: Initializing Pynori...
    

    한국어 문장 토큰화 : ['딥 러닝 자연어 처리가 재미있기는 합니다.', '그런데 문제는 영어보다 한국어로 할 때 너무 어렵습니다.', '이제 해보면 알걸요?']
    

#### 3. NLTK와 KoNLPy를 이용한 영어, 한국어 토큰화 실습


```python
from nltk.tokenize import word_tokenize
from nltk.tag import pos_tag

text = "I am actively looking for Ph.D. students. and you are a Ph.D. student."
tokenized_sentence = word_tokenize(text)

print('단어 토큰화:', tokenized_sentence)
print('품사 태깅:', pos_tag(tokenized_sentence))
```

    단어 토큰화: ['I', 'am', 'actively', 'looking', 'for', 'Ph.D.', 'students', '.', 'and', 'you', 'are', 'a', 'Ph.D.', 'student', '.']
    품사 태깅: [('I', 'PRP'), ('am', 'VBP'), ('actively', 'RB'), ('looking', 'VBG'), ('for', 'IN'), ('Ph.D.', 'NNP'), ('students', 'NNS'), ('.', '.'), ('and', 'CC'), ('you', 'PRP'), ('are', 'VBP'), ('a', 'DT'), ('Ph.D.', 'NNP'), ('student', 'NN'), ('.', '.')]
    


```python
from konlpy.tag import Okt
from konlpy.tag import Kkma

okt = Okt()
kkma = Kkma()

print('OKT 형태소 분석:', okt.morphs('열심히 코딩한 당신, 연휴에는 여행을 가봐요'))
print('OKT 품사 태깅', okt.pos('열심히 코딩한 당신, 연휴에는 여행을 가봐요'))
print('OKT 명사 추출:', okt.nouns('열심히 코딩한 당신, 연휴에는 여행을 가봐요'))
```

    OKT 형태소 분석: ['열심히', '코딩', '한', '당신', ',', '연휴', '에는', '여행', '을', '가봐요']
    OKT 품사 태깅 [('열심히', 'Adverb'), ('코딩', 'Noun'), ('한', 'Josa'), ('당신', 'Noun'), (',', 'Punctuation'), ('연휴', 'Noun'), ('에는', 'Josa'), ('여행', 'Noun'), ('을', 'Josa'), ('가봐요', 'Verb')]
    OKT 명사 추출: ['코딩', '당신', '연휴', '여행']
    


```python
import warnings
warnings.filterwarnings('ignore')
```


```python
print('꼬꼬마 형태소 분석:', kkma.morphs('열심히 코딩한 당신, 연휴에는 여행을 가봐요'))
print('꼬꼬마 품사 태깅:', kkma.pos('열심히 코딩한 당신, 연휴에는 여행을 가봐요'))
print('꼬꼬마 명사 추출:', kkma.nouns('열심히 코딩한 당신, 연휴에는 여행을 가봐요'))
```

    꼬꼬마 형태소 분석: ['열심히', '코딩', '하', 'ㄴ', '당신', ',', '연휴', '에', '는', '여행', '을', '가보', '아요']
    꼬꼬마 품사 태깅: [('열심히', 'MAG'), ('코딩', 'NNG'), ('하', 'XSV'), ('ㄴ', 'ETD'), ('당신', 'NP'), (',', 'SP'), ('연휴', 'NNG'), ('에', 'JKM'), ('는', 'JX'), ('여행', 'NNG'), ('을', 'JKO'), ('가보', 'VV'), ('아요', 'EFN')]
    꼬꼬마 명사 추출: ['코딩', '당신', '연휴', '여행']
    
