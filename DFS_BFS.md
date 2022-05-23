# DFS/BFS

- 자료구조 기초
    
    자료구조 : 데이터를 표현하고 관리하고 처리하기 위한 구조
    
    1. 스택
        
        선입후출 append(), pop()
        
    2. 큐
        
        선입선출 popleft(), appendleft(), pop(), append()
        
        from collections import deque
        
    
    > **재귀함수**
    > 
    > 
    > 재귀함수는 가장 마지막에 호출한 함수가 먼저 수행을 끝내야 그 앞의 함수 호출이 종료된다. 즉, 스택 자료구조와 같다
    > 

<aside>
💡 **그래프**

노드와 간선으로 표현된다. 탐색문제를 보면 그래프 형태로 표현 후 고민

1. 인접행렬 : 2차원 배열로 그래프의 연결관계를 표현하는 방식, 각노드가 연결된 형태를 기록
2. 인접리스트 : 리스트로 그래프의 연결관계를 표현하는 방식, 모든 노드에 대한 정보를 차례로 연결
</aside>

![Untitled](DFS%20BFS%20c177b842be1c499299e7040cbb8af876/Untitled.jpeg)

- `DFS`
    
    깊이우선탐색. 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘으로 특정한 경로로 탐색하다가 특정한 상황에서 최대한 깊숙이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로를 탐색하는 알고리즘. 스택
    
    - 구체적인 동작과정
        
        일반적으로 인접한 노드중 방문하지지 않은 노드가 여러개라면 번호가 낮은 순서부터 처리
        
        1. 탐색 시작 노드를 스택에 삽입하고 방문 처리
        2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있다면 그 인접 노드를 스택에 넣고 방문 처리, 방문하지 않은 인접노드가 없다면 스택 최상단 노드를 꺼냄
        3. 2번 과정을 반복
    
    ```python
    # DFS
    # 1 2 7 6 8 3 4 5
    def dfs(graph, v, visited):
    	# 현재 노드 방문처리
    	visited[v] = True
    	print(v,end=" ")
    	# 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    	for i in graph[v]:
    		if visited[i] == False:
    			dfs(graph,i,visited)
    
    # 각 노드가 방문된 정보를 리스트 자료형으로 표현
    visited = [False]*9
    # 각 노드가 연결된 정보를 리스트 자료형으로 표현
    graph = [[],[2,3,8],[1,7],[1,4,5],[3,5],[3,4],[7],[6,],[1,7]]
    
    # 정의된 DFS함수 호출
    dfs(graph, 1,visited)
    ```
    
    O(N), 스택을 이용하는 알고리즘이기 때문에 실제 구현은 재귀함수를 이용했을 때 간결
    
- `BFS`
    
    너비우선탐색. 가까운 노드부터 탐색하는 알고리즘으로 인접한 노드를 반복적으로 큐에 넣고 먼저 들어온 것이 먼저 나가게되어 가까운 노드부터 탐색하는 알고리즘. 큐
    
    - 구체적인 동작과정
        1. 탐색시작 노드를 큐에 삽입 후 방문처리
        2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입 후 방문처리
        3. 2번 과정 반복
    
    ```python
    # BFS
    # 1 2 3 8 7 4 5 6
    from collections import deque
    def bfs(graph, start, visited):
    	# 현재 노드 방문 처리
    	visited[start] = True
    	# 큐 구현을 위해 deque라이브러리 사용
    	queue = deque()
    	queue.append(start) # deque([start])
    	# 큐가 빌 때까지 반복
    	while queue:
    		# 큐에서 하나의 원소를 뽑아 출력
    		v = queue.popleft()
    		print(v, end=" ")
    		# 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입
    		for i in graph[v]:
    			if visited[i] == False:
    				queue.append(i)
    				visited[i] = True
    				
    visited = [False]*9
    graph = [[],[2,3,8],[1,7],[1,4,5],[3,5],[3,4],[7],[6,8],[1,7]]
    bfs(graph, 1, visited)
    ```
    
    O(N), 큐 자료구조에 기초하는 알고리즘이며 실제 수행 시간은 DFS보다 빠름
    
- **음료수 얼려먹기** dfs
    
    N*M 얼음틀(0:구멍, 1:칸막이) 구멍이 뚫려있는 부분끼리 상하좌우 붙어있는 경우 서로 연결되어 있는 것으로 간주한다. 얼음 틀의 모양이 주어졌을 때, 생성되는 총 아이스크림의 개수는?
    
    < 입력조건 >
    
    첫째줄 : 세로 N, 가로 M (1≤N,M≤1000)
    
    둘째줄 : 얼음틀의 형태가 주어짐
    
    ```python
    # 입력 받기
    N,M = map(int,input().split())
    ice = []
    for _ in range(N):ice.append(list(map(int, input())))
    # DFS로 특정 노드를 방문한 뒤 이 노드와 연결된 모든 노드들 방문
    def dfs(x,y):
    		# 주어진 범위를 벗어난다면 종료
        if x == -1 or y == -1 or x == N or y == M:return
        if ice[x][y] == 0: # 방문하지 않은 곳이라면
            ice[x][y] = 1 # 방문처리
    				# 상하좌우노드 모두 재귀적으로 호출 
            dfs(x-1,y)
            dfs(x+1,y)
            dfs(x,y-1)
            dfs(x,y+1)
    
    # 얼음의 개수
    result = 0
    # 모든 노드를 확인하며 1로 바꿈
    for i in range(N):
        for j in range(M):
            if ice[i][j] == 0: # 방문하지 않은 곳이라면
    	          result += 1 # 얼음 개수 +
                dfs(i,j) # 이 노드와 연결된 노드들 다 1로 변경
    print(result)
    ```
    
    **BFS** (1,1) → (2,1) → (1,2) → (2,2) → (2,3), (1,5), (4,1) → (4,2) → (4,3) → (4,4)
    
- **미로 탈출** bfs
    
    N*M의 미로((1,1) : 동빈 위치, (N,M) : 출구, 0 : 괴물, 1 : 괴물 없음)에서 동빈이가 탈출하기 위해 움직여야하는 최소 칸의 개수는? (시작칸과 마지막은 항상 1)
    
    < 입력조건 >
    
    첫째줄 :  세로 N, 가로 M (1≤N,M≤200)
    
    둘째줄 : 미로의 정보
    
    ```python
    from collections import deque
    # 입력받기
    N,M = map(int, input().split())
    miro = []
    for _ in range(N):miro.append(list(map(int,input())))
    # 상하좌우
    dx =[-1,+1,0,0]
    dy = [0,0,-1,+1]
    # BFS 소스코드 구현
    def bfs(x,y):
    		# 큐 구현을 위해 deque라이브러리 사용
        queue = deque()
        queue.append((x,y))
    		# 큐가 빌 때까지 반복
        while queue:
    				# 현재 노드
            x,y = queue.popleft()
    				# 상하좌우
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
    						# 범위를 벗어나거나 괴물인 경우 무시
                if nx == -1 or ny == -1 or nx == N or ny == M or miro[x][y] == 0:continue
    						# 해당 노드를 처음 방문하는 경우(1)에만 최단거리 기록
                if miro[nx][ny] == 1:
                    miro[nx][ny] = miro[x][y] + 1
                    queue.append((nx,ny))
    		# 가장 오른쪽 아래까지의 최단 거리 반환
        return (miro[N-1][M-1])
        
    print(bfs(0,0))
    ```