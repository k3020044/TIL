- #### 문제 개요
  
  N개의 정점을 가진 완전 이진 트리 T1이 있다.
  
  T1의 루트부터 전위 순회, 중위 순회, 후위 순회에 대해 방문 순서를 찾고,  각 순회의 n번째 방문 정점 번호 중 가장 큰값을 새로운 완전 이진 트리 T2의 n번 정점 값으로 저장한다.
  
  T2를 중위 순회하며 각 정점에 저장된 값을 출력한다.

----

- #### 풀이 과정
  
  1. 전위 순회, 중위 순회, 후위 순회 함수 구성하면서 각각 리스트 생성
  
  2. 생성된 리스트에서 각 인덱스별 최대값을 구해서 정답으로 순회할 새로운 배열 생성
  
  3. 새로운 배열을 중위 순회하면서 print
     
     ```python
     # 전위 순회
     def preorder(n):
         if n <= N: # 0이 아니라면
             pre.append(n) # pre 리스트에 저장
             preorder(2*n) # 왼쪽 자식
             preorder(2*n +1) # 오른쪽 자식
     
     # 중위 순회
     def inorder(n):
         if n <= N:
             inorder(2*n)
             ino.append(n)
             inorder(2*n +1)
     
     # 후위 순회
     def postorder(n):
         if n <= N:
             postorder(2*n)
             postorder(2*n +1)
             post.append(n)
     
     # 정답 출력할 중위순회 함수
     def new_inorder(n):
         if n <= N:
             new_inorder(2*n)
             print(lst[n], end=' ')
             new_inorder(2*n + 1)
     
     T = int(input())
     for tc in range(1, T+1):
         N = int(input())
         # 트리 구조로 탐색할 0~n까지의 1차원배열
         arr = []
         for i in range(N+1):
             arr.append(i)
         # 전위, 중위, 후위 순회 후 리스트 저장
         pre = []
         ino = []
         post = []
         preorder(1)
         inorder(1)
         postorder(1)
     
         # 최대값으로 저장한 새로운 배열 생성
         lst = [0] # 인덱스 맞춰야하니까 0 추가
         for i in range(N):
             lst.append(max(pre[i], ino[i], post[i]))
     
         print(f'#{tc}', end=' ')
         new_inorder(1)
         print()
     ```





------

- #### 배운 점
  
  1. 어제 공부를 제대로 안했더니 그새 순회 함수 잊어버려서 결국 못풀었다. 문제가 여러 단계를 거쳐야해서 조금 복잡해보일뿐 결국 중위순회,전위순회,후위순회 함수만 구성할 수 있으면 어렵지 않게 풀수 있는 문제였다. 시험 끝나고 필기한거 보고 풀어보면서 다시 한번 익혔다. 이제는 안 잊어버리겠지?
  
  2. 트리도 결국은 1차원 배열을 순회하는 하나의 방법임을 인식하고 문제를 풀어가야 겠다. 







----

- #### 배운 점
