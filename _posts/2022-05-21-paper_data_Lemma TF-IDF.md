```python
import os
os.chdir(r'C:\Users\ds990\Desktop\대학교\NLP\데이터')
```


```python
import warnings
warnings.filterwarnings('ignore')
```


```python
data = pd.read_csv('raw_data.csv')
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Authors</th>
      <th>Title</th>
      <th>Source</th>
      <th>Type</th>
      <th>Keywords</th>
      <th>Abstract</th>
      <th>Corresponding_Address</th>
      <th>Citation_Num</th>
      <th>Year</th>
      <th>WoS Research Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Truant, Elisa; Broccardo, Laura; Dana, Leo-Paul</td>
      <td>Digitalisation boosts company performance: an ...</td>
      <td>TECHNOLOGICAL FORECASTING AND SOCIAL CHANGE</td>
      <td>Article</td>
      <td>Digitalization; Company performance; Listed co...</td>
      <td>Digitalisation has become embedded in products...</td>
      <td>Truant, E(?�당 ?�??, Univ Turin, Dept Managemen...</td>
      <td>2</td>
      <td>2021</td>
      <td>Business; Regional &amp; Urban Planning</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marcysiak, Agata; Pleskacz, Zanna</td>
      <td>DETERMINANTS OF DIGITIZATION IN SMES</td>
      <td>ENTREPRENEURSHIP AND SUSTAINABILITY ISSUES</td>
      <td>Article</td>
      <td>outsourcing; sustainable development; core com...</td>
      <td>The aim of the study is to present the conditi...</td>
      <td>Marcysiak, A(?�당 ?�??, Siedlce Univ Nat Sci &amp; ...</td>
      <td>2</td>
      <td>2021</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saniuk, Sebastian; Grabowska, Sandra</td>
      <td>The Concept of Cyber-Physical Networks of Smal...</td>
      <td>ENERGIES</td>
      <td>Article</td>
      <td>cyber-physical networks; Industry 4; 0; small ...</td>
      <td>The era of Industry 4.0 is characterized by th...</td>
      <td>Saniuk, S(?�당 ?�??, Univ Zielona Gora, Dept En...</td>
      <td>3</td>
      <td>2021</td>
      <td>Energy &amp; Fuels</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Shen, Lei; Sun, Cong; Ali, Muhammad</td>
      <td>Role of Servitization, Digitalization, and Inn...</td>
      <td>SUSTAINABILITY</td>
      <td>Article</td>
      <td>servitization; digitalization; dynamic capabil...</td>
      <td>The structure of the manufacturing industry ha...</td>
      <td>Ali, M(?�당 ?�??, Dongguan Univ Technol, Sch Ec...</td>
      <td>3</td>
      <td>2021</td>
      <td>Green &amp; Sustainable Science &amp; Technology; Envi...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Yang, Shuili; Yi, Yang</td>
      <td>Effect of Technological Innovation Inputs on G...</td>
      <td>JOURNAL OF GLOBAL INFORMATION MANAGEMENT</td>
      <td>Article</td>
      <td>GVC Embedding Position; Industrial Value-Added...</td>
      <td>Under the backdrop of the continuous escalatio...</td>
      <td>Yang, SL(?�당 ?�??, Xian Univ Technol, Xian, Pe...</td>
      <td>1</td>
      <td>2021</td>
      <td>Information Science &amp; Library Science</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Title 과 Abstract 변수 합치기
data['Title + Abstract'] = data['Title'] + ' ' + data['Abstract']
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1564 entries, 0 to 1563
    Data columns (total 11 columns):
     #   Column                 Non-Null Count  Dtype 
    ---  ------                 --------------  ----- 
     0   Authors                1564 non-null   object
     1   Title                  1564 non-null   object
     2   Source                 1564 non-null   object
     3   Type                   1564 non-null   object
     4   Keywords               1502 non-null   object
     5   Abstract               1558 non-null   object
     6   Corresponding_Address  1563 non-null   object
     7   Citation_Num           1564 non-null   int64 
     8   Year                   1564 non-null   int64 
     9   WoS Research Area      1564 non-null   object
     10  Title + Abstract       1558 non-null   object
    dtypes: int64(2), object(9)
    memory usage: 134.5+ KB
    


```python
data['Title + Abstract'] = data['Title + Abstract'].astype('str')
```


```python
# Lemmatizer 호출
from nltk.stem import WordNetLemmatizer
lemmatizer = WordNetLemmatizer()

import re
```


```python
#전처리
def preprocessing(text):
    text = re.sub('[^a-zA-Z]', ' ', text)
    text = re.sub('[\s]+', ' ', text)
    text = text.lower().split()
    text = [lemmatizer.lemmatize(word) for word in text]
    return (' '.join(text))
```


```python
data['Title + Abstract'] = data['Title + Abstract'].map(preprocessing)
```


```python
data['Title + Abstract']
```




    0       digitalisation boost company performance an ov...
    1       determinant of digitization in smes the aim of...
    2       the concept of cyber physical network of small...
    3       role of servitization digitalization and innov...
    4       effect of technological innovation input on gl...
                                  ...                        
    1559    integrating crowd service sourcing into digita...
    1560    producer service openness and the development ...
    1561    building digital incentive for digital custome...
    1562    moving from a good to a service oriented organ...
    1563    industry and servitisation regional pattern of...
    Name: Title + Abstract, Length: 1564, dtype: object




```python
# Null값 확인
data['Title + Abstract'].isnull().sum()
```




    0




```python
# 중복값 제거
data.drop_duplicates(subset=['Title + Abstract'], inplace=True)
```


```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1558 entries, 0 to 1563
    Data columns (total 11 columns):
     #   Column                 Non-Null Count  Dtype 
    ---  ------                 --------------  ----- 
     0   Authors                1558 non-null   object
     1   Title                  1558 non-null   object
     2   Source                 1558 non-null   object
     3   Type                   1558 non-null   object
     4   Keywords               1501 non-null   object
     5   Abstract               1557 non-null   object
     6   Corresponding_Address  1557 non-null   object
     7   Citation_Num           1558 non-null   int64 
     8   Year                   1558 non-null   int64 
     9   WoS Research Area      1558 non-null   object
     10  Title + Abstract       1558 non-null   object
    dtypes: int64(2), object(9)
    memory usage: 146.1+ KB
    


```python
# BoW
from sklearn.feature_extraction.text import CountVectorizer

vectorizer = CountVectorizer(stop_words = 'english')
```


```python
feature_vector = vectorizer.fit_transform(data['Title + Abstract'])
feature_vector.shape
```




    (1558, 8688)




```python
print(feature_vector[:10])
```

      (0, 174)	1
      (0, 183)	1
      (0, 337)	1
      (0, 655)	1
      (0, 793)	1
      (0, 822)	1
      (0, 882)	1
      (0, 1292)	7
      (0, 1404)	1
      (0, 1675)	1
      (0, 1748)	1
      (0, 1837)	1
      (0, 2136)	1
      (0, 2140)	1
      (0, 2142)	1
      (0, 2143)	9
      (0, 2296)	1
      (0, 2459)	1
      (0, 2520)	1
      (0, 2530)	1
      (0, 2558)	1
      (0, 2924)	1
      (0, 2943)	1
      (0, 3144)	1
      (0, 3450)	1
      :	:
      (9, 4752)	1
      (9, 4937)	1
      (9, 5091)	2
      (9, 5121)	1
      (9, 5221)	1
      (9, 5269)	1
      (9, 5598)	4
      (9, 5784)	2
      (9, 5988)	8
      (9, 5995)	1
      (9, 6099)	1
      (9, 6504)	3
      (9, 6679)	1
      (9, 6818)	1
      (9, 6974)	1
      (9, 7051)	12
      (9, 7268)	2
      (9, 7513)	3
      (9, 7550)	1
      (9, 7806)	2
      (9, 7940)	4
      (9, 7942)	1
      (9, 8034)	11
      (9, 8213)	1
      (9, 8307)	4
    


```python
vocab = vectorizer.get_feature_names()
print(len(vocab))
vocab[:10]
```

    8688
    




    ['ab',
     'abandon',
     'abandoned',
     'abatement',
     'abb',
     'abbreviated',
     'abducted',
     'abduction',
     'abductive',
     'abductively']




```python
pd.DataFrame(feature_vector[:10].toarray(), columns=vocab).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ab</th>
      <th>abandon</th>
      <th>abandoned</th>
      <th>abatement</th>
      <th>abb</th>
      <th>abbreviated</th>
      <th>abducted</th>
      <th>abduction</th>
      <th>abductive</th>
      <th>abductively</th>
      <th>...</th>
      <th>young</th>
      <th>younger</th>
      <th>youth</th>
      <th>zadatch</th>
      <th>zealand</th>
      <th>zero</th>
      <th>zigzag</th>
      <th>zinc</th>
      <th>zone</th>
      <th>zurich</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 8688 columns</p>
</div>




```python
dist = np.sum(feature_vector, axis=0) # 각 단어가 전체 문서에서 몇 번 나왔는지 확인

df_freq = pd.DataFrame(dist, columns=vocab)
df_freq
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ab</th>
      <th>abandon</th>
      <th>abandoned</th>
      <th>abatement</th>
      <th>abb</th>
      <th>abbreviated</th>
      <th>abducted</th>
      <th>abduction</th>
      <th>abductive</th>
      <th>abductively</th>
      <th>...</th>
      <th>young</th>
      <th>younger</th>
      <th>youth</th>
      <th>zadatch</th>
      <th>zealand</th>
      <th>zero</th>
      <th>zigzag</th>
      <th>zinc</th>
      <th>zone</th>
      <th>zurich</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>27</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
      <td>...</td>
      <td>7</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 8688 columns</p>
</div>




```python
df_freq.T.sort_values(by=0, ascending=False).head(100)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>service</th>
      <td>5879</td>
    </tr>
    <tr>
      <th>product</th>
      <td>3632</td>
    </tr>
    <tr>
      <th>ps</th>
      <td>2198</td>
    </tr>
    <tr>
      <th>study</th>
      <td>1928</td>
    </tr>
    <tr>
      <th>model</th>
      <td>1894</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>benefit</th>
      <td>304</td>
    </tr>
    <tr>
      <th>offer</th>
      <td>300</td>
    </tr>
    <tr>
      <th>user</th>
      <td>298</td>
    </tr>
    <tr>
      <th>theory</th>
      <td>297</td>
    </tr>
    <tr>
      <th>implementation</th>
      <td>297</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 1 columns</p>
</div>




```python
from sklearn.feature_extraction.text import TfidfTransformer
transformer = TfidfTransformer()
```


```python
feature_tfidf = transformer.fit_transform(feature_vector)
feature_tfidf.shape
```




    (1558, 8688)




```python
tfidf_freq = pd.DataFrame(feature_tfidf.toarray(), columns=vocab)
tfidf_freq.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ab</th>
      <th>abandon</th>
      <th>abandoned</th>
      <th>abatement</th>
      <th>abb</th>
      <th>abbreviated</th>
      <th>abducted</th>
      <th>abduction</th>
      <th>abductive</th>
      <th>abductively</th>
      <th>...</th>
      <th>young</th>
      <th>younger</th>
      <th>youth</th>
      <th>zadatch</th>
      <th>zealand</th>
      <th>zero</th>
      <th>zigzag</th>
      <th>zinc</th>
      <th>zone</th>
      <th>zurich</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 8688 columns</p>
</div>




```python
df_tfidf = pd.DataFrame(tfidf_freq.sum())
df_tfidf_top = df_tfidf.sort_values(by = 0, ascending=False)
df_tfidf_top.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>service</th>
      <td>110.625125</td>
    </tr>
    <tr>
      <th>ps</th>
      <td>80.961133</td>
    </tr>
    <tr>
      <th>product</th>
      <td>80.600609</td>
    </tr>
    <tr>
      <th>servitization</th>
      <td>59.517267</td>
    </tr>
    <tr>
      <th>model</th>
      <td>53.018947</td>
    </tr>
    <tr>
      <th>business</th>
      <td>51.848314</td>
    </tr>
    <tr>
      <th>design</th>
      <td>49.343821</td>
    </tr>
    <tr>
      <th>manufacturing</th>
      <td>45.472034</td>
    </tr>
    <tr>
      <th>study</th>
      <td>42.497696</td>
    </tr>
    <tr>
      <th>firm</th>
      <td>42.414907</td>
    </tr>
    <tr>
      <th>value</th>
      <td>41.555845</td>
    </tr>
    <tr>
      <th>customer</th>
      <td>41.067578</td>
    </tr>
    <tr>
      <th>research</th>
      <td>37.904030</td>
    </tr>
    <tr>
      <th>innovation</th>
      <td>37.815793</td>
    </tr>
    <tr>
      <th>company</th>
      <td>35.842196</td>
    </tr>
    <tr>
      <th>based</th>
      <td>35.337838</td>
    </tr>
    <tr>
      <th>process</th>
      <td>34.470261</td>
    </tr>
    <tr>
      <th>development</th>
      <td>33.611717</td>
    </tr>
    <tr>
      <th>industry</th>
      <td>33.241203</td>
    </tr>
    <tr>
      <th>approach</th>
      <td>32.120372</td>
    </tr>
  </tbody>
</table>
</div>


