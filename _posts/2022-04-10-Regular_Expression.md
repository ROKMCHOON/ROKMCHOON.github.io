### 1. 정규 표현식 실습


```python
import re
```

#### 1) .기호


```python
r = re.compile('a.c')
r.search('kkk') # 아무런 결과도 출력되지 않는다.
```


```python
r.search('abc')
```




    <re.Match object; span=(0, 3), match='abc'>



.은 어떤 문자로도 인식될 수 있기 때문에 abc라는 문자열은 a.c라는 정규표현식 패턴으로 매치됩니다.

#### 2) ?기호

?는 앞의 문자가 존재할 수도 있고 존재하지 않을 수도 있는 경우를 나타냅니다. 예를 들어서 정규 표현식이 ab?c라고 합시다.
이 경우 이 정규 표현식에서의 b는 있다고 취급할 수도 있고, 없다고 취급할 수도 있습니다. 즉 abc와 ac 모두 매치할 수 있습니다.


```python
r = re.compile('ab?c')
r.search('abbc') # 아무런 결과도 출력되지 않는다.
```


```python
r.search('abc')
```




    <re.Match object; span=(0, 3), match='abc'>



b가 있는것으로 판단하여 abc를 매치했습니다.


```python
r.search('ac')
```




    <re.Match object; span=(0, 2), match='ac'>



b가 없는것으로 판단하여 ac를 매치했습니다.

#### 3) *기호

' * '은 바로 앞의 문자가 0개 이상일 경우를 나타냅니다. 앞의 문자는 존재하지 않을 수도 있으며, 또는 여러 개일 수도 있습니다.
정규 표현식이 ab*c 라면 ac,abc,abbbc 등과 매치할 수 있으며 b의 개수는 무수히 많을 수도 있습니다.


```python
r = re.compile('ab*c')
r.search('a') # 아무런 결과도 출력되지 않는다.
```


```python
r.search('ac')
```




    <re.Match object; span=(0, 2), match='ac'>




```python
r.search('abc')
```




    <re.Match object; span=(0, 3), match='abc'>




```python
r.search('abbbbbbbc')
```




    <re.Match object; span=(0, 9), match='abbbbbbbc'>



#### 4) +기호

+는 *와 유사합니다. 다른 점은 앞의 문자가 최소 1개 이상이어야 합니다. 정규 표현식이 ab+c라고 한다면 ac는 매치되지 않습니다.
하지만 abc, abbbc, abbbbbbc 등과 매치할 수 있으며 b의 개수는 무수히 많을 수 있습니다.


```python
r = re.compile('ab+c')
r.search('ac') # 아무런 결과도 출력되지 않는다.
```


```python
r.search('abc')
```




    <re.Match object; span=(0, 3), match='abc'>




```python
r.search('abbbbbc')
```




    <re.Match object; span=(0, 7), match='abbbbbc'>




```python
r.search('abbbbbbbbc')
```




    <re.Match object; span=(0, 10), match='abbbbbbbbc'>



#### 5) ^기호

^는 시작되는 문자열을 지정합니다. 정규표현식이 ^ab라면 문자열 ab로 시작되는 경우 매치합니다.


```python
r = re.compile('^ab')

# 아무런 결과도 출력되지 않는다.
r.search('ac')
r.search('bbc')
r.search('zab')
```


```python
r.search('abz')
```




    <re.Match object; span=(0, 2), match='ab'>



#### 6) {숫자} 기호

문자에 해당 기호를 붙이면, 해당 문자를 숫자만큼 반복한 것을 나타냅니다. 예를 들어서 정규 표현식이 ab{2}c 라면 
a와c 사이에 b가 존재하면서 b가 2개인 문자열에 대해서 매치합니다.


```python
r = re.compile('ab{2}c')

# 아무런 결과도 출력되지 않는다.
r.search('abc')
r.search('abbbc')
r.search('abbbbbc')
```


```python
r.search('abbc')
```




    <re.Match object; span=(0, 4), match='abbc'>



#### 7) {숫자1, 숫자2} 기호

문자에 해당 기호를 붙이면, 해당 문자를 숫자1 이상 숫자2 이하만큼 반복합니다. 예를 들어서 정규 표현식이 ab{2,8}c 라면 
a와c 사이에 b가 존재하면서 b는 2개 이상 8개 이하인 문자열에 대해서 매치합니다.


```python
r = re.compile('ab{2,8}c')

# 아무런 결과도 출력되지 않는다.
r.search('abc')
r.search('ac')
r.search('abbbbbbbbbbbbc')
```


```python
r.search('abbbbbbbc')
```




    <re.Match object; span=(0, 9), match='abbbbbbbc'>




```python
r.search('abbbc')
```




    <re.Match object; span=(0, 5), match='abbbc'>



#### 8) {숫자,} 기호

문자에 해당 기호를 붙이면 해당 문자를 숫자 이상만큼 반복합니다. 예를 들어서 정규 표현식이 a{2,}bc라면 뒤에 bc가 붙으면서
a의 개수가 2개 이상인 경우인 문자열과 매치합니다. 또한 만약 {0,}을 쓴다면 *와 동일한 의미가 되며, {1,}을 쓴다면 +와 동일한 의미가 됩니다.


```python
r = re.compile('a{2,}bc')

# 아무런 결과도 출력되지 않는다.
r.search('abc')
r.search('abbbbc')
r.search('abcccc')
r.search('bc')
r.search('aa')
```


```python
r.search('aabc')
```




    <re.Match object; span=(0, 4), match='aabc'>




```python
r.search('aaaaaabc')
```




    <re.Match object; span=(0, 8), match='aaaaaabc'>



#### 9) [ ]기호

[]안에 문자들을 넣으면 그 문자들 중 한 개의 문자와 매치하라는 의미를 가집니다. 예를 들어서 정규 표현식이 [abc]라면,
a 또는 b 또는 c가 들어가있는 문자열과 매치됩니다. 범위를 지정하는 것도 가능합니다. [a-zA-Z]는 알파벳 전부를 의미하며,
[0-9]는 숫자 전부를 의미합니다.


```python
r = re.compile('[abc]') # [abc]는 [a-c]와 같다.
r.search('zzz') # 아무런 결과도 출력되지 않는다.
```


```python
r.search('a')
```




    <re.Match object; span=(0, 1), match='a'>




```python
r.search('aaaaaa')
```




    <re.Match object; span=(0, 1), match='a'>




```python
r.search('baac')
```




    <re.Match object; span=(0, 1), match='b'>




```python
r = re.compile('[a-z]')

# 아무런 결과도 출력되지 않는다.
r.search('ABC')
r.search('111')
```


```python
r.search('aBC')
```




    <re.Match object; span=(0, 1), match='a'>



#### 10) [^문자] 기호

[^문자]는 ^기호 뒤에 붙은 문자들을 제외한 모든 문자를 매치하는 역할을 합니다. 예를 들어서 [^abc]라는 정규 표현식이 있다면,
a 또는 b 또는 c가 들어간 문자열을 제외한 모든 문자열을 매치합니다.


```python
r = re.compile('[^abc]')

# 아무런 결과도 출력되지 않는다.
r.search('a')
r.search('ab')
r.search('ccc')
```


```python
r.search('d')
```




    <re.Match object; span=(0, 1), match='d'>




```python
r.search("1")
```




    <re.Match object; span=(0, 1), match='1'>



### 3. 정규 표현식 모듈 함수 예제

#### (1) re.match() 와 re.search() 의 차이

search()가 정규 표현식 전체에 대해서 문자열이 매치하는지를 본다면, match()는 문자열의 첫 부분부터 정규 표현식과 매치하는지를 확인합니다.
문자열 중간에 찾을 패턴이 있더라도 match 함수는 문자열의 시작에서 패턴이 일치하지 않으면 찾지 않습니다.


```python
r = re.compile('ab.')
r.match('kkkkabc') # 아무런 결과도 출력되지 않는다.
```


```python
r.search('kkkkabc')
```




    <re.Match object; span=(4, 7), match='abc'>




```python
r.match('abckkkk')
```




    <re.Match object; span=(0, 3), match='abc'>



#### (2) re.split()

split() 함수는 입력된 정규 표현식을 기준으로 문자열들을 분리하여 리스트로 리턴합니다. 토큰화에 유용하게 쓰일 수 있습니다.


```python
# 공백 기준 분리
text = '사과 딸기 수박 메론 바나나'

re.split(' ', text)
```




    ['사과', '딸기', '수박', '메론', '바나나']




```python
# 줄바꿈 기준 분리
text = '''사과
딸기
수박
메론
바나나'''

re.split('\n', text)
```




    ['사과', '딸기', '수박', '메론', '바나나']




```python
# '+'를 기준으로 분리

text = '사과+딸기+수박+메론+바나나'

re.split('\+', text)
```




    ['사과', '딸기', '수박', '메론', '바나나']



#### (3) re.findall()

findall() 함수는 정규 표현식과 매치되는 모든 문자열들을 리스트로 리턴합니다. 단, 매치되는 문자열이 없다면 빈 리스트를 리턴합니다.
임의의 텍스트에 정규 표현식으로 숫자를 의미하는 규칙으로 findall()을 수행하면 전체 텍스트로부터 숫자만 찾아내서 리스트로 리턴합니다.


```python
text = ''' 이름 : 김철수
전화번호 : 010 - 1234 - 1234
나이 : 30
성별 : 남'''

re.findall('\d+', text)
```




    ['010', '1234', '1234', '30']



하지만 만약 입력 텍스트에 숫자가 없다면 빈 리스트를 리턴하게 됩니다.


```python
re.findall('\d+', '문자열입니다.')
```




    []



#### (4) re.sub()

sub() 함수는 정규 표현식 패턴과 일치하는 문자열을 찾아 다른 문자열로 대체할 수 있습니다. 아래와 같은 정제 작업에 많이 사용되는데,
영어 문장에 각주 등과 같은 이유로 특수 문자가 섞여있는 경우에 특수 문자를 제거하고 싶다면 알파벳 외의 문자는 
공백으로 처리하는 등의 용도로 쓸 수 있습니다.


```python
text = "Regular expression : A regular expression, regex or regexp[1] (sometimes called a rational expression)[2][3] is, in theoretical computer science and formal language theory, a sequence of characters that define a search pattern."

processed_text = re.sub('[^a-zA-Z]', ' ', text)
processed_text
```




    'Regular expression   A regular expression  regex or regexp     sometimes called a rational expression        is  in theoretical computer science and formal language theory  a sequence of characters that define a search pattern '



### 4. 정규 표현식 텍스트 전처리 예제


```python
text = """100 John    PROF
101 James   STUD
102 Mac   STUD"""
```


```python
re.split('\s+', text)
```




    ['100', 'John', 'PROF', '101', 'James', 'STUD', '102', 'Mac', 'STUD']




```python
re.findall('\d+', text)
```




    ['100', '101', '102']




```python
re.findall('[A-Z]', text)
```




    ['J', 'P', 'R', 'O', 'F', 'J', 'S', 'T', 'U', 'D', 'M', 'S', 'T', 'U', 'D']




```python
re.findall('[A-Z]{4}', text)
```




    ['PROF', 'STUD', 'STUD']




```python
re.findall('[A-Z][a-z]+', text)
```




    ['John', 'James', 'Mac']



### 5. 정규 표현식을 이용한 토큰화


```python
from nltk.tokenize import RegexpTokenizer

text = "Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop"

tokenizer1 = RegexpTokenizer('[\w]+')
tokenizer2 = RegexpTokenizer('\s+', gaps=True)

print(tokenizer1.tokenize(text))
print(tokenizer2.tokenize(text))
```

    ['Don', 't', 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', 'Mr', 'Jone', 's', 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop']
    ["Don't", 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name,', 'Mr.', "Jone's", 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop']
    
