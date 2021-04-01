### Line 3, 4


#### 점이 두 개이면 배열 그대로 반환 

```java
 static int[][] position; 

    public int[] closestPair(int[] a,int p,int q){
   		if(q-p==1)	
        	return a;
```

#### 점이 세 개이면 배열에서 최근접 점의 쌍 반환	

```java
if(q-p==2){
	int[] b=new int[2];
	 double min=getDist(a[0],a[1]);
            b[0] = a[0];
            b[1] = a[1];
			if(getDist(a[0],a[2])<min) {
                b[0] = a[0];
                b[1] = a[2];
           }
            if(getDist(a[1],a[2])<min) {
               b[0] = a[1];
               b[1] = a[2];
            }
            return b;
        } 
```

#### 점이 3개보다 많으면 SL과 SR로 분할

```java
int k=(p+q)/2;
int[] SL = new int[k]; 
int[] SR = new int[k];

for(int i=0;i<k;i++)
	SL[i]=position[i][0];
	
for(int i=k+1;i<a.length;i++)
	SR[i]=position[i][0]; 
```

#### 분할된 SL과 SR에 대해 재귀적으로 최근접 점의 쌍을 찾아 각각 CPL,CPR이라고 놓는다.

```java
int[] CPL=closestPair(SL,p,k);

int[] CPR=closestPair(SR,k+1,q);
           
```

#### 두 점 사이의 거리 구하기

```java
public static double getDist(int a, int b){  
	double x= Math.pow(position[a][0]-position[b][0],2);
    double y= Math.pow(position[a][1]-position[b][1],2);
    return Math.sqrt(x+y);
} 
```

#### 두 거리 중에 더 작은 값 반환

```java
public static double minDist(double a,double b){
	if(a<b)
		return a;
    else
        return b;
}
```

#### 입력: 오름차순으로 입력된 pNum개의 점

```java
 Scanner scanner = new Scanner(System.in);
        int pNum=scanner.nextInt();

        position=new int[pNum][2];

        for(int i=0;i<pNum;i++){

            position[i][0]= scanner.nextInt();

            position[i][1]= scanner.nextInt();
```

#### X좌표로 정렬된 배열 S

```java
int[] S=new int[pNum];
for(int i=0;i<pNum;i++)
	S[i]=position[i][0];
```

​        
