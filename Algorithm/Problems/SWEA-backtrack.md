- #### 문제 개요

A사는 여러 곳에 공장을 갖고 있다. 봄부터 새로 생산되는 N종의 제품을 N곳의 공장에서 한 곳당 한가지씩 생산하려고 한다.  

각 제품의 공장별 생산비용이 주어질 때 전체 제품의 최소 생산 비용을 계산하는 프로그램을 만드시오.

주어진 2차원 배열에서 각행, 열별로 중복되지 않게 하나씩 방문하면서 최소값을 찾는 문제

-----

- #### 풀이 과정
  
  1. 첫번째 풀이
     
     처음에는 인덱스를 순열로 구해서 순열을 완성한 후에 sum을 하도록 순열 함수를 변형한 함수를 만들었다. 그런데 이렇게 하면 순열을 다 만든 후에 sum을 한꺼번에 하게되어 시간 초과가 발생했다
     
     ```python
     def perm(i, k): # i = 선택된 원소 개수, k = 원소 개수
         global min_value
         if i == k:
             sum_value = 0
             for m in range(k):
                 sum_value += arr[m][p[m]]
                 if sum_value > min_value:
                     return
             if sum_value < min_value:
                 min_value = sum_value
         else:
             for j in range(i, k):
                 p[i], p[j] =  p[j], p[i]
                 perm(i+1, k)
                 p[i], p[j] = p[j], p[i] # 원상 복귀
      
     T = int(input())
     for tc in range(1, T+1):
         n = int(input())
         arr = [list(map(int, input().split())) for _ in range(n)]
         p = []
         for i in range(n):
             p.append(i)
         min_value = 99999999999
         perm(0, n)
         print(f'#{tc} {min_value}')
     ```
  
  2. 두번째 풀이
     
     탐색을 하면서 sum도 같이 해나가서, min 값보다 값이 커지면 sum을 중단하는 가지치기를 포함하는 함수를 구성했다. 그래서 열을 인덱스로 하는 visited 배열을 따로 만들어서 방문한 열에 대해 visited 표시를 하도록 하고, 지금까지의 sum을 함수의 변수로 넣어서 계속 가지고 다니게끔 했다.
     
     ```python
     def backtrack(row, N, now_sum, visited):
         global min_sum
         if row == N:  # 모두 탐색된 경우
             if now_sum < min_sum:  # 최소값 갱신
                 min_sum = now_sum
         elif now_sum > min_sum:
             return  # 지금까지의 합이 min_sum보다 크면 돌아가도록
         else:
             for col in range(N):  # 아직 방문하지 않은 열인경우
                 if visited[col] == 0:
                     visited[col] = 1  # 방문 처리함
                     backtrack(row + 1, N, now_sum + arr[row][col], visited)
                     visited[col] = 0  # 함수 호출하고 나서 초기화해줌
      
      
     T = int(input())
     for tc in range(1, T + 1):
         N = int(input())
         arr = [list(map(int, input().split())) for _ in range(N)]
         min_sum = 9999999999  # 임의의 값
         visited = [0] * N
         backtrack(0, N, 0, visited)  # 함수 호출
         print(f'#{tc} {min_sum}')
     ```





-----

- #### 배운 점
  
  1. 순열, dfs, bfs 등 재귀함수로 구현되는 순회 문제들에 대한 감을 조금 더 익힐 수 있었다. 아직 완벽하지는 않지만 문제를 더 풀다보면 점점 더 완성될 것 같다! 요즘은 그래도 예전보다 문제가 잘 풀려서 기분이 좋다. 이렇게 점점 더 발전해야지! 아 그래서 배운 점은 가지치기 활용법이다!
