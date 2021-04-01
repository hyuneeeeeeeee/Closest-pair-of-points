## Closest-pair-of-points ##

- 입력값 : 랜덤으로 할당된 점들의 배열 S

- 출력값 : S에 있는 점들 중 최근접 점의 쌍과 그 거리

------



> 이 문제는 총 3가지 방법으로 나눌 수 있다.

​	**A)  가장 가까운 두 점이 왼쪽 영역에 있는 경우** 

​	**B)  가장 가까운 두 점이 오른쪽 영역에 있는 경우**

​	**C)  가장 가까운 두 점이 왼쪽과 오른쪽 영역에 하나씩 있는 경우**



1. 만약 주어진 점의 개수가 2개라면 그대로 쌍을 리턴해준다.

   ```java
   if (S_length <= 3)
       if (S_length == 2)
           return S;
   ```

   

2. 점의 개수가 3개라면, 3개의 쌍에 대하여 거리를 구한 후 최근접 점의 쌍을 리턴해준다. 

   ```java
   for (int i = start; i < end; i++) {
       for (int j = i + 1; j <= end; j++) {
           double k = getDistance(S[i][0], S[i][1], S[j][0], S[j][1]);
           if (k < dist) {
               dist = k;
               CP[0][0] = S[i][0];
               CP[0][1] = S[i][1];
               CP[1][0] = S[j][0];
               CP[1][1] = S[j][1];
           }
       }
   }
   ```



3. 점들이 배열 S에 무작위로 할당이 되었기 때문에 x-좌표에 대해 오름차순 정렬을 해준다.

   그리고 주어진 점들의 i번째와 (i + 1)번째의 x 좌표 평균을 중앙선으로 놓는다.

   ```java
   Random random = new Random();
   int n = random.nextInt(100);
   int[][] Point = new int[n][2];
   int[][] temp = new int[1][2];
   
   for (int i = 0; i < Point.length; i++)
       for (int j = 0; j < 2; j++) {
           Point[i][0] = random.nextInt();
           Point[i][1] = random.nextInt();
       }
   
   for (int a = 0; a < Point.length; a++) {
       for (int b = a + 1; b < Point.length; b++) {
           if (Point[a][0] > Point[b][0]) {
               temp[0] = Point[a];
               Point[a] = Point[b];
               Point[b] = temp[0];
           }
       }
   } 
   ```





> *A와 B는 재귀적으로 쉽게 구하기 가능!*

4. x-좌표로 정렬된 S를 왼쪽과 오른쪽에 같은 개수의 점을 가지는 **Sl과 Sr로 분할**한다.

​	만약 S의 점의 개수가 홀수이면 Sl쪽에 1개 많게 분할한다. 분할된 Sl과 Sr에 대해서 재귀적으로 최근접 점의 쌍을 찾아서 각각을 CPl과 	CPr이라고 놓는다.

​	그리고 CPl과 CPr 중 거리가 더 작은 값의 쌍을 구해주면 된다. 

```java
int CPl[][] = ClosestPair(S, start, mid);

if (S_length % 2 == 1) {                          //점의 개수가 홀수일 경우
	int CPr[][] = ClosestPair(S, mid + 1, end);        
	if (getDistance(CPl[0][0], CPl[0][1], CPl[1][0], CPl[1][1]) < getDistance(CPr[0][0], CPr[0][1], CPr[1][0], CPr[1][1]))
		CP = CPr;
	else
		CP = CPl;           
	} 
else {                                      	//점의 개수가 짝수일 경우
	int CPr[][] = ClosestPair(S, mid, end);        
	if (getDistance(CPl[0][0], CPl[0][1], CPl[1][0], CPl[1][1]) < getDistance(CPr[0][0], CPr[0][1], CPr[1][0], CPr[1][1]))
		CP = CPr;
else
		CP = CPl;           
}
```





> *C 구하는 방법 : A와 B 과정에서 얻을 수 있는 최소 거리인 Dl과 Dr을 구한 뒤, 크기를 비교해 더 작은 값을 dist로 취한다.* 
>
> ***(dist = min(Dl, Dr))***

​	5. 중앙선을 기준으로 dist만큼 떨어진 지역을 **band**라고 부른다. 이 band 안에 있는 점에 대해서만 거리를 구해준다.

​	(중간 영역에 속한 점들 : 왼쪽 부분문제의 가장 오른쪽 점의 x좌표에서 d를 뺀 값과 오른쪽 부분문제의 가장 왼쪽 점의 x좌표에 d를 더	한 값 사이의 x 좌표 값을 가진 점들)

​	일단, band 안에 있는 점들을 **y좌표를 기준으로 오름차순 정렬**해준다. 

​	각 점을 제일 아래에 있는 점부터 시작해 자기보다 y좌표가 같거나 높은 것들을 비교하다가 y좌표차가 d 이상이 되면 그 점에 대한 

​	비교를 끝낸다.

------

​	***WHY ??***

​	점의 비교는 d * d 직사각형 안에 있는 것들만 보면 된다. 일단, d * d 정사각형 안에는 점과 점 사이가 d 미만이면 안 된다.  점과 점 사이	가 d 미만이라면 이미 구한 d가 1과 2에서 구한 쌍 중에 짧다는 것에 모순이 된다. 따라서 점과 점 사이는 d 이상의 거리가 있어야 한다.

------

​	이렇게 구한 점들 중에서 최근접 점의 쌍을 찾아서 이를 CPc라고 한다.

​	찾은 최근접 점의 쌍 CPl, CPr, CPc 중에서 가장 짧은 거리를 가진 쌍을 해로서 리턴한다.

```
int[][] CPc = new int[S_length][2];          //중간 영역에 속하는 점들의 배열
int Sc_length = 0;
for (int i = start; i <= end; i++) {
	int t = S[mid][0] - S[i][0];
	if (Math.abs(t) < dist) {
	CPc[Sc_length][1] = S[i][1];
	Sc_length++;
	}
}                           //y 좌표를 기준으로 오름차순 정렬하면서 배열에 값 넣기

for (int i = 0; i < Sc_length - 1; i++) {
	for (int j = i + 1; j < Sc_length; j++) {
	int t = CPc[j][1] - CPc[i][1];
	if (Math.abs(t) < dist)
	if (getDistance(CPc[i][0], CPc[i][1], CPc[j][0], CPc[j][1]) < dist)
		CP = CPc;               //자기보다 큰 요소만 보면서 거리 측정
	else
		break;              //y 좌표를 기준으로 오름차순 정렬되어있으므로 dist보다 거리가 큰 순간이 오면 반복문 종료
	}
}
return CP;
```
