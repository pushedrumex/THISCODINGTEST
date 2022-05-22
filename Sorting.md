# Sorting

데이터를 특정한 기준에 따라서 순서대로 나열하는 것

- `선택정렬`
    
    가장 작은 수를 선택해 맨 앞에 있는 데이터와 바꾸는 것을 반복
    
    ```python
    array = [7, 5, 9, 0 , 3, 1, 6, 2, 4, 8]
    
    for i in range(len(array)):
    	min_index = i # 가장 작은 원소의 인덱스
    	for j in range(i+1,len(array)):
    		if array[min_index] > array[j]:
    			min_index = j
    	array[i], array[min_index] = array[min_index], array[i] # 스와프
    ```
    
    - 시간복잡도
        
        O(N^2), 비효율적인 정렬
        
- `삽입정렬`
    
    데이터를 하나씩 확인하며, 각 데이터를 적절한 위치에 삽입(특정한 데이터가 삽입되기 이 전에, 그 앞까지의 데이터는 이미 정렬이 되어있다고 가정하고 삽입 될 데이터보다 작은 데이터를 만나면 그 위치에서 삽입)
    
    ```python
    array = [7, 5, 9, 0 , 3, 1, 6, 2, 4, 8]
    
    for i in range(1, range(len(array)):
    	for j in range(i, 0, -1): # i~1 (감소)
    		if array[j-1] > array[j]: # 한 칸씩 왼쪽으로 이동
    			array[j-1], array[j] = array[j], array[j-1]
    		else: # 나보다 작은 데이터를 만나면 break
    			break
    ```
    
    - 시간복잡도
        
        O(N^2), 보통은 비효율적이나 정렬이 거의 되어있는 상태라면 퀵정렬 알고리즘보다 더 강력
        
- `퀵 정렬`
    
    가장 많이 사용되는 알고리즘으로, 기준(피벗)을 설정한 다음 큰 수와 작은 수를 교환한 후 리스트를 반으로 나누는 방식 (호어분할 : 리스트에서 첫번째 데이터를 피벗으로 정함)
    
    1. 피벗을 설정한 뒤, 왼쪽에서부터 피벗보다 큰 데이터를 찾고 오른쪽에서부터 피벗보다 작은 데이터를 찾는다. 
    2. 큰데이터와 작은 데이터의 위치를 서로 교환해준다. 
    3. 왼쪽에서부터 찾은 값과 오른쪽에서부터 찾은 값의 위치가 서로 엇갈린 경우, 작은 데이터와 피벗의 위치를 서로 변경한다.(작은데이터가 없을 경우 pass)
    4. 피벗 기준으로 왼쪽의 데이터들은 피벗보다 작고, 오른쪽의 데이터들은 피벗보다 커지게된다.
    5. 왼쪽, 오른쪽 리스트에서도 각각 1~4과정을 반복한다. 
    - 과정 예시
        
        ![Untitled](Sorting%2037822df44c5e4bf696fae0c0a93eb0d4/Untitled.jpeg)
        
    
    > 퀵 정렬은 재귀함수 형태로 작성했을 때 구현이 간결하며, 퀵 정렬이 끝나는 조건은 현재리스트의 데이터 개수가 1개인 경우
    > 
    
    ```python
    array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]
    
    def quick_sort(array, start, end):
    	if start >= end: # 원소가 1개인 경우 종료
    		return
    	pivot = start # 피벗은 첫번째 원소
    	left = start + 1
    	right = end
    	while left <= right:
    
    		# 피벗보다 큰 데이터를 찾을 때까지 반복
    		while left <= end and array[left] <= array[pivot]:
    			left += 1
    		# 피벗보다 작은 데이터를 찾을 때까지 반복
    		while right > start and array[right] >= array[pivot]:
    			right -= 1
    
    		if left > right: # 엇갈렸다면 작은 데이터와 피벗을 교체
    										 # 모든 데이터가 pivot보다 크다면 pivot = right
    			array[right], array[pivot] = array[pivot], array[right]
    		else: # 엇갈리지 않았다면 작은 데이터와 큰 데이터를 교체
    			array[left], array[right] = array[right], array[left]
    
    	# 분할 이후 왼쪽 부분과 오른쪽 부분에서 각각 정렬 수행
    	quick_sort(array, start, right - 1)
    	quick_sort(array, right + 1, end)
    
    quick_sort(array, 0, len(array) - 1)
    print(array)
    ```
    
    ```python
    array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]
    
    def quick_sort(array):
    	# 리스트가 하나 이하의 원소만을 담고 있다면 종료
    	if len(array) <= 1:
    		return array
    
    	pivot = array[0] # 피벗은 첫번째 원소
    	tail = array[1:] # 피벗을 제외한 리스트
    
    	left_side = [x for x in tail if x <= pivot] # 분할된 왼쪽 부분
    																							# 등호는 같은 숫자가 2개 이상일 경우 대비
    	right_side = [x for x in tail if x > pivot] # 분할된 오른쪽 부분
    
    	# 분할 이후 왼쪽 부분과 오른쪽 부분에서 각각 정렬을 수행하고, 전체 리스트 반환
    	return quick_sort(left_side) + [pivot] + quick_sort(right_side)
    
    print(quick_sort(array))
    ```
    
    - 시간 복잡도
        
        평균적은 O(NlogN), 이미 데이터가 정렬되어 있는 경우 매우 느리게 동작. 최악의 경우 O(N^2)이지만 파이썬 기본 정렬 라이브러리를 이용하면 O(NlogN)을 보장할 수 있음.
        
- `계수 정렬`
    
    특정한 조건이 부합할 때만 사용할 수 있지만 매우 빠른 정렬 알고리즘, 데이터의 크기 범위가 제한되어 정수 형태로 표현할 수 있을 때만 사용가능하며 별도의 리스트를 선언하고 그 안에 정렬에 대한 정보를 담음
    
    ```python
    # 모든 원소의 값이 0보다 크거나 같다고 가정
    array = [7, 5, 9, 0, 3, 1, 6, 2, 9, 1, 4, 8, 0, 5, 2]
    # 모든 범위를 포함하는 리스트 선언(모든 값을 0으로 초기화)
    count = [0]*(max(array) + 1)
    
    for i in array:
    	count[i] += 1 # 각 데이터에 해당하는 인덱스 값 증가
    for i in range(len(count)):
    	for j in range(count[i]):
    		print(i, end = " ")
    
    # 0 0 1 1 2 2 3 4 5 5 6 7 8 9 9
    ```
    
    - 시간 복잡도
        
        O(N+K) N : 데이터의 개수 K : 최대값, 데이터의 범위만 한정되어 있고 데이터의 크기가 많이 중복되러 있을수록 효과적
        
    - 공간 복잡도
        
        O(N+K)
        
- `파이썬 정렬 라이브러리`
    
    문제에서 별도의 요구가 없다면 단순히 정렬해야하는 상황에서는 기본 정렬 라이브러리 사용, 데이터의 범위가 한정되어있다면 계수정렬 사용
    
    - sorted()
        
        O(NlogN) 기본 정렬 라이브러리, 퀵 정렬과 동작 방식이 비슷한 병합정렬 기반
        
    - sort()
        
        O(NlogN) 리스트 객체 내장함수
        
- **위에서 아래로**
    
    주어진 수를 내림차순으로 정렬해라
    
    < 입력조건 >
    
    첫째줄 : 수열에 속해있는 수의 개수 N 1~500
    
    둘째줄 : N개의 수  1~100,000
    
    ```python
    N = int(input())
    array = []
    for _ in range(N): array.append(int(input()))
    array.sort(reverse = True)
    print(*array)
    ```
    
- **성적이 낮은 순서로 학생 출력하기**
    
    N명의 학생의 이름과 성적이 주어졌을 때, 성적이 낮은 순서대로 학생의 이름을 출력해라
    
    < 입력조건 >
    
    첫째줄 : 학생수 N 1~100,000
    
    둘째줄 : 학생이름과 성적이 공백으로 입력
    
    ```python
    N = int(input())
    
    student = []
    
    for _ in range(N): student.append(input().split())
    student = sorted(student, key = lambda x : x[1])
    for i in student: print(i[0], end = " ")
    ```
    
- **두 배열의 원소 교체**
    
    크기가 N인 두 배열 A,B이 주어지며 최대 K번 바꿔치기가 가능하다. 배열 A의 모든 원소 합이 최대가 되도록 해라.
    
    < 입력조건 > 
    
    첫째줄 : N(1~100,000), K(0~N)
    
    둘째줄 : 배열 A의 원소들 (1~10,000,000)
    
    셋째줄 : 배열 B의 원소들 (1~10,000,000)
    
    ```python
    N, K = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    A.sort()
    B.sort(reverse=True)
    for i in range(K):
        if A[i] < B[i] : A[i], B[i] = B[i], A[i]
        else: break
    print(sum(A))
    ```