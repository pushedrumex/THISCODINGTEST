# Graph

노드와 노드 사이에 연결된 간선의 정보를 가지고 있는 자료구조

< 그래프의 구현방법 >

1. 인접 행렬 : 2차원 배열을 사용하는 방식 → O(V^2), 플로이드 워셜
2. 인접 리스트 : 리스트를 사용하는 방식 → O(E), 다익스트라

> 최단 경로를 찾아야하는 문제가 출제 되었을 때, 노드의 개수가 적은 경우에는 플루이드 워셜 알고리즘을, 노드와 간선 개수가 모두 많으면 다익스트라 알고리즘을 이용하면 유리
> 
- `트리`
    
    부모에서 자식으로 내려오는 계층적인 모델
    
    ex) 최소 힙은 항상 부모 노드가 자식 노드보다 크기가 작은 자료구조로서 트리자료구조에 속함
    
    ![Untitled](Graph%203999c647a4554d8998f67ccfd4851f98/Untitled.png)
    
- `서로소 집합 자료구조`
    
    서로소 부분 집합(공통 원소가 없는 두 집합)들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조로 union과 find 2개의 연산으로 조작 가능, 트리자료구조로 집합을 표현
    
    - union : 2개의 원소가 포함된 집합을 하나의 집합으로 합치는 연산
        
        두 노드의 루트노드 중에서 더 번호가 작은 원소가 부모노드가 되도록 구현
        
        → 번호가 큰 노드가 번호가 작은 노드를 간선으로 가리키도록 트리구조를 이용
        
    - find : 특정한 원소가 속한 집합이 어떤 집합인지 알려주는 연산(=루트노드)
    
    ![Untitled](Graph%203999c647a4554d8998f67ccfd4851f98/Untitled%201.png)
    
    ![Untitled](Graph%203999c647a4554d8998f67ccfd4851f98/Untitled%202.png)
    
    {1,2,3,4} {5,6}으로 두 집합으로 나눠짐
    
    1. 서로소 집합 알고리즘 union, find
        
        ```python
        # 특정 원소가 속한 집합을 찾기
        # 경로압축기법 활용
        def find_parent(parent,x):
        	# 루트 노드가 아니라면, 루트노드를 찾을 때까지 재귀적으로 호출
        	if parent[x] != x:
        		parent[x] = find_parent(parent,parent[x])
        	return parent[x] # 현상태의 루트노드 반환
        
        # 두 원소가 속한 집합을 합치기
        def union_parent(parent,a,b):
        	a = find_parent(parent,a)
        	b = find_parent(parent,b)
        	if a<b:
        		parent[b] = a
        	else:
        		parent[a] = b
        
        # 노드의 개수와 간선(union)의 개수 입력받기
        v,e = map(int,input().split())
        parent = [0]*(v+1)
        
        # 부모 테이블상에서, 부모를 자기 자신으로 초기화
        for i in range(1,v+1):
        	parent[i] = i
        
        # union 연산을 각각 수행
        for i in range(e):
        	a,b = map(int,input().split())
        	union_parent(parent,a,b)
        
        # 각 원소가 속한 집합 출력
        print('각 원소가 속한 집합: ',end=" ")
        for i in range(1,v+1)
        	print(find_parent(parent,i),end=" ")
        print()
        
        # 부모테이블(=집합=루트노드) 내용 출력
        print('부모 테이블: ',end=' ')
        for i in range(1,v+1)
        	print(parent[i],end=' ')
        ```
        
    2. 서로소 집합을 활용한 사이클 판별
        
        간선 방향성이 없는 무방향 그래프 내에서 사용 가능 → 루트노드가 서로 같다면 사이클이 발생한 것 
        
        ```python
        # 특정 원소가 속한 집합을 찾기
        # 경로압축기법 활용
        def find_parent(parent,x):
        	# 루트 노드가 아니라면, 루트노드를 찾을 때까지 재귀적으로 호출
        	if parent[x] != x:
        		parent[x] = find_parent(parent,parent[x])
        	return parent[x] # 현상태의 루트노드 반환
        
        # 두 원소가 속한 집합을 합치기
        def union_parent(parent,a,b):
        	a = find_parent(parent,a) # 루트노드
        	b = find_parent(parent,b) # 루트노드
        	if a<b:
        		parent[b] = a
        	else:
        		parent[a] = b
        
        # 노드의 개수와 간선(union)의 개수 입력받기
        v,e = map(int,input().split())
        parent = [0]*(v+1)
        
        # 부모 테이블상에서, 부모를 자기 자신으로 초기화
        for i in range(1,v+1):
        	parent[i] = i
        
        cycle = False # 사이클 발생 여부
        
        for i in range(e):
        	a,b = map(int,input().split())
        	# 사이클이 발생한 경우(루트노드가 동일한 경우) 종료
        	if find_parent(parent,a) == find_parent(parent,b):
        		cycle = True
        		break
        	# 사이클이 발생하지 않았다면 합집합 수행
        	else:
        		union_parent(parent,a,b)
        
        if cycle:
        	print("사이클이 발생했습니다.")
        else:
        	print("사이클이 발생하지 않았습니다.")
        ```
        
- `크루스칼 알고리즘`
    
    신장트리 중 최소비용으로 만들 수 있는 신장트리를 찾는 알고리즘 즉, 가장 적은 비용으로 모든 노드를 연결
    
    1. 간선 데이터를 비용에 따라 오름차순으로 정렬
    2. 간선을 하나씩 확인해가며 현재의 간선이 사이클을 발생시키는지 확인
        - 사이클이 발생하지 않는 경우 최소 신장 트리에 포함
        - 사이클이 발생하는 경우 최소 신장 트리에 포함하지 않음
    3. 모든 간선에 대해 2번 과정을 반복
    
    → 최종적으로 신장 트리에 포함되는 간선의 개수는 ‘노드개수 - 1’
    
    > **신장트리**
    > 
    > 
    > 하나의 그래프가 있을 때, 모든 노드를 포함하면서 사이클이 존재하지 않는 부분 그래프
    > 
    > ![Untitled](Graph%203999c647a4554d8998f67ccfd4851f98/Untitled.jpeg)
    > 
    
    ```python
    # 가장 거리가 짧은 간선부터 차례대로 집합에 추가
    # 단, 사이클을 발생시키는 간선은 제외하고 연결
    # O(ElogE)
    
    # 특정 원소가 속한 집합을 찾기
    def find_parent(parent,x):
    	# 루트노드가 아니라면 루트노드를 찾을 때까지 재귀적으로 호출
    	if parent[x] != x:
    		parent[x] = find+parent(parent,parent[x])
    	return parent[x] # 루트노드 반환
    
    # 두 원소가 속한 집합을 합치기
    def union_parent(parent,a,b):
    	a = find_parent(parent,a)
    	b = find_parent(parent,b)
    	if a>b:
    		parent[b] = a
    	else:
    		parent[a] = b
    
    # 노드의 개수와 간선의 개수 입력받기
    v,e = map(int,input().split())
    parent = [0]*(v+1)
    
    # 모든 간선의 정보를 담을 리스트와 최종 비용을 담을 변수
    edges = []
    result = 0
    
    # 부모 테이블상에서, 부모를 자기 자신으로 초기화
    for i in range(1,v+1)
    	parent[i] = i
    
    # 모든 간선에 대한 정보 입력받기
    for _ in range(e):
    	a,b,cost = map(int,input().split()
    	# 비용 순으로 정렬하기 위해서 튜플의 첫번째 원소를 비용으로 설정
    	edges.append((cost,a,b))
    
    # 간선을 비용 순으로 정렬
    edges.sort()
    
    # 간선을 하나씩 확인하며
    for edge in edges:
    	cost,a,b = edge
    	# 사이클이 발생하지 않는 경우에만 집합에 포함
    	if find_parent(parent,a) != find_parent(parent,b) # 루트노드가 다를 경우에만
    		union_parent(parent,a,b)
    		result += cost
    
    print(result)
    ```
    
- `위상 정렬 알고리즘`
    
    방향 그래프의 모든 노드를 방향성에 거스르지 않고 순서대로 나열하는 것 즉, 순서가 정해져 있는 일련의 작업을 차례대로 수행해야 할 때 사용할 수 있는 알고리즘
    
    1. 진입차수(특정한 노드로 들어오는 간선의 개수)가 0인 노드를 큐에 넣는다.
    2. 큐가 빌 때까지 다음의 과정을 반복한다.
        - 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.
        - 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.
    
    → 큐가 빌 때까지 원소를 꺼내서 처리하는 과정을 반복하며, 모든 원소를 방문하기 전 큐가 빈다면 사이클이 존재하는 것으로 판단
    
    ```python
    # 큐에서 빠져나간 노드를 순서대로 출력하면 그것이 바로 위상정렬임
    # 큐에 새롭게 들어가는 원소가 2개 이상이라면 여러가지 답이 존재하게 됨 -> 입력 순서에 따라 달라짐
    # O(V+E)
    
    from collections import deque
    
    # 노드의 개수와 간선의 개수 입력받기
    v,e = map(int,input().split())
    
    # 모든 노드에 대한 진입 차수는 0으로 초기화
    indegree = [0]*(v+1)
    
    # 각 노드에 연결된 간선 정보를 담기 위한 연결 리스트(그래프) 초기화
    graph = [[] for i in range(v+1)]
    
    # 방향그래프의 모든 간선 정보를 입력받기
    for _ in range(e):
    	a,b = map(int,input().split())
    	graph[a].append(b) # a에서 b로 이동 가능 a->b a는 b의 선수과목
    	indegree[b] += 1 # 진입차수 1 증가
    
    # 위상 정렬 함수
    def topology_sort():
    	result = [] # 알고리즘 수행 결과를 담을 리스트
    	q = deque() # 큐 기능을 위한 deque 사용
    	
    	# 처음 시작할 때는 진입차수가 0인 노드를 큐에 삽입
    	for i in range(1,v+1):
    		if indegree[i] == 0:
    			q.append(i)
    	
    	# 큐가 빌 때까지 반복
    	while q:
    		# 큐에서 원소 꺼내기
    		now = q.popleft()
    		result.append(now)
    		# 해당 원소와 연결된 노드들의 진입차수에서 1 빼기
    		for i in graph[now]:
    			indegree[i] -= 1
    			# 새롭게 진입차수가 0이 되는 노드를 큐에 삽입
    			if indegree[i] == 0
    				q.append(i)
    
    	# 위상 정렬을 수행한 결과 출력
    	for i in result:
    		print(i,end=' ')
    
    topology_sort()
    ```
    
- **팀결성**
    
    N+1 명의 학생들(0~N)이 N+1팀으로 구분되어 존재한다. 선생님이 ‘팀 합치기’와 ‘같은팀 여부 확인’ 연산을 사용할 수 있다. 선생님이 M개의 연산을 수행할 때 ‘같은 팀 여부 확인’연산에 대한 결과를 출력해라
    
    < 입력조건 >
    
    첫째줄 : N,M (1~100,000)
    
    둘째줄 : M개의 연산 
    
    - 팀합치기 : 0 a b = a번 학생이 속한 팀과 b번 학생이 속한 팀을 합친다
    - 같은 팀 여부 확인 : 1 a b = a번 학생과 b번 학생이 같은 팀에 속해있는지 확인
    
    ```python
    N,M = map(int,input().split())
    parent = [0]*(N+1)
    
    for i in range(N+1):
        parent[i] = i
    
    def find_parent(parent,x):
        if parent[x] != x:
            parent[x] = find_parent(parent,parent[x])
        return parent[x]
    
    def union_parent(parent,a,b):
        a = find_parent(parent,a)
        b = find_parent(parent,b)
        if a > b:parent[a] = b
        else:parent[b] = a
    
    for _ in range(M):
        cal,a,b = map(int,input().split())
        if cal == 0:
            union_parent(parent,a,b)
        else:
            if find_parent(parent,a) == find_parent(parent,b):
                print('Yes')
            else:
                print('NO')
    ```
    
- **도시분할계획**
    
    이 마을은 N개의 집과 그 집들을 연결하는 M개의 길로 이루어져 있다. 이 길은 양방향으로 다닐 수 있으며 각각 유지비가 있다. 이 마을을 2개의 분리된 마을로 분할한다. 분리된 마을안에 집들인 서로 연결되어야하며 마을에는 집이 하나 이상 있어야한다. 분리된 두 마을 사이의 길들은 필요가 없다.
    
    마을을 분리할 때, 불필요한 길을 최대한 없애고 유지비의 합이 최소가 되도록 하는 프로그램을 작성해라.
    
    < 입력조건 >
    
    첫째줄 : N(2~100,000),M(1~1,000,000)
    
    둘째줄 : M줄에 걸쳐 길의 정보가 주어진다. A,B,C = A번 집과 B번 집을 연결하는 길의 유지비는 C
    
    ```python
    # 일단 마을에 대해 크루스칼 알고리즘을 사용하고, 그 중 가장 비용이 큰 간선을 끊으면 될 듯?
    
    def find_parent(parent,x):
        if parent[x] != x:
            parent[x] = find_parent(parent,parent[x])
        return parent[x]
    
    def union_parent(parent,A,B):
        A = find_parent(parent,A)
        B = find_parent(parent,B)
        if A>B:parent[A] = B
        else:parent[B]=A
    
    N,M = map(int,input().split())
    parent = [0]*(N+1)
    edges = []
    result = 0
    last = 0 # 신장트리를 이루는 간선 중 가장 비용이 큰 간선의 비용
    
    for i in range(1,N+1):
        parent[i] = i
    
    for _ in range(M):
        A,B,C = map(int,input().split())
        edges.append((C,A,B))
    edges.sort()
    
    for edge in edges:
        C,A,B = edge
        if find_parent(parent,A) != find_parent(parent,B):
            union_parent(parent,A,B)
            result += C
            last = C # 정렬을 진행했기 때문에 마지막에 선택한 간선이 가장 큰 비용
    
    print(result-last)
    ```
    
- **커리큘럼**
    
    동빈이는 N개의 강의를 듣는다. 이 강의들은 선수과목이 있을 수 있으며 동시에 여러개의 강의를 들을 수 있다. 동빈이가 듣고자 하는 N개의 강의 정보(강의 시간 선수강의 -1)가 주어졌을 때, N개의 강의에 대하여 수강하기까지 걸리는 최소시간을 각각 출력해라
    
    < 입력조건 >
    
    첫째줄 : N(1~500)
    
    둘째줄 : 강의 시간(1~100,000)과 선수과목들의 번호가 자연수로 주어지며 각 줄은 -1로 끝난다.
    
    ```python
    # 위상정렬 알고리즘 사용
    # 인접한 노드에 대하여 현재보다 강의시간이 더 긴 경우가 있다면 그 값으로 갱신
    
    from collections import deque
    
    N = int(input())
    indegree = [0]*(N+1) # 진입차수
    graph = [[] for _ in range(N+1)] # 연결리스트
    time = [0]*(N+1)
    
    for i in range(1,N+1):
    	info = list(map(int,input().split()))
    	time[i] = info[0]
    	for j in info[1:-1]:
    		indegree[i] += 1
    		graph[j].append(i) # j는 i의 선수과목 j->i
    	
    def topology_sort():
    	result = time[:]
    	q = deque()
    
    	for i in range(1,N+1):
    		if indegree[i] == 0:
    			q.append(i)
    
    	while q:
    		now = q.popleft()
    		for i in graph[now]:
        # now가 선수과목인 과목들
    			result[i] = max(result[i],result[now]+time[i])
    			# 현재 i의 시간보다 시간이 더 긴 경우가 있다면 그 값을 저장
    			indegree[i] -= 1
    			if indegree[i] == 0:
    				q.append(i)
    	for i in range(1,N+1):
    		print(result[i])
    
    topology_sort()
    ```