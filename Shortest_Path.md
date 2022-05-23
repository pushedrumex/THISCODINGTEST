# Shortest Path

가장 짧은 경로를 찾는 알고리즘

![Untitled](Shortest%20Path%20ba98b32a9aaa477695ba663941703b20/Untitled.png)

- `다익스트라(그리디)`
    
    그래프에 여러개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘(음의 간선이 없을 경우에만 동작)
    
    1. 출발노드를 설정
    2. 최단 거리 테이블 초기화
    3. **방문하지 않은 노드 중 최단 거리가 가장 짧은 노드 선택**
        
        → 한번 방문한 노드의 최단거리가 감소하지 않음 즉, 한 단계당 하나의 노드에 대한 최단거리를 확실히 찾음
        
    4. 해당 노드를 거쳐 다른 노드로 가는 거리를 계산하여 최단거리테이블 갱신
    5. 3번과 4번 반복
    
    ```python
    # 간단한 다익스트라 알고리즘
    # O(V^2)
    
    import sys
    input = sys.stdin.readline
    INF = int(1e9) # 무한대를 의미하는 값으로 10억을 설정
    
    # 노드개수, 간선개수
    n,m = map(int, input().split())
    # 시작 노드번호
    start = int(input())
    
    # 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트
    graph = [[] for i in range(n+1)]
    # 방문한 적이 있는지 체크하는 목적의 리스트
    visited = [False]*(n+1)
    # 최단 거리 테이블을 모두 무한으로 초기화
    distance = [INF]*(n+1)
    
    # 모든 간선의 정보를 입력받기
    for _ in range(m):
    	p1,p2,d = map(int,input().split())
    	graph[p1].append(p2,d))
    	# a번 노드에서 b번 노드로 가는 거리가 d라는 의미
    
    # 방문하지 않은 노드 중, 가장 최단 거리가 짧은 노드의 번호를 반환
    # 개선된 다익스트라에서 우선순위 큐 자료구조 사용
    def get_smallest_node():
    	min_value = INF
    	index = 0 # 가장 최단 거리가 짧은 노드의 인덱스
    	for i in range(1,n+1):
    		if distance[i] < min_value and not visited[i]:
    			min_value = distance[i]
    			index = i
    	return index
    
    def dijkstra(start):
    	# 시작 노드에 대해서 초기화
    	distance[start] = 0
    	visited[start] = True
    	# 시작노드와 연결되어있는 노드들과의 거리
    	for j in graph[start]:
    		distance[j[0]] = j[1]
    	# 시작 노드를 제외한 전체 n-1개의 노드에 대해 반복
    	for i in range(n-1):
    		# 현재 최단 거리가 가장 짧은 노드를 꺼내서 방문처리
    		now = get_smallest_node()
    		visited[now] = True
    		# 현재 노드와 연결된 다른 노드를 확인
    		for j in graph[now]:
    			d = distance[now] + j[1]
    			# 현재 노드를 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우
    			if d < distance[j[0]]:
    				distance[j[0]] = d
    
    # 다익스트라 알고리즘 수행
    dijkstra(start)
    
    # 모든 노드로 가기 위한 최단 거리 출력
    for i in range(1,n+1):
    	# 도달할 수 없는 경우 무한이라고 출력
    	if distance[i] == INF:
    		print("INFINITY")
    	# 도달할 수 있는 경우 거리 출력
    	else:
    		print(distance[i])
    ```
    
    > 최단거리가 가장 짧은 노드를 찾기 위해서 우선순위가 가장 높은 데이터를 가장 먼저 삭제하는 우선순위 큐 사용
    > 
    > 
    > (거리,노드)를 우선순위 큐에 넣으면 거리순으로 정렬됨 → heapq 사용
    > 
    
    ```python
    # 개선된 다익스트라 알고리즘
    # O(ElogV)
    
    import heapq
    import sys
    
    input = sys.stdin.readline
    INF = int(1e9) # 무한
    
    # 노드개수, 간선개수
    n,m = map(int,input().split())
    # 시작 노드 번호 입력받기
    start = int(input())
    # 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트
    graph = [[] for i in range(n+1)]
    # 최단거리를 무한으로 초기화
    distance = [INF]*(n+1)
    
    # 모든 간선 정보를 입력받기
    for _ in range(m):
    	p1,p2,d = map(int,input().split())
    	# p1번 노드에서 p2번 노드로 가는 거리가 d라는 의미
    	graph[p1].append((p2,d))
    
    def dijkstra(start):
    	q = []
    	# 시작 노드로 가기 위한 최단 경로는 0으로 설정하여 큐에 삽입
    	heapq.heappush(q,(0,start))
    	distance[start] = 0
    
    	while q: # q가 비어있지 않다면
    		# 거리가 가장 짧은 노드가 pop됨
    		dist,now = heapq.heappop(q)
    		# 이미 거리가 갱신된 적이 있다면 무시
    		if distance[now] < dist:
    			continue
    		for i in graph[now]:
    			d = dist+i[1]
    			# 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
    			if d < distance[i[0]]:
    				distance[i[0]] = d
    				heapq.heappush(q,(d,i[0])
    
    dijkstra(start)
    
    for i in range(1,n+1):
    		if distance[i] == INF:
    			print("INFINITY")
    		else:
    			print(distance[i])
    		
    ```
    
- `플로이드 워셜(dp)`
    
    모든 노드에 대하여 다른 모든 노드로 가는 최단 거리 정보를 찾음
    
    → 다익스트라 알고리즘과 다르게 2차원 리스트에 ‘최단거리’정보를 저장
    
    ```python
    # O(N^3)
    
    INF = int(1e9) # 무한
    
    # 노드의 개수 및 간선의 개수 입력받기
    n = int(input())
    m  = int(input())
    
    # 2차원 리스트를 만들고 모든 값을 무한으로 초기화
    graph = [[INF]*(N+1) for _ in range(n+1)]
    
    # 자기 자신으로 가는 비용은 0으로 초기화
    for a in range(1,n+1):
    	for b in range(1,n+1):
    		if a == b:
    		graph[a][b] = 0
    
    # 각 간선에 대한 정보를 입력받아 그 값으로 초기화
    for _ in range(m):
    	p1,p2,d = map(int,input().split())
    	graph[p1][p2] = d
    
    # 점화식에 따라 플로이드 워셔 알고리즘 수행
    # 모든 점을 거쳐가는 경우의 수를 모두 고려해서 가장 짧은 거리가 저장되도록
    # p1에서 k를 거쳐 p2로 이동하는 모든 경우
    for k in range(1,n+1):
    	for p1 in range(1,n+1):
    		for p2 in range(1,n+1):
    			graph[p1][p2] = min(graph[p1][p2],graph[p1][k]+graph[k][p2])
    
    # 수행된 결과를 출력
    for a in range(1,n+1):
    	for b in range(1,n+1):
    	# 도달할 수 없는 경우 무한이라고 출력
    		if graph[a][b] == INF:
    			print("INFINITY",end=" ")
    		# 도달할 수 있는 경우 거리를 출력
    		else:
    			print(graph[a][b],end=" ")
    	print()
    ```
    
- **미래도시**
    
    방문 판매원 A가 1번 위치에서 K번 회사를 방문한 뒤에 X번 회사로 가는 최소시간
    
    단, 한번 이동할 때 1만큼의 시간이 걸리고 두 회사가 연결되어있다면 양방향으로 이동가능하며 도달할 수 없다면 -1을 출력
    
    < 입력조건 >
    
    첫째줄 : 전체회사개수 N (1~100) 경로의 개수 M (1~100)
    
    둘째줄 : M개의 줄에 걸처 연결된 두 회사의 번호
    
    마지막 줄 : X 와 K (1~100)
    
    ```python
    INF = int(1e9)
    
    N,M = map(int, input().split())
    graph = [[INF]*(N+1) for _ in range(N+1)]
    
    for p1 in range(1,N+1):
        for p2 in range(1,N+1):
            if p1 == p2:
                graph[p1][p2] = 0
    
    for _ in range(M):
        p1,p2 = map(int,input().split())
    		# p1화 p2가 연결되어있다면 서로에게 가는 비용은 1
        graph[p1][p2] = 1
        graph[p2][p1] = 1
    
    X,K = map(int,input().split())
    
    # 플로이드 워셜 알고리즘
    for k in range(1,N+1):
        for p1 in range(1,N+1):
            for p2 in range(1,N+1):
                graph[p1][p2] = min(graph[p1][p2],graph[p1][k]+graph[k][p2])
    
    # 도달할 수 없는 경우 -1
    if graph[1][K] + graph[K][X] == INF:
        print(-1)
    # 도달할 수 있다면 최단거리 출력
    else:
        print(graph[1][K] + graph[K][X])
    ```
    
    전형적인 프로이드 워셜 알고리즘으로 N의 범위가 100이하기 때문에 O(N^3)로 가능
    
- **전보**
    
    N개의 도시가 있는 마을에서 C도시가 다른 도시들로 **한번에** 메시지를 전송할때, C에서 보낸 메시지를 받게되는 도시의 개수와 모든 도시들이 메시지를 받는데까지 걸리는 총 시간은?
    
    < 입력조건 >
    
    첫째줄 : 도시개수 N(1~30,000) 통로 개수 M(1~30,000) 메시지를 보내고자하는 도시 C(1~N)
    
    둘째줄 : 통로에 대한 정보 X(1~N),Y(1~N),Z(1~1,000) X에서 Y로 통로가 있으며 메시지 전달되는 시간 Z
    
    ```python
    import heapq
    INF = int(1e9)
    
    N,M,start = map(int,input().split())
    graph = [[] for _ in range(N+1)]
    distance = [INF]*(N+1)
    
    for _ in range(M):
        p1,p2,d = map(int,input().split())
        graph[p1].append((p2,d))
    
    def dijkstra(start):
        q = []
        heapq.heappush(q,(0,start))
        distance[start] = 0
        
        while q:
            dist,now = heapq.heappop(q)
            if distance[now] < dist:
                continue
            for i in graph[now]:
                d = dist+i[1]
                if d < distance[i[0]]:
                    distance[i[0]] = d
                    heapq.heappush(q,(d,i[0]))
    
    dijkstra(start)
    
    total_num = 0 # 도달할 수 있는 노드 개수
    total_dist = 0 # 도달할 수 있는 모든 마을이 메시지를 받는데 걸린 시간
    
    for i in range(1,N+1):
        if 0 < distance[i] < INF: # 나를 제외하고 도달할 수 있는 노드인 경우
            total_dist = max(total_dist,distance[i])
            total_num += 1
    print(total_num,total_dist)
    ```