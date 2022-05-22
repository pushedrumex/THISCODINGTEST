# Binary Search

배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘으로, 탐색범위를 절반으로 좁혀가며 데이터를 탐색, O(logN), 탐색범위가 2000만을 넘는다면 이진탐색으로 문제 접근

1. 시작점과 끝점을 확인한 다음 둘 사이의 중간점을 정한다. 중간점이 실수일 때 소수점 이하를 버린다.
2. 찾으려는 데이터보다 중간점의 데이터가 크다면 끝점을 중간점 - 1로 변경한 후 1을 실행한다.
3. 찾으려는 데이터보다 중간점의 데이터가 작다면 시작점을 중간점 +1로 변경한 후 1을 실행한다.

```python
# 재귀함수
def binary_search(array, target, start, end):
	if start > end:
		return None
	mid = (start + end) // 2
	# 찾은 경우 인덱스 반환
	if array[mid] == target : return mid
	elif array[mid] > target : return binary_search(array, target, start, mid - 1)
	else : return binary_search(array, target, mid + 1, end)

n, target = list(map(int, input().split()))
array = list(map(int, input(),split()))

result = binary_search(array, target, 0, n-1)
if result == None : print("원소가 존재하지 않습니다.")
else : print(result + 1)
```

```python
# 반복문
def binary_search(array, target, start, end):
	while start <= end:
		mid = (start + end) // 2
		# 찾은 경우 인덱스 반환
		if array[mid] == target : return mid
		elif array[mid] > target : end = mid - 1
		else : start = mid + 1
	return None

n, target = list(map(int, input().split()))
array = list(map(int, input(),split()))

result = binary_search(array, target, 0, n-1)
if result == None : print("원소가 존재하지 않습니다.")
else : print(result + 1)
```

- `트리`
    
    < **트리 자료구조 >**
    
    데이터베이스는 내부적으로 대용량 데이터 처리에 적합한 트리자료구조를 이용하여 데이터가 정렬되어있다.
    
    ![Untitled](Binary%20Search%20ed8c7692bd304aecbed715064bd22902/Untitled.jpeg)
    
    - 루트노드 : 최상단 노드
    - 서브트리 : 트리에서 일부를 떼어낸것
    - 단말노드 : 최하단 노드
    
    < 이진탐색 트리 >
    
    이진 탐색이 동작할 수 있도록 고안된 효율적인 탐색이 가능한 자료구조로, 부모노드보다 왼쪽자식노드는 작고 오른쪽 자식노드는 크다. 즉, 왼쪽자식노드 < 부모노드 < 오른쪽자식노드
    
    → 찾으려는 데이터가 부모노드보다 작다면 왼쪽노드들, 크다면 오른쪽노드들만 확인
    
- **부품찾기**
    
    매장에 있는 부품의 종류 N개 에 손님이 찾는 부품 M개가 있는지 확인하는 프로그램을 작성해라.각각 있다면 yes, 없다면 no 출력
    
    < 입력조건 >
    
    첫째줄 : 정수 N (1~1,000,000)
    
    둘째줄 : N개의 정수 (2~1,000,000)
    
    셋째줄 : 정수 M (1~100,000)
    
    넷째줄 : M개의 정수(2~100,000)
    
    ```python
    N = int(input())
    n = list(map(int, input().split()))
    M = int(input())
    m = list(map(int, input().split()))
    
    n.sort()
    
    def bs(good, n, start, end):
        if start > end: return False
        mid = (start + end)//2
        if n[mid] > good : return bs(good, n, start, mid - 1)
        elif n[mid] < good : return bs(good, n, mid + 1, end)
        else : return True
    
    for good in m:
        if bs(good, n, 0, N-1):print("yes",end=" ")
        else:print("no",end=" ")
    
    # N개의 부품을 정렬하기 : O(NlogN)
    # M개의 부품을 찾는 과정 : O(MlogN)
    # 총 시간 복잡도 = O((M+N)logN)
    ```
    
- **떡복이 떡 만들기**
    
    절단기에 높이 H를 지정하면 줄지어진 떡을 한번에 절단한다. 높이가 H보다 긴 떡은 H위 부분이 잘리고 낮은 떡은 잘리지 않는다. 손님이 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에서 설정할 수 있는 높이의 최댓값은?
    
    < 입력조건 >
    
    첫째줄 : 떡개수 N(1~1000,000), 요청한 길이 M(1~2000,000,000)
    
    둘째줄 : 떡의 개별 높이 (0~10억)
    
    ```python
    def bs(L, M, start, end):
        while end >= start: # 가운데가 답일 수 있기 때문에 등호
            sum = 0 # 최소
            mid = (start + end) // 2 # 최대
    
            for l in L:
                if l > mid:sum += l - mid # 잘린 떡의 총 길이
    
            if sum < M: end = mid - 1
            elif sum > M: start = mid + 1;result = mid
            else:result = mid;break # 찾았다면 break
        return result
    
    N, M = map(int, input().split())
    L = list(map(int, input().split()))
    
    print(bs(L, M, 0, max(L)))
    ```