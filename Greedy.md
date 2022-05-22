# Greedy

기준에 따라 좋은 것을 선택하는 알고리즘

<aside>
💡 그리디 알고리즘 문제에서는 문제 풀이를 위한 최소한의 아이디어를 떠올리고 이것이 정당한지 컴토할 수 있어야 답 도출 가능

</aside>

- **큰 수의 법칙**
    
    다양한 수로 이루어진 배열이 있을 때 이 수를 M번 더하여 가장 큰 수를 만들어라. 단 같은 인덱스를 가진 수는 연속으로 K번을 초과해서 더해질 수 없다.
    
    < 입력조건 > 
    
    첫째줄 : N(2≤N≤1000) M(1≤M≤10000) K(1≤K≤10000)
    
    둘째줄 : N개의 자연수 (1≤자연수≤10000)
    
    - 문제 해설
        
        가장 큰수를 K번 더하고 두번째로 큰 수를 한번 더하는 연산을 반복
        
        1) 단순한 풀이
        
        ```python
        N,M,K = map(int,input().split())
        Nums = list(map(int,input().split()))
        Nums.sort(reverse=True)
        
        First = Nums[0]
        Second = Nums[1]
        
        sum = 0
        K_temp = K
        
        for _ in range(M):
            if K_temp == 0:
                K_temp = K
                sum += Second
                continue
            sum += First
            K_temp -= 1
        
        print(sum)
        ```
        
        2) M이 100억을 넘어간다면?
        
        ```python
        N,M,K = map(int,input().split())
        Nums = list(map(int,input().split()))
        Nums.sort(reverse=True)
        
        First = Nums[0]
        Second = Nums[1]
        
        p = M//(K+1)
        r = M%(K+1)
        
        print(First*(K*p + r) + Second*p)
        ```
        
- **거스름돈**
    
    500원 100원 50원 10원짜리 동전으로 거스름돈 동전 최소개수
    
    ```python
    money = int(input())
    
    # 큰 단위의 화폐부터 차례대로 확인
    coins = [500,100,50,10]
    coinNum = 0
    
    for coin in coins:
    	coinNum += money//coin
    	money %= coin
    
    print(coinNum)
    ```
    
    - 시간 복잡도 : O(K), K : 동전 종류
- **숫자 카드 게임**
    
     여러개의 숫자 카드 중에서 가장 높은 숫자가 쓰인 카드 한 장을 뽑는 게임이다.N은 행이고 M은 열이다. 먼저, 뽑고자 하는 카드가 포함되어 있는 행을 선택하고 선택된 행에 포함된 카드들 중 가장 숫자가 낮은 카드를 뽑아야한다. 따라서 처음에 행을 선택할 때, 이후 해당 행에서 숫자가 가장 낮은 카드를 뽑을 것을 고려하여 최종적으로 가장 높은 숫자의 카드를 뽑아라.
    
    < 입력조건 > 
    
    첫째줄 : N(행) M(열) (1≤N,M≤100)
    
    둘째줄 : N개 줄의 숫자 (1≤자연수≤10000)
    
    - 문제해설
        
        각 행마다 가장 작은 수를 찾은 뒤에 그 수 중 가장 큰 수
        
        ```python
        N,M = map(int,input().split())
        d = []
        
        for i in range(N):
        	d.append(min(list(map(int, input().split()))))
        
        print(max(d))
        ```
        
- **1이 될때까지**
    
    N이 1이 될때까지 두 과정중 하나를 선택하여 수행해라.단 수행수는 최소여야한다.
    
    1. N에서 1을 뺀다.
    2. N을 K로 나눈다.단, N이 K로 나누어 떨어질 때만
    
    < 입력조건 >
    
    첫째줄 : N(2≤N≤100000) K(2≤K≤100000)
    
    - 문제해설
        
        최대한 많이 나누기
        
        1) 단순한 풀이
        
        ```python
        N,K = map(int,input().split())
        
        num = 0
        while N!=1:
        	num += 1
        	if N%K == 0:N//=K
        	else:N-=1
        
        print(num)
        ```
        
        2) N이 100억을 넘어간다면?
        
        ```python
        N,K = map(int,input().split())
        
        num = 0
        while N!=1:
            while N%K==0: # 나눠질때까지 나눔
                N//=K
                num+=1
            if N!=1:
                if N>K: N-=N%K;num+=N%K  # 나머지를 빼줘서 K로 나눠떨어지도록 만듬
                else: num+=N-1;break   # 더이상 나눌 수 없으므로 break
        print(num)
        ```