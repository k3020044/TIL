#### 문제 개요

N x N 행렬에 정수가 입력되어 있고, 2차원 배열을 탐색하면서 주어진 정수 K에 대하여 K x K 인 정사각형의 모서리 부분에 입력된 정수들의 합의 최대값을 구하는 문제

파리퇴치 문제는 K x K 정사각형에 속한 모든 정수의 합을 구해야 했다면, 이번에는 모서리 부분만 구하는 거여서 K x K 정사각형에서 (K-2) x (K-2) 정사각형을 빼는 과정을 추가하면 된다. 

---

#### 풀이 과정

for문으로 시작점 좌표 i, j를 찾고, 또 그 안에 for문을 사용해서 K x K 정사각형의 합을 구한다. 그리고 작은 사각형 부분의 합을 빼주고, max_value 값과 비교하여 최대값을 갱신해준다.

나는 원래 풀던대로 풀었는데 이상하게 i = 0 인 부분만 계산되고, 그 다음 i에 대해서는 밑에 있는 for문이 돌아가지 않았다. 아직도 이해가 안가는 부분.

그래서 한시간 반정도를 고민하며 날리면서, 결국 모서리 합을 구하는 부분을 따로 함수로 구현해서 풀었는데 이렇게 하면 또 돌아간다. 왜 그런걸까?

1. for문이 다 안돌아가는 코드
   
   ```python
   
   t = int(input())
   for tc in range(1, t+1):
       n, k = map(int, input().split())
       arr = [list(map(int, input().split())) for _ in range(n)]
   
       max_value = 0
       for x in range(n-k+1): # x = 0일때만 계산된
           for y in range(n-k+1):
               sum_value = 0
               for n in range(k):
                   for m in range(k):
                       sum_value += arr[x+n][y+m]
   
               for i in range(1, k-1):
                   for j in range(1, k-1):
                       sum_value -= arr[x+i][y+j]
   
               if sum_value > max_value:
                   max_value = sum_value
   
       print(f'#{tc} {max_value}')
   
   ```
   
   
   
   

2.  함수 활용한 코드
   
   ```python
   
   def values(i, j):
       global max_value
       sum_value = 0
       for n in range(k):
           for m in range(k):
               sum_value += arr[i+n][j+m]
       for x in range(1, k-1):
           for y in range(1, k-1):
               sum_value -= arr[i+x][j+y]
       if sum_value > max_value:
           max_value = sum_value
   
   t = int(input())
   for tc in range(1, t+1):
       n, k = map(int, input().split())
       arr = [list(map(int, input().split())) for _ in range(n)]
   
       max_value = 0
       for i in range(n-k+1):
           for j in range(n-k+1):
               #print(i, j)
               values(i, j)
   
       print(f'#{tc} {max_value}')
   ```
   
   
   

---

#### 느낀 점

아무리 봐도 내가 예전에 파리퇴치 문제 푼 코드를 봐도 틀린게 없는데 왜 안됐을까? 이럴 때 정말 답답함을 느낀다. 똑같은 logic으로 방식만 바꿔서 구현했을 때는 잘돌아가는데 말이다. 어쨌든 열심히 풀었고, 결국은 풀었다. 


