### 한국어 전처리 패키지

#### 1. PyKoSpacing


```python
sent = '김철수는 극중 두 인격의 사나이 이광수 역을 맡았다. 철수는 한국 유일의 태권도 전승자를 가리는 결전의 날을 앞두고 10년간 함께 훈련한 사형인 유연재(김광수 분)를 찾으러 속세로 내려온 인물이다.'
```


```python
new_sent = sent.replace(" ", '')
print(new_sent)
```

    김철수는극중두인격의사나이이광수역을맡았다.철수는한국유일의태권도전승자를가리는결전의날을앞두고10년간함께훈련한사형인유연재(김광수분)를찾으러속세로내려온인물이다.
    


```python
from pykospacing import Spacing
spacing = Spacing()
kospacing_sent = spacing(new_sent)

print(sent)
print(kospacing_sent)
```

    김철수는 극중 두 인격의 사나이 이광수 역을 맡았다. 철수는 한국 유일의 태권도 전승자를 가리는 결전의 날을 앞두고 10년간 함께 훈련한 사형인 유연재(김광수 분)를 찾으러 속세로 내려온 인물이다.
    김철수는 극중 두 인격의 사나이 이광수 역을 맡았다. 철수는 한국 유일의 태권도 전승자를 가리는 결전의 날을 앞두고 10년간 함께 훈련한 사형인 유연재(김광수 분)를 찾으러 속세로 내려온 인물이다.
    

#### 2. Py-Hanspell


```python
from hanspell import spell_checker

sent = "맞춤법 틀리면 외 않되? 쓰고싶은대로쓰면돼지 "
spelled_sent = spell_checker.check(sent)

hanspell_sent = spelled_sent.checked
print(hanspell_sent)
```

    맞춤법 틀리면 왜 안돼? 쓰고 싶은 대로 쓰면 되지
    


```python
spelled_sent = spell_checker.check(new_sent)
hanspell_sent = spelled_sent.checked

print(hanspell_sent)
print(kospacing_sent)
```

    김철수는 극 중 두 인격의 사나이 이광수 역을 맡았다. 철수는 한국 유일의 태권도 전승자를 가리는 결전의 날을 앞두고 10년간 함께 훈련한 사형인 유연제(김광수 분)를 찾으러 속세로 내려온 인물이다.
    김철수는 극중 두 인격의 사나이 이광수 역을 맡았다. 철수는 한국 유일의 태권도 전승자를 가리는 결전의 날을 앞두고 10년간 함께 훈련한 사형인 유연재(김광수 분)를 찾으러 속세로 내려온 인물이다.
    

#### 3. SOYNLP를 이용한 단어 토큰화

#### 1) 신조어 문제


```python
from konlpy.tag import Okt
tokenizer = Okt()
print(tokenizer.morphs('에이비식스 이대휘 1월 최애돌 기부 요정'))
```

    ['에이', '비식스', '이대', '휘', '1월', '최애', '돌', '기부', '요정']
    

#### 2) 학습하기


```python
import urllib.request
from soynlp import DoublespaceLineCorpus
from soynlp.word import WordExtractor
```


```python
urllib.request.urlretrieve("https://raw.githubusercontent.com/lovit/soynlp/master/tutorials/2016-10-20.txt", filename="2016-10-20.txt")
```




    ('2016-10-20.txt', <http.client.HTTPMessage at 0x273cdb920a0>)




```python
# 훈련 데이터를 다수의 문서로 분리
corpus = DoublespaceLineCorpus('2016-10-20.txt')
len(corpus)
```




    30091




```python
# 상위 3개의 문서만 출력
i = 0
for document in corpus:
    if len(document) > 0:
        print(document)
        i = i+1
    if i == 3:
        break
```

    19  1990  52 1 22
    오패산터널 총격전 용의자 검거 서울 연합뉴스 경찰 관계자들이 19일 오후 서울 강북구 오패산 터널 인근에서 사제 총기를 발사해 경찰을 살해한 용의자 성모씨를 검거하고 있다 성씨는 검거 당시 서바이벌 게임에서 쓰는 방탄조끼에 헬멧까지 착용한 상태였다 독자제공 영상 캡처 연합뉴스  서울 연합뉴스 김은경 기자 사제 총기로 경찰을 살해한 범인 성모 46 씨는 주도면밀했다  경찰에 따르면 성씨는 19일 오후 강북경찰서 인근 부동산 업소 밖에서 부동산업자 이모 67 씨가 나오기를 기다렸다 이씨와는 평소에도 말다툼을 자주 한 것으로 알려졌다  이씨가 나와 걷기 시작하자 성씨는 따라가면서 미리 준비해온 사제 총기를 이씨에게 발사했다 총알이 빗나가면서 이씨는 도망갔다 그 빗나간 총알은 지나가던 행인 71 씨의 배를 스쳤다  성씨는 강북서 인근 치킨집까지 이씨 뒤를 쫓으며 실랑이하다 쓰러뜨린 후 총기와 함께 가져온 망치로 이씨 머리를 때렸다  이 과정에서 오후 6시 20분께 강북구 번동 길 위에서 사람들이 싸우고 있다 총소리가 났다 는 등의 신고가 여러건 들어왔다  5분 후에 성씨의 전자발찌가 훼손됐다는 신고가 보호관찰소 시스템을 통해 들어왔다 성범죄자로 전자발찌를 차고 있던 성씨는 부엌칼로 직접 자신의 발찌를 끊었다  용의자 소지 사제총기 2정 서울 연합뉴스 임헌정 기자 서울 시내에서 폭행 용의자가 현장 조사를 벌이던 경찰관에게 사제총기를 발사해 경찰관이 숨졌다 19일 오후 6시28분 강북구 번동에서 둔기로 맞았다 는 폭행 피해 신고가 접수돼 현장에서 조사하던 강북경찰서 번동파출소 소속 김모 54 경위가 폭행 용의자 성모 45 씨가 쏜 사제총기에 맞고 쓰러진 뒤 병원에 옮겨졌으나 숨졌다 사진은 용의자가 소지한 사제총기  신고를 받고 번동파출소에서 김창호 54 경위 등 경찰들이 오후 6시 29분께 현장으로 출동했다 성씨는 그사이 부동산 앞에 놓아뒀던 가방을 챙겨 오패산 쪽으로 도망간 후였다  김 경위는 오패산 터널 입구 오른쪽의 급경사에서 성씨에게 접근하다가 오후 6시 33분께 풀숲에 숨은 성씨가 허공에 난사한 10여발의 총알 중 일부를 왼쪽 어깨 뒷부분에 맞고 쓰러졌다  김 경위는 구급차가 도착했을 때 이미 의식이 없었고 심폐소생술을 하며 병원으로 옮겨졌으나 총알이 폐를 훼손해 오후 7시 40분께 사망했다  김 경위는 외근용 조끼를 입고 있었으나 총알을 막기에는 역부족이었다  머리에 부상을 입은 이씨도 함께 병원으로 이송됐으나 생명에는 지장이 없는 것으로 알려졌다  성씨는 오패산 터널 밑쪽 숲에서 오후 6시 45분께 잡혔다  총격현장 수색하는 경찰들 서울 연합뉴스 이효석 기자 19일 오후 서울 강북구 오패산 터널 인근에서 경찰들이 폭행 용의자가 사제총기를 발사해 경찰관이 사망한 사건을 조사 하고 있다  총 때문에 쫓던 경관들과 민간인들이 몸을 숨겼는데 인근 신발가게 직원 이모씨가 다가가 성씨를 덮쳤고 이어 현장에 있던 다른 상인들과 경찰이 가세해 체포했다  성씨는 경찰에 붙잡힌 직후 나 자살하려고 한 거다 맞아 죽어도 괜찮다 고 말한 것으로 전해졌다  성씨 자신도 경찰이 발사한 공포탄 1발 실탄 3발 중 실탄 1발을 배에 맞았으나 방탄조끼를 입은 상태여서 부상하지는 않았다  경찰은 인근을 수색해 성씨가 만든 사제총 16정과 칼 7개를 압수했다 실제 폭발할지는 알 수 없는 요구르트병에 무언가를 채워두고 심지를 꽂은 사제 폭탄도 발견됐다  일부는 숲에서 발견됐고 일부는 성씨가 소지한 가방 안에 있었다
    테헤란 연합뉴스 강훈상 특파원 이용 승객수 기준 세계 최대 공항인 아랍에미리트 두바이국제공항은 19일 현지시간 이 공항을 이륙하는 모든 항공기의 탑승객은 삼성전자의 갤럭시노트7을 휴대하면 안 된다고 밝혔다  두바이국제공항은 여러 항공 관련 기구의 권고에 따라 안전성에 우려가 있는 스마트폰 갤럭시노트7을 휴대하고 비행기를 타면 안 된다 며 탑승 전 검색 중 발견되면 압수할 계획 이라고 발표했다  공항 측은 갤럭시노트7의 배터리가 폭발 우려가 제기된 만큼 이 제품을 갖고 공항 안으로 들어오지 말라고 이용객에 당부했다  이런 조치는 두바이국제공항 뿐 아니라 신공항인 두바이월드센터에도 적용된다  배터리 폭발문제로 회수된 갤럭시노트7 연합뉴스자료사진
    


```python
word_extractor = WordExtractor()
word_extractor.train(corpus)
word_score_table = word_extractor.extract()
```

    training was done. used memory 1.189 Gb
    all cohesion probabilities was computed. # words = 223348
    all branching entropies was computed # words = 361598
    all accessor variety was computed # words = 361598
    


```python
word_score_table['반포한'].cohesion_forward
```




    0.08838002913645132




```python
word_score_table['반포한강'].cohesion_forward
```




    0.19841268168224552




```python
word_score_table['반포한강공'].cohesion_forward
```




    0.2972877884078849




```python
word_score_table['반포한강공원'].cohesion_forward
```




    0.37891487632839754




```python
word_score_table['반포한강공원에'].cohesion_forward
```




    0.33492963377557666




```python
word_score_table['디스'].right_branching_entropy
```




    1.6371694761537934




```python
word_score_table['디스플'].right_branching_entropy
```




    -0.0




```python
word_score_table['디스플레'].right_branching_entropy
```




    -0.0




```python
word_score_table['디스플레이'].right_branching_entropy
```




    3.1400392861792916




```python
from soynlp.tokenizer import LTokenizer
scores = {word : score.cohesion_forward for word, score in word_score_table.items()}
l_tokenizer = LTokenizer(scores = scores)
l_tokenizer.tokenize('국제사회와 우리의 노력들로 범죄를 척결하자', flatten=False)
```




    [('국제사회', '와'), ('우리', '의'), ('노력', '들로'), ('범죄', '를'), ('척결', '하자')]



#### 6) 최대 점수 토크나이저


```python
from soynlp.tokenizer import MaxScoreTokenizer

nospace = ('국제사회와 우리의 노력들로 범죄를 척결하자').replace(" ",'')
maxscore_tokenizer = MaxScoreTokenizer(scores = scores)
maxscore_tokenizer.tokenize(nospace)

```




    ['국제사회', '와', '우리', '의', '노력', '들로', '범죄', '를', '척결', '하자']




```python
from soynlp.normalizer import *
```


```python
print(emoticon_normalize('앜ㅋㅋㅋㅋ이영화존잼쓰ㅠㅠㅠㅠㅠ', num_repeats=2))
print(emoticon_normalize('앜ㅋㅋㅋㅋㅋㅋㅋㅋㅋ이영화존잼쓰ㅠㅠㅠㅠ', num_repeats=2))
print(emoticon_normalize('앜ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ이영화존잼쓰ㅠㅠㅠㅠㅠㅠ', num_repeats=2))
print(emoticon_normalize('앜ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ이영화존잼쓰ㅠㅠㅠㅠㅠㅠㅠㅠ', num_repeats=2))
```

    아ㅋㅋ영화존잼쓰ㅠㅠ
    아ㅋㅋ영화존잼쓰ㅠㅠ
    아ㅋㅋ영화존잼쓰ㅠㅠ
    아ㅋㅋ영화존잼쓰ㅠㅠ
    


```python
print(repeat_normalize('와하하하하하하하하하핫', num_repeats=2))
print(repeat_normalize('와하하하하하하핫', num_repeats=2))
print(repeat_normalize('와하하하하핫', num_repeats=2))
```

    와하하핫
    와하하핫
    와하하핫
    


```python
from konlpy.tag import Okt
tokenizer = Okt()
sent = '은경이는 사무실로 갔습니다.'
mor_sent = tokenizer.morphs(sent)

print('형태소 분석 입력 : ', sent)
print('형태소 분석 결과 :', mor_sent)
```

    형태소 분석 입력 :  은경이는 사무실로 갔습니다.
    형태소 분석 결과 : ['은경', '이', '는', '사무실', '로', '갔습니다', '.']
    


```python
from ckonlpy.tag import Twitter
twitter = Twitter()
twitter.morphs('은경이는 사무실로 갔습니다.')
```

    C:\Users\ds990\anaconda3\lib\site-packages\konlpy\tag\_okt.py:17: UserWarning: "Twitter" has changed to "Okt" since KoNLPy v0.4.5.
      warn('"Twitter" has changed to "Okt" since KoNLPy v0.4.5.')
    




    ['은', '경이', '는', '사무실', '로', '갔습니다', '.']




```python
import warnings
warnings.filterwarnings('ignore')
```


```python
twitter.add_dictionary('은경이', 'Noun')
```


```python
twitter.morphs('은경이는 사무실로 갔습니다.')
```




    ['은경이', '는', '사무실', '로', '갔습니다', '.']


