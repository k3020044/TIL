- #### 문제 개요
  
   루트 없는 트리가 주어진다. 이때, 트리의 루트를 1이라고 정했을 때, 각 노드의 부모를 구하는 프로그램을 작성하시오.
  
  ---

- #### 풀이 과정
  
  보통 트리 문제는 INPUT이 부모, 자식 순으로 주어지는데 이 문제는 부모, 자식 구분이 없이 연결 관계로만 주어져서 처음에는 이걸 어떻게 트리로 구현하는지 고민했다. 그래서 결국 구글링을 했는데 이런 이유로 DFS 순회로 풀면 되는 문제였다. 루트가 1로 지정돼있기 때문에 배열에서 1부터 순회를 돌면서 부모 값을 저장해주면 된다.
  
  ```python
  from collections import deque
  import sys
  input = sys.stdin.readline
  
  n = int(input().rstrip())
  g = [[] for _ in range(n+1)]
  p = [0 for _ in range(n+1)]
  
  for _ in range(n-1):
      a,b = map(int , input().rstrip().split())
      g[a].append(b)
      g[b].append(a)
  
  def bfs():
      q = deque()
      q.append(1)
  
      while q:
          node = q.popleft()
          for i in g[node]:
              if p[i] == 0:
                  p[i] = node
                  q.append(i)
  
  bfs()
  
  for i in range(2,n+1):
      print(p[i])
  ```
  
  

---    

- #### 배운점
  
  1. 재귀로 함수를 구성할때 recursionerror가 떴는데, 이거는 기본적으로 재귀가 1000번까지 연산이 가능해서 그 회수를 초과하면 뜨는 에러다. 이때는 import 라인에서 연산 횟수를 지정하는 코드를 넣으면 된다
  
  2. dfs, bfs, 트리 순회는 역시나 여러번 계속 반복하면서 공부해야겠다
