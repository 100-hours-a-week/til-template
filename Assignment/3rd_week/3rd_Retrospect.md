### 1번

`transform()`을 활용하였는데 `merge()`를 활용하여 작성하는 방법도 있었다. merge는 각 sales 값을 더하는 과정에서 차원이 바뀌게 되어 다시 본래 데이터의 형태로 바꾸어 주어야 하기 때문에 transform이 비교적 더 간단하게 사용이 가능한 것 같다. ([참고자료](https://m.blog.naver.com/sw4r/222392753166))

- `transform()`을 사용한 풀이
    
    ```python
    # groupby 활용하여 Year별 총 Sales를 Total_Sales 컬럼으로 추가
    df['Total_Sales'] = df.groupby('Year')['Sales'].transform('sum') # transform을 이용하여 총 매출 계산
    df
    ```
    
- `merge()`를 사용한 풀이
    
    ```python
    sum_sales = df.groupby('Year')['Sales'].sum().rename('Total_Sales').reset_index()
    new_df = df.merge(sum_sales)
    new_df
    ```
    

### 2번

처음에는 `for`문을 사용하여 아래와 같이 문제를 풀었는데, 살펴보니 `lambda`를 사용하는 방법을 알고 풀이를 변경했다. lambda를 사용하니 더 간결하게 작성이 가능했지만 익숙한 방법을 선호하다 보니, for문을 작성하였던 것 같다. lambda도 친숙하게 사용할 수 있도록 학습해야겠다.

- `for`문을 사용한 풀이
    
    ```python
    # 추출된 값을 리스트에 저장
    address_city= []
    company_name = []
    
    # for문을 사용하여 address의 city, company의 name 요소 추출
    for i in range(len(df)):
      address_city.append(df['address'][i]['city'])
      company_name.append(df['company'][i]['name'])
    
    # 리스트를 새로운 컬럼으로 추가
    df['City'] = address_city
    df['Company'] = company_name
    
    # 원하는 컬럼만 추출
    new_df = df[['id', 'name', 'username', 'email', 'City', 'Company']]
    
    # 컬럼 이름 변경
    new_df.columns = ['ID', 'Name', 'Username', 'Email', 'City', 'Company']
    new_df
    ```
    
- `lambda`를 사용한 풀이
    
    ```python
    # 새로운 데이터프레임 생성
    new_df = pd.DataFrame({
        'ID': df['id'], # 컬럼 이름 변경
        'Name': df['name'],
        'Username': df['username'],
        'Email': df['email'],
        'City' : df['address'].apply(lambda x: x['city']), # lambda 사용하여 address의 city 추출
        'Company' : df['company'].apply(lambda x: x['name'])
    })
    
    new_df
    ```
    

### 3번

시계열 데이터를 생성하는데는 큰 무리가 없었다. 다만, `freq`를 사용할 때 단순히 Month를 생각하여 `freq=‘M’`을 작성하였지만 그래프가 아래와 같이 출력되었다. 찾아보니 월의 시작 날짜는 `‘MS’`를 사용하여야 하며, 월의 마지막 날짜는 `‘M’`을 사용해야 한다는 것을 알았다.

- `freq=’M’`

![3rd_freq_M.png](https://github.com/ahyun0/2-rina-kim-til/blob/main/image/3rd_freq_M.png)

- `freq=’MS’`

![3rd_freq_MS.png](https://github.com/ahyun0/2-rina-kim-til/blob/main/image/3rd_freq_MS.png)
