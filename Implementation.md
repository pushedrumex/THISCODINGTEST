# Implementation

머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정

- 완전 탐색 : 가능한 경우의 수를 모두 검사해보는 탐색 방법
- 시뮬레이션 : 문제에서 제시한 알고리즘을 한단계씩 차례대로 직접 수행

<aside>
💡 **파이썬 리스트**

코딩 테스트에서는 보통 128~512MB로 메모리를 제한하며 파이썬의 경우 리스트의 길이가 1000이면 약 4KB를 차지한다.코테에서는 메모리 사용량 제한보다 더 적은 크기의 메모리를 사용해야한다.

</aside>

<aside>
💡 **제한 시간**

1초에 2000만 번의 연산을 수행한다고 가정하고 풀면 안정적이다. 알고리즘 문제를 풀 때 시간제한과 데이터의 개수를 먼저 확인해라. 또한 Pypy3는 파이썬3의 문법을 그대로 지원하며, 대부분 파이썬3보다 실행속도가 더 빠르다.특히 반복문이 많을 수록 Pypy3가 빠르다.

</aside>

- **상하좌우** 시뮬레이션
    
    N*N 크기의 정사각형 공간(왼쪽위는 1,1 오른쪽 아래는 N,N) 위에서 계획서가 주어졌을 때 최종적으로 도착할 지점의 좌표는? 시작좌표는 항상 (1,1)이며 정사각형을 넘어가는 계획은 무시한다.
    
    - L : 왼쪽 한칸 이동
    - R : 오른쪽 한칸 이동
    - U : 위로 한칸 이동
    - D : 아래로 한칸 이동
    
    < 입력 조건 >
    
    첫째줄 : 공간의 크기를 나타내는 N(1≤N≤100)
    
    둘째줄 : 계획서내용 (1≤이동횟수≤100)
    
    ```python
    N = int(input())
    plan = input().split()
    X,Y = 1,1
    
    for i in plan:
        if i == 'L' and Y>1:Y-=1
        elif i == 'R' and Y+1<=N:Y+=1
        elif i == 'U' and X>1:X-=1
        elif i == 'D' and X+1<=N:X+=1
    
    print(X,Y)
    ```
    
- **시각** 완전탐색
    
    00시00분00초부터 N시59분59초까지 모든 시각 중 3이 하나라도포함되는 모든 경우의 수를 구해라.
    
    < 입력조건 >
    
    첫째줄 : N(0≤N≤23)
    
    1. 풀이법1
    
    ```python
    N = int(input())
    
    # if 3시, 13시, 23시 = 3600
    # else = 1575
    
    num = 0
    
    for i in range(N+1):
        if i == 3 or i == 13 or i == 23:
            num += 3600
        else: num += 1575
    
    print(num)
    ```
    
    1. 풀이법2 : 시, 분, 초 를 문자열로 바꿔서 ‘3’이 있는지 확인
    
    ```python
    N = int(input())
    
    count = 0
    for i in range(N+1):
        for j in range(60):
            for k in range(60):
                if '3' in str(i) + str(j) + str(k):
                    count+=1
    
    print(count)
    ```
    
- **왕실의 나이트**
    
    8*8의 좌표 평면에서 말이 이동할 수 있는 모든 경우의 수는?단, 이동할 때는 L자 형태로만 이동할 수 있으며 좌표평면 밖으로는 나갈수 없다. 행 : 1~8, 열 : a~h
    
    < 입력조건 >
    
    첫째줄 : 현재 말이 위치한 좌표 행열
    
    ```python
    start = list(input()) # 열행
    
    X = int(ord(start[0]) - ord('a') + 1)
    Y = int(start[1])
    
    end = [[X+2,Y-1],[X+2,Y+1],[X-2,Y-1],[X-2,Y+1],[Y+2,X-1],[Y+2,X+1],[Y-2,X-1],[Y-2,X+1]]
    num = 0
    for i in end: if 0<i[0]<9 and 0<i[1]<9:num+=1
    print(num)
    ```
    
- **게임 개발**
    
    N*M크기의 직사각형의 맵에서 캐릭터는 상하좌우로 움직일 수 있으며 메뉴얼은 다음과 같을 때, 캐릭터가 방문한 칸의 수를 출력해라.
    
    1. 현재방향 기준으로 왼쪽방향부터 차례대로 갈 곳을 정한다.
    2. 왼쪽방향의 칸이 아직 안가본 칸이라면 왼쪽으로 회전한 후 한 칸 전진한다. 왼쪽방향의 칸이 이미 가본 칸이거나 바다라면 왼쪽방향만 회전만 수행하고 1단계로 돌아간다.
    3. 네 방향의 칸이 보두 가본칸이거나 바다로 되어있는칸이라면 바라보는 방향을 유지한채, 한칸 뒤로 이동한다. 뒷칸이 바다라면 움직임을 멈춘다.
    
    < 입력조건 >
    
    첫째줄 : 세로크기(3≤N≤50) 가로크기(3≤M≤50)
    
    둘째줄 : 캐릭터의 위치 A,B(A:아래방향 B:오른쪽방향)와 바라보는 방향 d(0:위쪽 1:오른쪽 2: 아래쪽 3:왼쪽)
    
    셋째줄 : 맵에의 정보 (0: 육지 1: 바다)
    
    1. 내 풀이
    
    ```python
    N,M = map(int, input().split())
    A,B,d = map(int, input().split())
    
    # game_map setting
    # 외부는 바다
    game_map = [[1]*(M+2)]
    for _ in range(N): game_map.append([1] + list(map(int,input().split())) + [1])
    game_map.append([1]*(M+2))
    
    A+=1
    B+=1
    game_map[A][B] = -1
    move = 1 # 방문한 칸의 수
    
    rotate = 0 # 회전 횟수
    while True:
        d = (3+d)%4 # 0 3 2 1 0 3 2 1
        rotate += 1
        flag = 0 # 이동한다면 1로 바뀜
        if d == 0 and game_map[A-1][B] == 0: A -= 1;flag = 1
        elif d == 1 and game_map[A][B+1] == 0: B += 1;flag = 1
        elif d == 2 and game_map[A+1][B] == 0: A += 1;flag = 1
        elif d == 3 and game_map[A][B-1] == 0: B -= 1;flag = 1
        if flag == 1: game_map[A][B] = -1;move += 1;rotate = 0
        if rotate == 4:
            if d == 3:
                if game_map[A][B+1] == 1:break
                else: B += 1
            elif d == 1:
                if game_map[A][B-1] == 1:break
                else:B -= 1
            elif d == 0:
                if game_map[A+1][B] == 1:break
                else:A += 1
            elif d == 2:
                if game_map[A-1][B] == 1:break
                else:A -= 1
            rotate = 0
    print(move)
    ```
    
    1. dx, dy 사용
    
    일반적으로 방향을 설정해서 이동하는 문제에서는 dx, dy 리스트를 만들어 방향을 정하는 것이 효과적
    
    ```python
    N,M = map(int, input().split())
    A,B,d = map(int, input().split())
    
    dxdy = {0:[-1,0],1:[0,1],2:[1,0],3:[0,-1]}
    
    # game_map setting
    # 외부는 바다
    game_map = [[1]*(M+2)]
    for _ in range(N): game_map.append([1] + list(map(int,input().split())) + [1])
    game_map.append([1]*(M+2))
    
    A+=1
    B+=1
    game_map[A][B] = -1 # 방문표시
    move = 1 # 방문한 칸의 수
    
    rotate = 0 # 회전 횟수
    while True:
        d = (3+d)%4 # 3 2 1 0 3 2 1 0
        rotate += 1
        if game_map[A+dxdy[d][0]][B+dxdy[d][1]] == 0:
            A+=dxdy[d][0];B+=dxdy[d][1];rotate = 0;move += 1;game_map[A][B] = -1
        if rotate == 4:
            if game_map[A-dxdy[d][0]][B-dxdy[d][1]] == 1:break # 바다라면,break
            else: A -= dxdy[d][0];B -= dxdy[d][1];rotate = 0 # 뒤로 한칸
    print(move)
    ```