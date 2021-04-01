## Closest pair of points ##

- 입력값 : 랜덤으로 할당된 점들의 배열 S

- 출력값 : S에 있는 점들 중 최근접 점의 쌍과 그 거리

------



> 이 문제는 총 3가지 방법으로 나눌 수 있다.

​	**A)  가장 가까운 두 점이 왼쪽 영역에 있는 경우** 

​	**B)  가장 가까운 두 점이 오른쪽 영역에 있는 경우**

​	**C)  가장 가까운 두 점이 왼쪽과 오른쪽 영역에 하나씩 있는 경우**



1. 일단, 점들이 배열 S에 무작위로 할당이 되었기 때문에 x-좌표에 대해 오름차순 정렬을 해준다.

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


2. 만약 주어진 점의 개수가 2개라면 그대로 쌍을 리턴해준다.

   ```java
   if (S_length <= 3)
       if (S_length == 2)
           return S;
   ```

   

3. 점의 개수가 3개라면, 3개의 쌍에 대하여 거리를 구한 후 최근접 점의 쌍을 리턴해준다. 

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



> *A와 B는 재귀적으로 쉽게 구하기 가능!*

4. x-좌표로 정렬된 S를 왼쪽과 오른쪽에 같은 개수의 점을 가지는 **Sl과 Sr로 분할**한다.

​	만약 S의 점의 개수가 홀수이면 Sl쪽에 1개 많게 분할한다. 분할된 Sl과 Sr에 대해서 재귀적으로 최근접 점의 쌍을 찾아서 각각을 CPl과 CPr이라고 놓는다.

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

​	(중간 영역에 속한 점들 : 왼쪽 부분의 가장 오른쪽 점의 x 좌표에서 d를 뺀 값과 오른쪽 부분의 가장 왼쪽 점의 x좌표에 d를 더한 값 사이의 x 좌표 값을 가진 점들)

​	일단, band 안에 있는 점들을 **y-좌표를 기준으로 오름차순 정렬**해준다. 

​	각 점을 제일 아래에 있는 점부터 시작해 자기보다 y 좌표가 같거나 높은 것들을 비교하다가 y 좌표 차가 d 이상이 되면 그 점에 대한 비교를 끝낸다.

------

​	***WHY ??***

​	점의 비교는 d * d 직사각형 안에 있는 것들만 보면 된다. 일단, d * d 정사각형 안에는 점과 점 사이가 d 미만이면 안 된다.  점과 점 사이가 d 미만이라면 이미 구한 d가 1과 2에서 구한 쌍 중에 짧다는 것에 모순이 된다. 따라서 점과 점 사이는 d 이상의 거리가 있어야 한다.

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



추가적으로 필요한 함수 : 두 점 사이의 거리 구하기

```java
static double getDistance(int x, int y, int x1, int y1) {
	double d;
	int xd, yd;
	yd = (int) Math.pow((y1 - y), 2);
	xd = (int) Math.pow((x1 - x), 2);
	d = Math.sqrt(yd + xd);
	return d;
} 
```
------



1. if(i<=S) return (2 또는 3개의 점들 사이의 최근접 쌍) // 3개 이하일 경우, 분할 안 함
2. 정렬된 S를 같은 크기의 SL과 SR로 분할한다. 단 |S|가 홀수이면, |SL|=|SR|+ 1 이 되도록 분할한다.
3. CPL = ClosestPair(SL) // CPL은 S1에서의 최근접 점의 쌍
4. CPR = ClosestPair(SR) // CPR은 S2에서의 최근접 점의 쌍
5. d = min{dist(CPL), dist(CPR)}일 때, 중간 영역에 속하는 점들 중에서 최근접 점의 쌍을 찾아서 이를 CPC라고 한다. 단, dist()는 두 점 사이의 거리이다.
6. return (CPL, CPC, CPR 중에서 거리가 가장 짧은 쌍)

저희 팀은 처음에 위 가정으로 진행하는 것으로 정하고 각자 파트 분배를 했습니다. 1,2번은 전수연, 3,4번은 권순준, 5,6번은 이주현이 맡아서 코드를 짜오고, 각자 짜온 부분을 합쳐서 최종 결과본을 만들기로 결정했습니다. 하지만 코드를 짜다보니, 각자 파트를 나눈 상태에서 코딩을 하게 되면 나중에 통합하는 과정에서 이해하는 것에 어려움을 느낄 것 같아 처음부터 다 같이 공부하여 코드를 짜보고 통합하는 것으로 협의하였습니다. 그렇게 코드를 짠 후에 합치기로 한 날, 서로의 코드를 보다보니 겹치는 부분이 상당수였고, 각자 브랜치에 올린 코드 중 더욱 간결한 코드를 사용하기로 하고 이주현의 코드를 최종 결과로 하여 완성했습니다.
