- #### 문제 개요

어떤 공연장에는 가로로 C개, 세로로 R개의 좌석이 C×R격자형으로 배치되어 있다. 각 좌석의 번호는 해당 격자의 좌표 (x,y)로 표시된다. 

예를 들어보자. 아래 그림은 가로 7개, 세로 6개 좌석으로 구성된 7×6격자형 좌석배치를 보여주고 있다. 그림에서 각 단위 사각형은 개별 좌석을 나타내며, 그 안에 표시된 값 (x,y)는 해당 좌석의 번호를 나타낸다. 가장 왼쪽 아래의 좌석번호는 (1,1)이며, 가장 오른쪽 위 좌석의 번호는 (7,6)이다.

이 공연장에 입장하기 위하여 많은 사람이 대기줄에 서있다. 기다리고 있는 사람들은 제일 앞에서부터 1, 2, 3, 4, . 순으로 대기번호표를 받았다. 우리는 대기번호를 가진 사람들에 대하여 (1,1)위치 좌석부터 시작하여 시계방향으로 돌아가면서 비어있는 좌석에 관객을 순서대로 배정한다. 이것을 좀 더 구체적으로 설명하면 다음과 같다.

먼저 첫 번째 사람, 즉 대기번호 1인 사람은 자리 (1,1)에 배정한다. 그 다음에는 위쪽 방향의 좌석으로 올라가면서 다음 사람들을 배정한다. 만일 더 이상 위쪽 방향으로 빈 좌석이 없으면 오른쪽으로 가면서 배정한다. 오른쪽에 더 이상 빈자리가 없으면 아래쪽으로 내려간다. 그리고 아래쪽에 더 이상 자리가 없으면 왼쪽으로 가면서 남은 빈 좌석을 배정한다. 이 후 왼쪽으로 더 이상의 빈 좌석이 없으면 다시 위쪽으로 배정하고, 이 과정을 모든 좌석이 배정될 때까지 반복한다.

쉽게 요약하면 결국 N x N 행렬에서 시계방향으로 달팽이처럼 돌면서 1부터 번호를 붙이는 문제다. 

----

- #### 풀이 과정

while문으로 달팽이 순회를 돌리면 되는 문제이다. delta배열을 사용해서 방향을 정해주고 방향을 전환할 변수를 하나 정해서 배열의 인덱스에서 벗어나거나 이미 방문한 곳이면 방향을 전환하도록 구성하면 된다.

정답까지 한참 걸린 문제다. 예제 테스트케이스는 다 맞고 코드를 봐도 아무 문제가 없어 보이는데 자꾸 정답이 아니라고 해서, 다른 사람들이 질문 올린거를 찾다가 k가 1인 경우도 적어야 한다는 말이 있길래 수정했더니 허무하게도 바로 정답이 됐다. 

```python
c, r = map(int, input().split()) # c가로, r세로
k = int(input())
if c*r < k:
    print(0)
    exit()
if k == 1:
    print(1, 1)
arr = [[0] * (c+1) for _ in range(r+1)]

# 시작점
x = r
y = 1
i = 0 # 방향
cnt = 1 # 1씩 증가하며 입력
arr[x][y] = 1

    # 상 우 하 좌
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

while cnt < k:
    #for i in range(4): # 이거는 한칸씩 방향바꾸면서 이동할때
    nx = x+dx[i]
    ny = y+dy[i]
    if 1<=nx<r+1 and 1<=ny<c+1 and arr[nx][ny] == 0:
        x = nx
        y = ny
        cnt += 1
        arr[x][y] = cnt
        if arr[x][y] == k:
            print(y, r - x + 1)
    else:
        i = (i+1) % 4 #방향전환
```

-----

- #### 배운 점
  
  1. 문제를 꼼꼼히 봐야한다는 걸 다시 한번 느꼈다. 특히 문제에서 변수로 주어지는 값들의 range가 어떻게 되는지 확실히 보고, 내 코드에서 모든 경우의 수가 고려되었는지 한번씩 꼭 확인해야할 것 같다. 이번 문제는 정말 허무하게 k가 1인 경우 (1, 1)을 출력하도록 하는 한줄만 적었으면 바로 통과였는데, 통과까지 한참이나 걸렸다. 
  
  2. 델타 배열 사용할때 for문으로 구성하면 한번 이동할때마다 방향 전환을 계속하게 되고, 이 경우 처럼 한방향으로 쭉 가다가 전환할때는 방향 전환에 사용할 변수를 하나 생성해야 한다. 
