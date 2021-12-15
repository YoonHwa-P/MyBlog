---
title: "Text Mining in Python"
date: 2021-12-15 17:11:16
categories:
- python
- machineLeaning
tags:
- Study
- python
- machineLeaning
- TextMining
---

## 개요
- 빅데이터 분석 및 시각화 & 텍스트 마이닝
<br><br>



- Ref01_ [Matplotlib 히스토그램 그리기](https://wikidocs.net/92112)
- Ref02_ [딥 러닝을 이용한 자연어 처리 입문](https://wikidocs.net/94600)
- 네이버 쇼핑 리뷰 감성 분류하기(Naver Shopping Review Sentiment Analysis)


<br><br>

---

## 평가
- 다음은 네이버 쇼핑 리뷰 감성 분류하기 예제입니다. 
- 빈칸에 `# 코드 입력`란에 적당한 코드를 작성하시기를 바랍니다. 
- 각 빈칸당 10점입니다. 

### Colab에 Mecab 설치


```python
# Colab에 Mecab 설치
!git clone https://github.com/SOMJANG/Mecab-ko-for-Google-Colab.git
%cd Mecab-ko-for-Google-Colab
!bash install_mecab-ko_on_colab190912.sh
```

    Cloning into 'Mecab-ko-for-Google-Colab'...
    remote: Enumerating objects: 91, done.[K
    remote: Total 91 (delta 0), reused 0 (delta 0), pack-reused 91[K
    Unpacking objects: 100% (91/91), done.
    /content/Mecab-ko-for-Google-Colab
    Installing konlpy.....
    Collecting konlpy
      Downloading konlpy-0.5.2-py2.py3-none-any.whl (19.4 MB)
    [K     |████████████████████████████████| 19.4 MB 2.4 MB/s 
    [?25hCollecting JPype1>=0.7.0
      Downloading JPype1-1.3.0-cp37-cp37m-manylinux_2_5_x86_64.manylinux1_x86_64.whl (448 kB)
    [K     |████████████████████████████████| 448 kB 23.5 MB/s 
    [?25hRequirement already satisfied: lxml>=4.1.0 in /usr/local/lib/python3.7/dist-packages (from konlpy) (4.2.6)
    Collecting colorama
      Downloading colorama-0.4.4-py2.py3-none-any.whl (16 kB)
    Requirement already satisfied: tweepy>=3.7.0 in /usr/local/lib/python3.7/dist-packages (from konlpy) (3.10.0)
    Requirement already satisfied: numpy>=1.6 in /usr/local/lib/python3.7/dist-packages (from konlpy) (1.19.5)
    Collecting beautifulsoup4==4.6.0
      Downloading beautifulsoup4-4.6.0-py3-none-any.whl (86 kB)
    [K     |████████████████████████████████| 86 kB 2.4 MB/s 
    [?25hRequirement already satisfied: typing-extensions in /usr/local/lib/python3.7/dist-packages (from JPype1>=0.7.0->konlpy) (3.10.0.2)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in /usr/local/lib/python3.7/dist-packages (from tweepy>=3.7.0->konlpy) (1.3.0)
    Requirement already satisfied: requests[socks]>=2.11.1 in /usr/local/lib/python3.7/dist-packages (from tweepy>=3.7.0->konlpy) (2.23.0)
    Requirement already satisfied: six>=1.10.0 in /usr/local/lib/python3.7/dist-packages (from tweepy>=3.7.0->konlpy) (1.15.0)
    Requirement already satisfied: oauthlib>=3.0.0 in /usr/local/lib/python3.7/dist-packages (from requests-oauthlib>=0.7.0->tweepy>=3.7.0->konlpy) (3.1.1)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /usr/local/lib/python3.7/dist-packages (from requests[socks]>=2.11.1->tweepy>=3.7.0->konlpy) (1.24.3)
    Requirement already satisfied: chardet<4,>=3.0.2 in /usr/local/lib/python3.7/dist-packages (from requests[socks]>=2.11.1->tweepy>=3.7.0->konlpy) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.7/dist-packages (from requests[socks]>=2.11.1->tweepy>=3.7.0->konlpy) (2021.10.8)
    Requirement already satisfied: idna<3,>=2.5 in /usr/local/lib/python3.7/dist-packages (from requests[socks]>=2.11.1->tweepy>=3.7.0->konlpy) (2.10)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in /usr/local/lib/python3.7/dist-packages (from requests[socks]>=2.11.1->tweepy>=3.7.0->konlpy) (1.7.1)
    Installing collected packages: JPype1, colorama, beautifulsoup4, konlpy
      Attempting uninstall: beautifulsoup4
        Found existing installation: beautifulsoup4 4.6.3
        Uninstalling beautifulsoup4-4.6.3:
          Successfully uninstalled beautifulsoup4-4.6.3
    Successfully installed JPype1-1.3.0 beautifulsoup4-4.6.0 colorama-0.4.4 konlpy-0.5.2
    Done
    Installing mecab-0.996-ko-0.9.2.tar.gz.....
    Downloading mecab-0.996-ko-0.9.2.tar.gz.......
    from https://bitbucket.org/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.2.tar.gz
    --2021-12-15 08:19:45--  https://bitbucket.org/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.2.tar.gz
    Resolving bitbucket.org (bitbucket.org)... 104.192.141.1, 2406:da00:ff00::22c0:3470, 2406:da00:ff00::22e9:9f55, ...
    Connecting to bitbucket.org (bitbucket.org)|104.192.141.1|:443... connected.
    HTTP request sent, awaiting response... 302 Found
    Location: https://bbuseruploads.s3.amazonaws.com/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.2.tar.gz?Signature=Djk%2BX4VYfoZUGHzDRgTrcVVdFvE%3D&Expires=1639557778&AWSAccessKeyId=AKIA6KOSE3BNJRRFUUX6&versionId=null&response-content-disposition=attachment%3B%20filename%3D%22mecab-0.996-ko-0.9.2.tar.gz%22&response-content-encoding=None [following]
    --2021-12-15 08:19:46--  https://bbuseruploads.s3.amazonaws.com/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.2.tar.gz?Signature=Djk%2BX4VYfoZUGHzDRgTrcVVdFvE%3D&Expires=1639557778&AWSAccessKeyId=AKIA6KOSE3BNJRRFUUX6&versionId=null&response-content-disposition=attachment%3B%20filename%3D%22mecab-0.996-ko-0.9.2.tar.gz%22&response-content-encoding=None
    Resolving bbuseruploads.s3.amazonaws.com (bbuseruploads.s3.amazonaws.com)... 52.216.113.163
    Connecting to bbuseruploads.s3.amazonaws.com (bbuseruploads.s3.amazonaws.com)|52.216.113.163|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 1414979 (1.3M) [application/x-tar]
    Saving to: ‘mecab-0.996-ko-0.9.2.tar.gz’
    
    mecab-0.996-ko-0.9. 100%[===================>]   1.35M  1.07MB/s    in 1.3s    
    
    2021-12-15 08:19:48 (1.07 MB/s) - ‘mecab-0.996-ko-0.9.2.tar.gz’ saved [1414979/1414979]
    
    Done
    Unpacking mecab-0.996-ko-0.9.2.tar.gz.......
    Done
    Change Directory to mecab-0.996-ko-0.9.2.......
    installing mecab-0.996-ko-0.9.2.tar.gz........
    configure
    make
    make check
    make install
    ldconfig
    Done
    Change Directory to /content
    Downloading mecab-ko-dic-2.1.1-20180720.tar.gz.......
    from https://bitbucket.org/eunjeon/mecab-ko-dic/downloads/mecab-ko-dic-2.1.1-20180720.tar.gz
    --2021-12-15 08:21:19--  https://bitbucket.org/eunjeon/mecab-ko-dic/downloads/mecab-ko-dic-2.1.1-20180720.tar.gz
    Resolving bitbucket.org (bitbucket.org)... 104.192.141.1, 2406:da00:ff00::6b17:d1f5, 2406:da00:ff00::22cd:e0db, ...
    Connecting to bitbucket.org (bitbucket.org)|104.192.141.1|:443... connected.
    HTTP request sent, awaiting response... 302 Found
    Location: https://bbuseruploads.s3.amazonaws.com/a4fcd83e-34f1-454e-a6ac-c242c7d434d3/downloads/b5a0c703-7b64-45ed-a2d7-180e962710b6/mecab-ko-dic-2.1.1-20180720.tar.gz?Signature=ZNAR2x6%2FNWxJ4p%2BOkG%2BjdG77Dqk%3D&Expires=1639558279&AWSAccessKeyId=AKIA6KOSE3BNJRRFUUX6&versionId=tzyxc1TtnZU_zEuaaQDGN4F76hPDpyFq&response-content-disposition=attachment%3B%20filename%3D%22mecab-ko-dic-2.1.1-20180720.tar.gz%22&response-content-encoding=None [following]
    --2021-12-15 08:21:19--  https://bbuseruploads.s3.amazonaws.com/a4fcd83e-34f1-454e-a6ac-c242c7d434d3/downloads/b5a0c703-7b64-45ed-a2d7-180e962710b6/mecab-ko-dic-2.1.1-20180720.tar.gz?Signature=ZNAR2x6%2FNWxJ4p%2BOkG%2BjdG77Dqk%3D&Expires=1639558279&AWSAccessKeyId=AKIA6KOSE3BNJRRFUUX6&versionId=tzyxc1TtnZU_zEuaaQDGN4F76hPDpyFq&response-content-disposition=attachment%3B%20filename%3D%22mecab-ko-dic-2.1.1-20180720.tar.gz%22&response-content-encoding=None
    Resolving bbuseruploads.s3.amazonaws.com (bbuseruploads.s3.amazonaws.com)... 54.231.82.195
    Connecting to bbuseruploads.s3.amazonaws.com (bbuseruploads.s3.amazonaws.com)|54.231.82.195|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 49775061 (47M) [application/x-tar]
    Saving to: ‘mecab-ko-dic-2.1.1-20180720.tar.gz’
    
    mecab-ko-dic-2.1.1- 100%[===================>]  47.47M  13.0MB/s    in 4.5s    
    
    2021-12-15 08:21:25 (10.5 MB/s) - ‘mecab-ko-dic-2.1.1-20180720.tar.gz’ saved [49775061/49775061]
    
    Done
    Unpacking  mecab-ko-dic-2.1.1-20180720.tar.gz.......
    Done
    Change Directory to mecab-ko-dic-2.1.1-20180720
    Done
    installing........
    configure
    make
    make install
    apt-get update
    apt-get upgrade
    apt install curl
    apt install git
    bash <(curl -s https://raw.githubusercontent.com/konlpy/konlpy/master/scripts/mecab.sh)
    Done
    Successfully Installed
    Now you can use Mecab
    from konlpy.tag import Mecab
    mecab = Mecab()
    사용자 사전 추가 방법 : https://bit.ly/3k0ZH53
    NameError: name 'Tagger' is not defined 오류 발생 시 런타임을 재실행 해주세요
    블로그에 해결 방법을 남겨주신 tana님 감사합니다.
    

## 네이버 쇼핑 리뷰 데이터에 대한 이해와 전처리
- 


```python
import re
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import urllib.request
from collections import Counter
from konlpy.tag import Mecab
from sklearn.model_selection import train_test_split
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
```

## 데이터 불러오기


```python
urllib.request.urlretrieve("https://raw.githubusercontent.com/bab2min/corpus/master/sentiment/naver_shopping.txt", filename="ratings_total.txt")
```




    ('ratings_total.txt', <http.client.HTTPMessage at 0x7f7d3557f750>)



- 해당 데이터에는 열 제목이 별도로 없음. 그래서 임의로 두 개의 열제목인 "ratings"와 "reviews" 추가


```python
# (1) 데이터 불러오고, 전체 리뷰 개수 출력 # 200,000
totalDt = pd.read_table('ratings_total.txt', names=['ratings', 'reviews'])
print('전체 리뷰 개수 :',len(totalDt)) # 전체 리뷰 개수 출력
```

    전체 리뷰 개수 : 200000
    


```python
totalDt[:5]
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
      <th>ratings</th>
      <th>reviews</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>배공빠르고 굿</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>택배가 엉망이네용 저희집 밑에층에 말도없이 놔두고가고</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>아주좋아요 바지 정말 좋아서2개 더 구매했어요 이가격에 대박입니다. 바느질이 조금 ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>선물용으로 빨리 받아서 전달했어야 하는 상품이었는데 머그컵만 와서 당황했습니다. 전...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>민트색상 예뻐요. 옆 손잡이는 거는 용도로도 사용되네요 ㅎㅎ</td>
    </tr>
  </tbody>
</table>
</div>



- 훈련 데이터와 테스트 데이터 분리하기


```python
totalDt['label'] = np.select([totalDt.ratings > 3], [1], default=0)
totalDt[:5]
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
      <th>ratings</th>
      <th>reviews</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>배공빠르고 굿</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>택배가 엉망이네용 저희집 밑에층에 말도없이 놔두고가고</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>아주좋아요 바지 정말 좋아서2개 더 구매했어요 이가격에 대박입니다. 바느질이 조금 ...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>선물용으로 빨리 받아서 전달했어야 하는 상품이었는데 머그컵만 와서 당황했습니다. 전...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>민트색상 예뻐요. 옆 손잡이는 거는 용도로도 사용되네요 ㅎㅎ</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- 각 열에 대해서 중복을 제외한 샘플의 수 카운트


```python
totalDt['ratings'].nunique(), totalDt['reviews'].nunique(), totalDt['label'].nunique()
```




    (4, 199908, 2)



- ratings열의 경우 1, 2, 4, 5라는 네 가지 값을 가지고 있습니다. reviews열에서 중복을 제외한 경우 199,908개입니다. 현재 20만개의 리뷰가 존재하므로 이는 현재 갖고 있는 데이터에 중복인 샘플들이 있다는 의미입니다. 중복인 샘플들을 제거해줍니다.


```python
# (2) review열에서 중복 데이터 제거 drop_duplicates() 함수 활용
totalDt.drop_duplicates(subset=['reviews'], inplace=True)
print('총 샘플의 수 :',len(totalDt))
```

    총 샘플의 수 : 199908
    

- NULL 값 유무 확인


```python
print(totalDt.isnull().values.any())
```

    False
    

- 훈련 데이터와 테스트 데이터를 3:1 비율로 분리


```python
train_data, test_data = train_test_split(totalDt, test_size = 0.25, random_state = 42)
print('훈련용 리뷰의 개수 :', len(train_data))
print('테스트용 리뷰의 개수 :', len(test_data))
```

    훈련용 리뷰의 개수 : 149931
    테스트용 리뷰의 개수 : 49977
    

## 레이블의 분포 확인


```python
# (3) label 1, 0 막대그래프 그리기
import matplotlib.pyplot as plt
import numpy as np

fig, ax = plt.subplots(1,1,figsize=(7,5))
width = 0.15

plot_Dt= train_data['label'].value_counts().plot(kind = 'bar', color='orange', edgecolor='black').legend()

plt.title('train_data',fontsize=20) ## 타이틀 출력
plt.ylabel('Count',fontsize=10) ## y축 라벨 출력
plt.show()

```


    
![train_data](../../imeges/python/output_22_0.png)
    



```python
print(train_data.groupby('label').size().reset_index(name = 'count'))
```

       label  count
    0      0  74918
    1      1  75013
    

- 두 레이블 모두 약 7만 5천개로 50:50 비율을 가짐

## 데이터 정제하기
- 정규 표현식을 사용하여 한글을 제외하고 모두 제거해줍니다. 


```python
# 한글과 공백을 제외하고 모두 제거
# (4) 한글 및 공백 제외한 모든 글자 제거
train_data['reviews'] = train_data['reviews'].str.replace("[^ㄱ-ㅎㅏ-ㅣ가-힣 ]","")
train_data['reviews'].replace('', np.nan, inplace=True)
print(train_data.isnull().sum())
```

    ratings    0
    reviews    0
    label      0
    dtype: int64
    

- 테스트 데이터에 대해서도 같은 과정을 거칩니다. 


```python
# (5) 데스트 데이터에 적용하기
# 코드 1 중복 제거
# 코드 2 정규 표현식 수행
# 코드 3 공백은 Null 값으로 변경
# 코드 4 Null 값 제거
test_data.drop_duplicates(subset = ['reviews'], inplace=True) # 중복 제거
test_data['reviews'] = test_data['reviews'].str.replace("[^ㄱ-ㅎㅏ-ㅣ가-힣 ]","") # 정규 표현식 수행
test_data['reviews'].replace('', np.nan, inplace=True) # 공백은 Null 값으로 변경
test_data = test_data.dropna(how='any') # Null 값 제거
print('전처리 후 테스트용 샘플의 개수 :',len(test_data))
```

    전처리 후 테스트용 샘플의 개수 : 49977
    

## 토큰화 
- 형태소 분석기 Mecab을 사용하여 토큰화 작업을 수행한다. 


```python
# (6) Mecab 클래스 호출하기
mecab = Mecab()
print(mecab.morphs('와 이런 것도 상품이라고 차라리 내가 만드는 게 나을 뻔'))
```

    ['와', '이런', '것', '도', '상품', '이', '라고', '차라리', '내', '가', '만드', '는', '게', '나을', '뻔']
    

- 불용어를 지정하여 필요없는 토큰들을 제거하도록 한다. 


```python
# (7) 불용어 만들기
stopwords = ['도', '는', '다', '의', '가', '이', '은', '한', '에', '하', '고', '을', '를', '인', '듯', '과', '와', '네', '들', '듯', '지', '임', '게']
```

- 훈련 데이터와 테스트 데이터에 대해서 동일한 과정을 거친다. 


```python
train_data['tokenized'] = train_data['reviews'].apply(mecab.morphs)
train_data['tokenized'] = train_data['tokenized'].apply(lambda x: [item for item in x if item not in stopwords])
```


```python
test_data['tokenized'] = test_data['reviews'].apply(mecab.morphs)
test_data['tokenized'] = test_data['tokenized'].apply(lambda x: [item for item in x if item not in stopwords])
```

## 단어와 길이 분포 확인하기
긍정 리뷰에는 주로 어떤 단어들이 많이 등장하고, 부정 리뷰에는 주로 어떤 단어들이 등장하는지 두 가지 경우에 대해서 각 단어의 빈도수를 계산해보겠습니다. 각 레이블에 따라서 별도로 단어들의 리스트를 저장해줍니다.




```python
negative_W = np.hstack(train_data[train_data.label == 0]['tokenized'].values)
positive_W = np.hstack(train_data[train_data.label == 1]['tokenized'].values)
negative_W
positive_W
```




    array(['적당', '만족', '합니다', ..., '잘', '삿', '어요'], dtype='<U25')



- Counter()를 사용하여 각 단어에 대한 빈도수를 카운트한다. 우선 부정 리뷰에 대해서 빈도수가 높은 상위 20개 단어 출력


```python
negative_word_count = Counter(negative_W)
print(negative_word_count.most_common(20))
```

    [('네요', 31799), ('는데', 20295), ('안', 19718), ('어요', 14849), ('있', 13200), ('너무', 13058), ('했', 11783), ('좋', 9812), ('배송', 9677), ('같', 8997), ('구매', 8876), ('어', 8869), ('거', 8854), ('없', 8670), ('아요', 8642), ('습니다', 8436), ('그냥', 8355), ('되', 8345), ('잘', 8029), ('않', 7984)]
    

'네요', '는데', '안', '않', '너무', '없' 등과 같은 단어들이 부정 리뷰에서 주로 등장합니다. 긍정 리뷰에 대해서도 동일하게 출력해봅시다.


```python
positive_word_count = Counter(positive_W)
print(positive_word_count.most_common(20))
```

    [('좋', 39488), ('아요', 21184), ('네요', 19895), ('어요', 18686), ('잘', 18602), ('구매', 16171), ('습니다', 13320), ('있', 12391), ('배송', 12275), ('는데', 11670), ('했', 9818), ('합니다', 9801), ('먹', 9635), ('재', 9273), ('너무', 8397), ('같', 7868), ('만족', 7261), ('거', 6482), ('어', 6294), ('쓰', 6292)]
    

'좋', '아요', '네요', '잘', '너무', '만족' 등과 같은 단어들이 주로 많이 등장합니다. 두 가지 경우에 대해서 각각 길이 분포를 확인해봅시다.


```python
# (8) 긍정 리뷰와 부정 리뷰 히스토그램 작성하기

fig,(ax1,ax2) = plt.subplots(1,2,figsize=(9,5))
text_len = train_data[train_data['label']==1]['tokenized'].map(lambda x: len(x))
ax1.hist(text_len, color='pink', edgecolor='black')
ax1.set_title('Positive Reviews')
ax1.set_xlabel('length of samples')
ax1.set_ylabel('number of samples')
print('긍정 리뷰의 평균 길이 :', np.mean(text_len))

text_len = train_data[train_data['label']==0]['tokenized'].map(lambda x: len(x))
ax2.hist(text_len, color='skyblue', edgecolor='black')
ax2.set_title('부정 리뷰')
ax2.set_title('Negative Reviews')
fig.suptitle('Words in texts')
ax2.set_xlabel('length of samples')
ax2.set_ylabel('number of samples')
print('부정 리뷰의 평균 길이 :', np.mean(text_len))
plt.show()

```

    긍정 리뷰의 평균 길이 : 13.5877381253916
    부정 리뷰의 평균 길이 : 17.02948557089084
    


    
![Review_Histogram](../../imeges/python/output_43_1.png)
    


- 긍정 리뷰보다는 부정 리뷰가 좀 더 길게 작성된 경향이 있는 것 같다. 


```python
X_train = train_data['tokenized'].values
y_train = train_data['label'].values
X_test= test_data['tokenized'].values
y_test = test_data['label'].values
```

## 정수 인코딩
- 이제 기계가 텍스트를 숫자로 처리할 수 있도록 훈련 데이터와 테스트 데이터에 정수 인코딩을 수행해야 합니다. 우선, 훈련 데이터에 대해서 단어 집합(vocaburary)을 만들어봅시다.


```python
# (9) 정수 인코딩 클래스 호출 및 X_train 데이터에 적합하기
tokenizer = Tokenizer()
tokenizer.fit_on_texts(X_train)
```

단어 집합이 생성되는 동시에 각 단어에 고유한 정수가 부여되었습니다. 이는 tokenizer.word_index를 출력하여 확인 가능합니다. 등장 횟수가 1회인 단어들은 자연어 처리에서 배제하고자 합니다. 이 단어들이 이 데이터에서 얼만큼의 비중을 차지하는지 확인해봅시다.


```python
threshold = 2
total_cnt = len(tokenizer.word_index) # 단어의 수
rare_cnt = 0 # 등장 빈도수가 threshold보다 작은 단어의 개수를 카운트
total_freq = 0 # 훈련 데이터의 전체 단어 빈도수 총 합
rare_freq = 0 # 등장 빈도수가 threshold보다 작은 단어의 등장 빈도수의 총 합

# 단어와 빈도수의 쌍(pair)을 key와 value로 받는다.
for key, value in tokenizer.word_counts.items():
    total_freq = total_freq + value

    # 단어의 등장 빈도수가 threshold보다 작으면
    if(value < threshold):
        rare_cnt = rare_cnt + 1
        rare_freq = rare_freq + value

print('단어 집합(vocabulary)의 크기 :',total_cnt)
print('등장 빈도가 %s번 이하인 희귀 단어의 수: %s'%(threshold - 1, rare_cnt))
print("단어 집합에서 희귀 단어의 비율:", (rare_cnt / total_cnt)*100)
print("전체 등장 빈도에서 희귀 단어 등장 빈도 비율:", (rare_freq / total_freq)*100)
```

    단어 집합(vocabulary)의 크기 : 39998
    등장 빈도가 1번 이하인 희귀 단어의 수: 18213
    단어 집합에서 희귀 단어의 비율: 45.53477673883694
    전체 등장 빈도에서 희귀 단어 등장 빈도 비율: 0.7935698749320282
    

단어가 약 40,000개가 존재합니다. 등장 빈도가 threshold 값인 2회 미만. 즉, 1회인 단어들은 단어 집합에서 약 45%를 차지합니다. 하지만, 실제로 훈련 데이터에서 등장 빈도로 차지하는 비중은 매우 적은 수치인 약 0.8%밖에 되지 않습니다. 아무래도 등장 빈도가 1회인 단어들은 자연어 처리에서 별로 중요하지 않을 듯 합니다. 그래서 이 단어들은 정수 인코딩 과정에서 배제시키겠습니다.

등장 빈도수가 1인 단어들의 수를 제외한 단어의 개수를 단어 집합의 최대 크기로 제한하겠습니다.


```python
# 전체 단어 개수 중 빈도수 2이하인 단어 개수는 제거.
# 0번 패딩 토큰과 1번 OOV 토큰을 고려하여 +2
vocab_size = total_cnt - rare_cnt + 2
print('단어 집합의 크기 :',vocab_size)
```

    단어 집합의 크기 : 21787
    

이제 단어 집합의 크기는 21,787개입니다. 이를 토크나이저의 인자로 넘겨주면, 토크나이저는 텍스트 시퀀스를 숫자 시퀀스로 변환합니다. 이러한 정수 인코딩 과정에서 이보다 큰 숫자가 부여된 단어들은 OOV로 변환하겠습니다.


```python
# (10) 토크나이저 클래스 호출 및 OOV 변환 코드 작성
# 코드 1
# 코드 2

tokenizer = Tokenizer(vocab_size, oov_token = 'OOV') 
tokenizer.fit_on_texts(X_train)

X_train = tokenizer.texts_to_sequences(X_train)
X_test = tokenizer.texts_to_sequences(X_test)
```

정수 인코딩이 진행되었는지 확인하고자 X_train과 X_test에 대해서 상위 3개의 샘플만 출력합니다.


```python
print(X_train[:3])
```

    [[67, 2060, 299, 14259, 263, 73, 6, 236, 168, 137, 805, 2951, 625, 2, 77, 62, 207, 40, 1343, 155, 3, 6], [482, 409, 52, 8530, 2561, 2517, 339, 2918, 250, 2357, 38, 473, 2], [46, 24, 825, 105, 35, 2372, 160, 7, 10, 8061, 4, 1319, 29, 140, 322, 41, 59, 160, 140, 7, 1916, 2, 113, 162, 1379, 323, 119, 136]]
    


```python
print(X_test[:3])
```

    [[14, 704, 767, 116, 186, 252, 12], [339, 3904, 62, 3816, 1651], [11, 69, 2, 49, 164, 3, 27, 15, 6, 1, 513, 289, 17, 92, 110, 564, 59, 7, 2]]
    

## 패딩
이제 서로 다른 길이의 샘플들의 길이를 동일하게 맞춰주는 패딩 작업을 진행해보겠습니다. 전체 데이터에서 가장 길이가 긴 리뷰와 전체 데이터의 길이 분포를 알아보겠습니다.


```python
print('리뷰의 최대 길이 :',max(len(l) for l in X_train))
print('리뷰의 평균 길이 :',sum(map(len, X_train))/len(X_train))
plt.hist([len(s) for s in X_train], bins=35, label='bins=35', color="skyblue")
plt.xlabel('length of samples')
plt.ylabel('number of samples')
plt.show()
```

    리뷰의 최대 길이 : 85
    리뷰의 평균 길이 : 15.307521459871541

    
![LengthOfReview](../../imeges/python/output_58_1.png)


리뷰의 최대 길이는 85, 평균 길이는 약 15입니다.

그리고 그래프로 봤을 때, 전체적으로는 60이하의 길이를 가지는 것으로 보입니다.


```python
def below_threshold_len(max_len, nested_list):
  count = 0
  for sentence in nested_list:
    if(len(sentence) <= max_len):
        count = count + 1
  print('전체 샘플 중 길이가 %s 이하인 샘플의 비율: %s'%(max_len, (count / len(nested_list))*100))

  
```

- 최대 길이가 85이므로 만약 80으로 패딩할 경우, 몇 개의 샘플들을 온전히 보전할 수 있는지 확인해봅시다.


```python
max_len = 80
below_threshold_len(max_len, X_train)
```

    전체 샘플 중 길이가 80 이하인 샘플의 비율: 99.99933302652553
    

훈련용 리뷰의 99.99%가 80이하의 길이를 가집니다. 훈련용 리뷰를 길이 80으로 패딩하겠습니다.


```python
X_train = pad_sequences(X_train, maxlen = max_len)
X_test = pad_sequences(X_test, maxlen = max_len)
```

# GRU로 네이버 쇼핑 리뷰 감성 분류하기


```python
from tensorflow.keras.layers import Embedding, Dense, GRU
from tensorflow.keras.models import Sequential
from tensorflow.keras.models import load_model
from tensorflow.keras.callbacks import EarlyStopping, ModelCheckpoint

embedding_dim = 100
hidden_units = 128

model = Sequential()
model.add(Embedding(vocab_size, embedding_dim))
model.add(GRU(hidden_units))
model.add(Dense(1, activation='sigmoid'))

es = EarlyStopping(monitor='val_loss', mode='min', verbose=1, patience=4)
mc = ModelCheckpoint('best_model.h5', monitor='val_acc', mode='max', verbose=1, save_best_only=True)

model.compile(optimizer='rmsprop', loss='binary_crossentropy', metrics=['acc'])
history = model.fit(X_train, y_train, epochs=15, callbacks=[es, mc], batch_size=64, validation_split=0.2)

def sentiment_predict(new_sentence):
  new_sentence = re.sub(r'[^ㄱ-ㅎㅏ-ㅣ가-힣 ]','', new_sentence)
  new_sentence = mecab.morphs(new_sentence) # 토큰화
  new_sentence = [word for word in new_sentence if not word in stopwords] # 불용어 제거
  encoded = tokenizer.texts_to_sequences([new_sentence]) # 정수 인코딩
  pad_new = pad_sequences(encoded, maxlen = max_len) # 패딩

  score = float(model.predict(pad_new)) # 예측
  if(score > 0.5):
    print("{:.2f}% 확률로 긍정 리뷰입니다.".format(score * 100))
  else:
    print("{:.2f}% 확률로 부정 리뷰입니다.".format((1 - score) * 100))
```

    Epoch 1/15
    1875/1875 [==============================] - ETA: 0s - loss: 0.2725 - acc: 0.8967
    Epoch 00001: val_acc improved from -inf to 0.91916, saving model to best_model.h5
    1875/1875 [==============================] - 54s 25ms/step - loss: 0.2725 - acc: 0.8967 - val_loss: 0.2301 - val_acc: 0.9192
    Epoch 2/15
    1875/1875 [==============================] - ETA: 0s - loss: 0.2158 - acc: 0.9213
    Epoch 00002: val_acc improved from 0.91916 to 0.92240, saving model to best_model.h5
    1875/1875 [==============================] - 43s 23ms/step - loss: 0.2158 - acc: 0.9213 - val_loss: 0.2137 - val_acc: 0.9224
    Epoch 3/15
    1875/1875 [==============================] - ETA: 0s - loss: 0.1985 - acc: 0.9289
    Epoch 00003: val_acc improved from 0.92240 to 0.92637, saving model to best_model.h5
    1875/1875 [==============================] - 44s 24ms/step - loss: 0.1985 - acc: 0.9289 - val_loss: 0.2060 - val_acc: 0.9264
    Epoch 4/15
    1873/1875 [============================>.] - ETA: 0s - loss: 0.1878 - acc: 0.9332
    Epoch 00004: val_acc did not improve from 0.92637
    1875/1875 [==============================] - 43s 23ms/step - loss: 0.1878 - acc: 0.9332 - val_loss: 0.2031 - val_acc: 0.9260
    Epoch 5/15
    1874/1875 [============================>.] - ETA: 0s - loss: 0.1783 - acc: 0.9369
    Epoch 00005: val_acc improved from 0.92637 to 0.92670, saving model to best_model.h5
    1875/1875 [==============================] - 46s 24ms/step - loss: 0.1783 - acc: 0.9369 - val_loss: 0.2030 - val_acc: 0.9267
    Epoch 6/15
    1873/1875 [============================>.] - ETA: 0s - loss: 0.1698 - acc: 0.9405
    Epoch 00006: val_acc improved from 0.92670 to 0.92764, saving model to best_model.h5
    1875/1875 [==============================] - 44s 24ms/step - loss: 0.1697 - acc: 0.9405 - val_loss: 0.2055 - val_acc: 0.9276
    Epoch 7/15
    1873/1875 [============================>.] - ETA: 0s - loss: 0.1611 - acc: 0.9436
    Epoch 00007: val_acc did not improve from 0.92764
    1875/1875 [==============================] - 44s 24ms/step - loss: 0.1610 - acc: 0.9437 - val_loss: 0.2098 - val_acc: 0.9244
    Epoch 8/15
    1875/1875 [==============================] - ETA: 0s - loss: 0.1526 - acc: 0.9473
    Epoch 00008: val_acc did not improve from 0.92764
    1875/1875 [==============================] - 44s 23ms/step - loss: 0.1526 - acc: 0.9473 - val_loss: 0.2269 - val_acc: 0.9189
    Epoch 9/15
    1875/1875 [==============================] - ETA: 0s - loss: 0.1435 - acc: 0.9507
    Epoch 00009: val_acc did not improve from 0.92764
    1875/1875 [==============================] - 44s 24ms/step - loss: 0.1435 - acc: 0.9507 - val_loss: 0.2258 - val_acc: 0.9204
    Epoch 00009: early stopping
    


```python
sentiment_predict('이 상품 진짜 싫어요... 교환해주세요')
```

    99.03% 확률로 부정 리뷰입니다.
    


```python
sentiment_predict('이 상품 진짜 좋아여... 강추합니다. ')
```

    99.51% 확률로 긍정 리뷰입니다.
    
