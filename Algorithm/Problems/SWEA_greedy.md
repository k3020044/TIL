- #### 문제 개요
  
   그림처럼 NxN 칸에 숫자가 적힌 판이 주어지고, 각 칸에서는 오른쪽이나 아래로만 이동할 수 있다.  
  
  맨 왼쪽 위에서 오른쪽 아래까지 이동할 때, 지나는 칸에 써진 숫자의 합계가 최소가 되도록 움직였다면 이때의 합계가 얼마인지 출력하는 프로그램을 만드시오.
  
  ---------

- #### 풀이 과정
  
  N x N 배열에서 (0, 0)좌표에서 시작해서 (N-1,N-1) 좌표로 오른쪽이나 아래방향 2가지 방향으로만 움직이면서 방문한 좌표들의 배열값을 더해서 그중 최소 값을 구하면 된다
  
  1. 첫번째 풀이
  
  ```python
  T = int(input())
  for tc in range(1, T+1):
      n = int(input())
      arr = [list(map(int, input().split())) for _ in range(n)]
      #    우   하
      dx = [0, 1]
      dy = [1, 0]
      # 초기 좌표
      x = 0
      y = 0
      sum_subset = arr[0][0]
      min_value = 10*n*n
  
      while x != n-1 or y != n-1: # x,y 좌표 (n-1,n-1)될 때까지 반복
  
          for i in range(2):
              nx = x + dx[i]
              ny = y + dy[i]
              print(nx, ny)
              if 0<=nx<n and 0<=ny<n: # 좌표 안에 있다면, 어차피 중복은 없음
                  # 이동하고 나서 더하기
                  x = nx
                  y = ny
                  sum_subset += arr[x][y]
                  print(sum_subset)
  
      if sum_subset < min_value:
          min_value = sum_subset
      print(f'#{tc} {min_value}')
  ```


처음에는 시계방향으로 순회하는 달팽이 문제처럼 while문에 델타배열을 섞어서 코드를 작성했는데, 이렇게 작성하면 for문이 돌아가면서 오른쪽,아래 방향으로 이동하는 것을 계속 반복하게 된다.

   

       2. 두번째 풀이

```python
def f(x, y, sum_value): # x:행 좌표, y: 열 좌표, sum_value: (x,y)좌표까지의 합
    global min_sum

    if sum_value > min_sum: #가지치기, 현재 계산한 값이 min_sum보다 적으면 중단
        return
    if x == n-1 and y == n-1: # (n-1, n-1) 좌표 도착하면 min_sum 갱신
        if sum_value < min_sum: 
            min_sum = sum_value
        return
    #    우  하
    dx = [0, 1]
    dy = [1, 0]
    for i in range(2):
        nx = x + dx[i]
        ny = y + dy[i]
        if 0<=nx<n and 0<=ny<n: #좌표 안에 있는 경우, 중복은 발생할수 없으니까 고려안함
            f(nx, ny, sum_value + arr[nx][ny]) # 다음칸으로 이동하여 함수호출
# 함수 안에 현재까지의 합 sum_value도 같이 변수로 넣어주면 깔끔하게 풀수있음            

T = int(input())
for tc in range(1, T+1):
    n = int(input()) # n: n x n
    arr = [list(map(int, input().split())) for _ in range(n)]

    min_sum = 10 * n * n # 10이하의 자연수가 n x n 배열에 있으므로
    f(0, 0, arr[0][0])
    print(f'#{tc} {min_sum}')
                    
```

1번의 한방향으로만 가는 오류를 해결하기 위해 재귀함수를 구성해서 모든 경우수를 탐색하도록했다. 그리고 함수 안에 sum_value를 변수로 지정해서, 방문한의 좌표에 대한 값들을 계속 더하면서 갱신하도록 했다. (n-1, n-1) 좌표에 도달하면 min_sum 값과 비교하여 값이 더 작은 경우 갱신하도록 했다. 

----

- #### 배운점
  
  1. global 변수를 지정하는 것의 역할과 이미를 이해할 수 있었다. global 변수로 지정하면 함수가 계속 호출하되면서도 값을 계속 사용할 수 있다. 최소값, 최대값 같은 것을 구할때 사용하면 된다.
  
  2. 가지치기의 활용. 함수에서 가지치기 조건문 하나를 추가해서 지금까지 지나온 길이 최소값이 아닌 경우 중단하도록 구성했다.
