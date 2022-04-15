```python
import numpy as np
from numpy import dot
from numpy.linalg import norm

def cos_sim(A,B):
    return dot(A, B) / (norm(A)*norm(B))

doc1 = np.array([0,1,1,1])
doc2 = np.array([1,0,1,1])
doc3 = np.array([2,0,2,2])

print('문서1과 문서2의 유사도 :', round(cos_sim(doc1, doc2),2))
print('문서1과 문서3의 유사도 :', round(cos_sim(doc1, doc3),2))
print('문서2와 문서3의 유사도 :', round(cos_sim(doc2, doc3),2))
```

    문서1과 문서2의 유사도 : 0.67
    문서1과 문서3의 유사도 : 0.67
    문서2와 문서3의 유사도 : 1.0
    


```python
import os
os.chdir(r'C:\Users\ds990\Desktop\대학교\NLP\데이터')
```


```python
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

data = pd.read_csv('movies_metadata.csv', low_memory = False)
data.head(2)
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
      <th>adult</th>
      <th>belongs_to_collection</th>
      <th>budget</th>
      <th>genres</th>
      <th>homepage</th>
      <th>id</th>
      <th>imdb_id</th>
      <th>original_language</th>
      <th>original_title</th>
      <th>overview</th>
      <th>...</th>
      <th>release_date</th>
      <th>revenue</th>
      <th>runtime</th>
      <th>spoken_languages</th>
      <th>status</th>
      <th>tagline</th>
      <th>title</th>
      <th>video</th>
      <th>vote_average</th>
      <th>vote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>{'id': 10194, 'name': 'Toy Story Collection', ...</td>
      <td>30000000</td>
      <td>[{'id': 16, 'name': 'Animation'}, {'id': 35, '...</td>
      <td>http://toystory.disney.com/toy-story</td>
      <td>862</td>
      <td>tt0114709</td>
      <td>en</td>
      <td>Toy Story</td>
      <td>Led by Woody, Andy's toys live happily in his ...</td>
      <td>...</td>
      <td>1995-10-30</td>
      <td>373554033.0</td>
      <td>81.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}]</td>
      <td>Released</td>
      <td>NaN</td>
      <td>Toy Story</td>
      <td>False</td>
      <td>7.7</td>
      <td>5415.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>NaN</td>
      <td>65000000</td>
      <td>[{'id': 12, 'name': 'Adventure'}, {'id': 14, '...</td>
      <td>NaN</td>
      <td>8844</td>
      <td>tt0113497</td>
      <td>en</td>
      <td>Jumanji</td>
      <td>When siblings Judy and Peter discover an encha...</td>
      <td>...</td>
      <td>1995-12-15</td>
      <td>262797249.0</td>
      <td>104.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}, {'iso...</td>
      <td>Released</td>
      <td>Roll the dice and unleash the excitement!</td>
      <td>Jumanji</td>
      <td>False</td>
      <td>6.9</td>
      <td>2413.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 24 columns</p>
</div>




```python
# 상위 2만개의 샘플을 데이터에 저장
data = data.head(20000)
```


```python
# overview 열에 존재하는 모든 결측값을 전부 카운트하여 출력
print('overview 열의 결측값의 수 :', data['overview'].isnull().sum())
```

    overview 열의 결측값의 수 : 135
    


```python
# 결측값을 빈 값으로 대체
data['overview'] = data['overview'].fillna('')
```


```python
tfidf = TfidfVectorizer(stop_words = 'english')
tfidf_matrix = tfidf.fit_transform(data['overview'])
print('TF-IDF 행렬의 크기(shape) :', tfidf_matrix.shape)
```

    TF-IDF 행렬의 크기(shape) : (20000, 47487)
    


```python
cosine_sim = cosine_similarity(tfidf_matrix, tfidf_matrix)
print('코사인 유사도 연산 결과:', cosine_sim.shape)
```

    코사인 유사도 연산 결과: (20000, 20000)
    


```python
title_to_index = dict(zip(data['title'], data.index))

# 영화 제목 Father of the Bride Part 2의 인덱스를 리턴
idx = title_to_dict['Father of the Bride Part II']
print(idx)
```

    4
    


```python
def get_recommendations(title, cosine_sim = cosine_sim):
    # 선택한 영화의 타이틀로부터 해당 영화의 인덱스를 받아온다.
    idx = title_to_index[title]
    
    # 해당 영화와 모든 영화와의 유사도를 가져온다
    sim_scores = list(enumerate(cosine_sim[idx]))
    
    # 유사도에 따라 영화들을 정렬한다.
    sim_scores = sorted(sim_scores, key=lambda x:x[1], reverse=True)
    
    # 가장 유사한 10개의 영화를 받아온다.
    sim_scores = sim_scores[1:11]
    
    # 가장 유사한 10개의 영화의 인덱스를 얻는다.
    movie_indices = [idx[0] for idx in sim_scores]
    
    # 가장 유사한 10개의 영화의 제목을 리턴한다.
    return data['title'].iloc[movie_indices]
```


```python
get_recommendations('The Dark Knight Rises')
```




    12481                            The Dark Knight
    150                               Batman Forever
    1328                              Batman Returns
    15511                 Batman: Under the Red Hood
    585                                       Batman
    9230          Batman Beyond: Return of the Joker
    18035                           Batman: Year One
    19792    Batman: The Dark Knight Returns, Part 1
    3095                Batman: Mask of the Phantasm
    10122                              Batman Begins
    Name: title, dtype: object


