#### 문제 개요

n개의 정수가 주어지고, 이를 조합하여 주어진 정수 m을 넘지 않는 최대 합을 구하는 문제

---

#### 풀이 과정

1. n개의 정수를 토대로 문제 조건(특정 수의 배수로 구성된 것만 필요)에 맞는 정수로만 구성된 새로운 리스트 생성

2. 조합 함수 구성

3. 몇개의 원소가 필요한지에는 제한이 없으므로 nCr에서 r을 기준으로 for문을 돌림

```python
# 조합 함수
r = 0
comb = [0] * r # 조합 생성할 리스트
sum_value = 0

def nCr(n, r, s, sum_value): # n개 중에 r개 고르는 조합, s는 시작범위
    global max_value
    if r == 0: # 더이상 고려할 원소가 없다는 의미
        #print(*comb)
        if sum(comb) <= m and sum(comb) > max_value:
             max_value = sum(comb)
    elif sum_value > m: # 가지치
         return
    else:
        for i in range(s, n-r+1):
            comb[r-1] = lst_a[i] # 끝자리에 원소 배정
            nCr(n, r-1, i+1, sum_value + lst_a[i])

t = int(input())
for tc in range(1, t+1):
    n, m = map(int,input().split()) #n:보석개수, m:예산
    lst = list(map(int,input().split())) # 보석 리스트

    lst_a = [] # 배수로만 구성된 보석 리스트
    for n in lst:
        if n % 4 == 0:
            lst_a.append(n)
        elif n % 6 == 0:
            lst_a.append(n)
        elif n % 7 == 0:
            lst_a.append(n)
        elif n % 9 == 0:
            lst_a.append(n)
        elif n % 11 == 0:
            lst_a.append(n)

    max_value = 0

    for i in range(1, len(lst_a)+1):
        r = i
        comb = [0] * r
        nCr(len(lst_a), r, 0, sum_value)

    print(f'#{tc} {max_value}')
```

---

#### 배운 점

1. 조합 함수에 가지치기를 더해서 구현하는 법을 익혔다. 조합함수의 재귀함수가 정확히 아직은 이해가 안가기는 하는데 대강의 느낌은 이해하겠다.

2. 문제를 풀때 자꾸 인덱스 에러가 나서 결국 시간 내에 다 못풀었는데 지금 보니까 함수 호출하면서 for문 돌릴때 lst_a라 해야하는걸 lst로 적은 참사였다. 급하게 풀다가 리스트 이름을 너무 비슷하게 지은 탓이었다. 앞으로는 리스트 만들때 이름은 확실히 구분해서 실수를 줄여야겠다.


