# 算法基础课

课上学主要思想；课下：1.背住模板（不用一字一句背，但要能调试通过）    2.找一个对应模板题默写（反复多次3-5遍，删了重写）

背的是**思路**而不是字母，写模板要熟练。

## 第1章. 基础算法

### 快速排序—分治

①确定分界点：可随机取点，常见 q[L]、q[ L+R / 2]、q[R]； ②⭐调整区间 ③递归处理左右两端

注意：边界问题要注意。（一般有边界问题的都建议直接背模板，因为模板是千锤百炼的且考试时难以仔细确定）；

​			快排是不稳定的（相同元素前后位置可能变化），但若把位置下标也放进来同时排序则就可以稳定。  O(nlog~2~n)

​			快排的模板在考试时基本不会用到，但工作面试时可能要求手敲。

C++内置的sort()函数是快排+插排的组合。

```c++
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do {
            i ++ ;
        } while (q[i] < x);
        do {
            j -- ;
        } while (q[j] > x);
        if (i < j)
            swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

/* 快排思路：
	1.有数组q，左端点l，右端点r     2.确定划分边界x     3.将 q 分为 <= x 与 >= x 的两个小数组
	4. i的含义：i之前的元素都≤x，即q[l..i-1]<=x
	5. j的含义：j之后的元素都≥x，即q[j+1..r]>=x
	6.结论：while循环结束后，q[l..j] <= x , q[j+1..r] >= x
	7.递归处理两个小数组
*/
```



### 归并排序—分治

①确定分界点：mid=(L+R) / 2  ②递归排序left&right  ③⭐归并—合二为一

注意：归并是稳定的。    O(nlog~2~n)

```c++
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;        //递归的终止情况

    int mid = l + r >> 1;      //第一步：分成子问题  [此时 mid 向下取整, merge_sort(q, l, mid ) 中 mid 一定不会取到r值 
    						//∴ merge_sort(q, l, mid ) 不会无限划分]
    
    merge_sort(q, l, mid);      //第二步：递归处理子问题
    merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;    //第三步：合并子问题
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) 
            tmp[k ++ ] = q[i ++ ];       // 注：tmp 保存的是 q[l..mid] , q[mid+1..r] 中从小到大排序的所有数
        else 
            tmp[k ++ ] = q[j ++ ];

    while (i <= mid) 
        tmp[k ++ ] = q[i ++ ];
    while (j <= r) 
        tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) 
        q[i] = tmp[j];
}

/*  归并思路：
	1.有数组 q, 左端点 l, 右端点 r      2.确定划分边界 mid      3.递归处理子问题 q[l..mid], q[mid+1..r]
	4.合并子问题 {Ⅰ.主体合并：至少有一个小数组添加到 tmp 数组中
				Ⅱ.收尾：可能存在的剩下的一个小数组的尾部直接添加到 tmp 数组中
				Ⅲ.复制回来：tmp 数组覆盖原数组			}												*/
```



### 整数二分

每次把长度缩减一半，再选择答案所在的区间。保证每一次所选的区间里面都有所需答案。二分的模板是需要题目一定有解的。

适用：定义的性质是有边界的，且可被区分。

```c++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_L(int l, int r)  //查找左边界 Left
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}

// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_R(int l, int r)  //查找右边界 Right
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}

/*  记忆：有加必有减  int mid = l + r + 1 (加)>> 1;  if (check(mid)) l = mid;  else r = mid - 1  (减);
一般二分应用于无非下面四种情况:
1：找大于等于数的第一个位置 （满足某个条件的第一个数）
2：找小于等于数的最后一个数 （满足某个条件的最后一个数）
3.查找最大值 （满足该边界的右边界）
4.查找最小值 (满足该边界的左边界)                                                                             */
```



### 浮点数二分

当最终区间长度足够小（如 R-L<=10^-6^）即认为有答案。

```c++
ool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_f(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```



### 高精度运算-C/C++

仅C/C++要用，Java、Python一般不用考虑。Java有BigInteger，Python可以无限大。

用一个数组来存大数的每一位，注意低序号存低位（a[0]存个位）；  计算过程模仿小学的列竖式。

#### 高精度—加法-Add：

```c++
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int> &A, vector<int> &B)   //传的引用，使用vector容器
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;                          // C数组存放运算结果
    int t = 0;							 // t是进位
    
    for (int i = 0; i < A.size(); i ++ )    // 从低位开始计算，保证A的位数最大
    {
        t += A[i];
        if (i < B.size())
            t += B[i];
        C.push_back(t % 10);			   // 该位存放相加后的结果(数只能是0~9，多了会进位)
        t /= 10;                            //考虑是否进位，t只能是0或1
    }

    if (t)                                  //最终如果还有进位就放在最高位
        C.push_back(t);
    
    return C;
}
```

#### 高精度—减法-Sub：

```c++
// C = A - B, 满足A >= B, A >= 0, B >= 0   [执行自定义的cmp函数，如果 A<B 就交换下位置：A-B 等价于 -(B-A)]
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    int t = 0;              // t是借位。t是1表示被借位，t是0则没有
    
    for (int i = 0; i < A.size(); i ++ )
    { 
        t = A[i] - t;        // 要进行 A[i] - B[i]; 先让A把对应借位t减掉，再看减去B后是否小于0
        if (i < B.size()) 
            t -= B[i];       // 等价于 t = t - B[i]           {减等操作：C -= A 相当于 C = C - A}  
        C.push_back((t + 10) % 10);       // 存放对应位的结果数。 +10表示若A-B小于0就让A从高位借个10
        if (t < 0) 
            t = 1;			// 若A-B-t<0，则表示A要向高位借位。
        else 
            t = 0;
    }

    while (C.size() > 1 && C.back() == 0)     //去掉高位前导0  [如 001 → 1]      
        C.pop_back();						// {back()返回对向量最后一个元素的引用  pop_back()删除最后一个元素}
    
    return C;
}
```

#### 高精度乘低精度-Mul：

这里的乘法是把b看作一个整体与A的每一位相乘，同时考虑到进位。

```c++
// C = A * b, A >= 0, b >= 0
vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;

    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )   //执行运算时A的位数还在或者A位数全完后t进位还在  [从低位开始]
    {
        if (i < A.size()) 
            t += A[i] * b;                      //将b看作一个整体与A的每一位相乘
        C.push_back(t % 10);
        t /= 10;                                //进位，但此时t不仅仅是0/1
    }

    while (C.size() > 1 && C.back() == 0)       //去除前导0
        C.pop_back();

    return C;
}
```

#### 高精度乘高精度

```c++
// C = A * B, A >= 0, B >= 0
vector<int> mul_HA(vector<int> &a, vector<int> &b) 
{
	vector<int> c(a.size() + b.size(), 0);    
  	int t = 0;                        // t是进位
    
  	for(int i = 0; i < a.size(); i++) 
    {
    	for(int j =0; j < b.size(); j++)   //外层A的每一位与B相乘（转成低精度乘高精度）
        {
      		c[i + j] += a[i] * b[j] + t;
      		t = c[i + j] / 10;
      		c[i + j] %= 10;
    	}
    	c[i + b.size()] = t;      //每一遍循环后的进位
  	}
    
 	while(c.size() > 1 && c.back() == 0)     //去除前导0
     	 c.pop_back();
    
  	return c;
}
```

#### 高精度除以低精度-Div：

```c++
// A ÷ b = C ... r, A >= 0, b > 0
vector<int> div(vector<int> &A, int b, int &r)        
{
    vector<int> C;                   // C是商;  r是余数(传的引用)
    r = 0;
    
    for (int i = A.size() - 1; i >= 0; i -- )      //除法是从高位开始
    {
        r = r * 10 + A[i];
        C.push_back(r / b);				//余数除以b
        r %= b;                          //得到新的余数
    }
    
    reverse(C.begin(), C.end());         //商结果的整个颠倒（让数组低序号存商的低位）
    
    while (C.size() > 1 && C.back() == 0)      //去除商的前导0
        C.pop_back();
    
    return C;
}
```

### 前缀和-数列求和

前缀和—从位置1到位置i这个区间内的所有的数字之和。   （类似数组求和 S~i~ ）

[不算一个模板，属于一个公式，一种思想]

**一维前缀和：**

类似于数列。               预处理(即求出S[i])是O(n)，但查询a[l]+..+a[r]是O(1)

```c++
S[i] = a[1] + a[2] + ... + a[i]                  //S[i]即是前缀和。[注意序号必须从1开始，这样S[0]就定义为0，好处理边界]
    
a[l] + ... + a[r] = S[r] - S[l - 1]         //区间和计算
S[i] = S[i-1] + a[i]                        //前缀和的初始化   
    
a[1] + ... + a[10] = S[10] - s[0] = s[10]
```

**二维前缀和：**

注意这里是二维数组，不是面积坐标系，注意序号。  这里↓图中交点边其实是分散的数组离散点。与矩阵相关.    {容斥原理}

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202308242029247.jpg" style="zoom:25%;" />

```c++
/*    
S[i, j] = 第i行j列格子左上部分所有元素的和。 
S[i,j] = S[i-1,j] + S[i,j-1] - S[i-1,j-1] + a[i,j]              //求前缀和

以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
  S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]       //算子矩阵和
*/
```

### 差分（前缀和的逆运算）

有数组a[1], a[2], …, a[n]  构造数组 b[1], b[2], …, b[n] 使得满足  a[i] = b[1] + b[2] + … + b[i]； {即 bn = a~n~ - a~n-1~}

则 b数组 成为 a数组 的 差分； a数组为b数组的前缀和。                    {等差数列中公差d=an-an-1， d即为a的差分}

差分都不用考虑如何构造。

**一维差分：**

```c++
/*  应用：
	要给a数组的区间[l, r]中的每个数都加上c： 
	   差分操作：先b[l] += c, 再 b[r+1] -= c ；   {差分操作为O(1)}
		∵ ai = b1 +…+ bi,  ∴从l->r,每个ai都会加上bl,所以会加上c ∵r区间之后的ai保持原样 ∴让br+1 - c 来抵消之前的+c

构造时：对于a数组中的a[i]，就等价于在[i,i]区间上的a数组数加上a[i] ;   {不用考虑差分如何构造，只考虑如何更新}
*/	
insert(i, i, a[i]);
void insert(int l, int r, int c){
	b[l] += c;
    b[r+1] -= c;
}
a[i] = b[1] + ... + b[i]       // <==>  for_i: b[i] += b[i-1]   then a[i] <==> b[i]
```

**二维差分：**

构造差分矩阵b，满足原矩阵a是差分矩阵b的前缀和。

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202308242255552.PNG" style="zoom:50%;" />

```c++
/*应用： 给其中的子矩阵中元素加值。
  给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
	S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c
*/	
insert(i,j,i,j, a[i][j]);
void insert(int x1, int y1, int x2, int y2, int c){
	b[x1][y1]     += c;         //相当于(x1,y1)右下角矩阵所有元素+c
    b[x2+1][y1]   -= c;
    b[x1][y2+1]   -= c;
    b[x2+1][y2+1] += c;
}
a[2][2] = b[1][1] + b[1][2] + b[2][1] + b[2][2]    //假设序号从1开始
//  <==>  for_i, for_j: b[i][j] += b[i-1][j] + b[i][j-1] - b[i-1][j-1]   then a[i][j] <==> b[i][j]     
```

### 双指针算法

```c++
for (int i = 0, j = 0; i < n; i ++ )
{
    while (j < i && check(i, j)) {
        j ++ ;
    }
    // 具体问题的逻辑
}
常见问题分类：
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作
    
/*  复杂度是O(n) ； 核心思想：将O(n^2)的朴素算法优化到O(n)                */
```

### 位运算-求第k位 / lowbit

```c++
/*  常见应用：
		(1) 求n的二进制表示中第k位数字是多少:   
		          n >> k & 1                （先右移k位再与上1）
			  [思路]1.先用右移操作把第k位移到最后一位 2.按位与上1来看个位是几
		(2) lowbit()操作：返回n的二进制表达式中最低位的1所对应的值：<常见于对树状数组的操作> 
		          lowbit(n) = n & -n        （自己与上自己的相反数）
        	   [假设一个数的二进制最低位的1在从右往左数的第k位，那么它的lowbit值就是2^(k−1), 即lowbit(0b1010) = 0b10 = 2]	             [思路]因负数是补码表示，即 -x = ~x+1 (取反加一)；又因10...00 取反变01...11，再加一就相同，所以与上即可。
*/
```

### 整数的离散化 (保序)

离散化：把无限空间中有限的个体映射到有限的空间中去，以此提高算法的时空效率。 离散化本质上是一种哈希，它在保持原序列大小关系的前提下把其映射成正整数。当原数据很大或含有负数、小数时，难以表示为数组下标，一些算法和数据结构（如BIT）无法运作，这时可以考虑将其离散化。                离散化是在不改变数据相对大小的条件下，对数据进行相应的缩小。降低时间复杂度。

当数据只与它们之间的**相对大小有关**，而与**具体是多少无关**时，可以进行离散化。

```c++
vector<int> alls;                                           // 存储所有待离散化的值
sort(alls.begin(), alls.end());                             // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素 [先unique调整重复元素位置;再把重复元素删去]
			   /*  unique()该函数的作用是“去除”容器或者数组中所有相邻的重复出现的元素(只保留一个);
			   注:Ⅰ.此处去除并非真正意义的erase，而是把不重复的元素移到前面来，返回值是重复元素的首地址。 
                   Ⅱ.unique针对是相邻元素; 对于顺序顺序错乱的数组成员或容器成员，需要先进行排序，可调用std::sort()函数 */

// 二分求出x对应的离散化的值
int find(int x)                // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1;             // 映射到 1, 2, ...n   [加1就是从1开始映射，不加1就是从0开始映射]
}
```



```c++
// unique()函数思维实现：（属于双指针算法）
vector<int>::iterator unique(vector<int> &a){
	int j=0;
    for(int i=0; i<a.size(); i++){
		if(!i || a[i] != a[i-1]){    //第一个数一定在里面，相邻元素不能相等
            a[j++] = a[i];            // a[0] ~ a[j-1] 就是所有a中不重复的数
        }
    }
	return a.begin() + j;
}
```

### 区间合并

①按区间左端点排序   ②按顺序扫描，更改end

```c++
// 将所有存在交集的区间合并   (如果两个区间只有端点相交也算作要合并)
typedef pair<int, int> PII                    // first存左端点，second存右端点
void merge(vector<PII> &segs)        
{
    vector<PII> res;

    sort(segs.begin(), segs.end());      //先排序  [pair排序是先以左端点排序再按右端点排序]

    int st = -2e9, ed = -2e9;      //先假设一个无穷小区间 [-∞,-∞]  -2e9 = -2×10^9
    for (auto seg : segs) {        // 从前往后扫描所有区间
        if (ed < seg.first)                  // ①新区间seg左端点与之前的区间的end不相交
        {
            if (st != -2e9) {
                res.push_back({st, ed});
            }
            st = seg.first;                  //更新左右端点st、ed
            ed = seg.second;
        }
        else {                             // ②新区间seg左端点与之前的区间的end有相交
            ed = max(ed, seg.second);	   // 更新end
        }
    }
    
    if (st != -2e9) {
        res.push_back({st, ed});
    }
    
    segs = res;
}
```

### 取余(%)运算规则

```c
注意取余运算与取模运算的区别！！！
取余运算在对负整数取余时采取向0取整；
取模运算在对负整数取模时采取向下取整（即向-∞取整）！！！
【定义】
给定一个正整数p，任意一个整数n，一定存在等式 ：
n = kp + r
其中 k、r 是整数，且 0 ≤ r < p，则称 k 为 n 除以 p 的商，r 为 n 除以 p 的余数。
对于正整数 p 和整数 a,b，定义如下运算：
取模运算：a % p（或a mod p），表示a除以p的余数。
模p加法： ，其结果是a+b算术和除以p的余数。
模p减法： ，其结果是a-b算术差除以p的余数。
模p乘法： ，其结果是 a * b算术乘法除以p的余数。
注：
1. 同余式：正整数a，b对p取模，它们的余数相同，记做 或者a ≡ b (mod p)。
2. n % p 得到结果的正负由被除数n决定,与p无关。例如：7%4 = 3， -7%4 = -3， 7%-4 = 3， -7%-4 = -3。
 
【基本性质】
若p|(a-b)，则a≡b (% p)。例如 11 ≡ 4 (% 7)， 18 ≡ 4(% 7)
(a % p)=(b % p)意味a≡b (% p)
对称性：a≡b (% p)等价于b≡a (% p)
传递性：若a≡b (% p)且b≡c (% p) ，则a≡c (% p)
    
【运算规则】    
模运算与基本四则运算有些相似，但是除法例外。其规则如下：
1.         (a + b) % p = (a % p + b % p) % p 
           (a - b) % p = (a % p - b % p) % p 
           (a * b) % p = (a % p * b % p) % p 

2.         a ^ b % p = ((a % p)^b) % p 

3. 结合律： ((a+b) % p + c) % p = (a + (b+c) % p) % p 

4.         ((a*b) % p * c)% p = (a * (b*c) % p) % p 

5. 交换律： (a + b) % p = (b+a) % p 
           (a * b) % p = (b * a) % p 

6.分配律： ((a +b)% p * c) % p = ((a * c) % p + (b * c) % p) % p 


重要定理: 若a ≡ b (% p)，则对于任意的c，都有(a + c) ≡ (b + c) (%p)；

（10） 若a≡b (% p)，则对于任意的c，都有(a * c) ≡ (b * c) (%p)；

（11） 若a≡b (% p)，c≡d (% p)， 则 (a + c) ≡ (b + d) (%p)，(a - c) ≡ (b - d) (%p)， 
                                  (a * c) ≡ (b * d) (%p)，(a / c) ≡ (b / d) (%p)；
```



## 第2章. 数据结构

```c++
struct Node{
 	int value;
 	Node* next;
};      
new Node();  非常慢  // 这样的链表实现方式在面试中用得多，但笔试中很少。因为效率低，且有些语言有内置标准函数。
		           // 这里是强调用数组来模拟链表、栈、队列；提高效率
```

### 单链表



### 双链表































































## 第5章.动态规划 [无模板]

动态规划（DP）是一种思想，没有固定的代码模板。但有一步一步的步骤。 **重点是状态表示和状态转移方程**。

### 背包问题

状态划分是看最后一个物品的选法来划分。

#### 0-1背包：

Dp{ 状态表示 f(i, j) ：{只从前 i 个物品中选, 总体积 ≤ j 的集合}  {属性：Max / Min / 数量}  

​     { 状态计算 ：集合划分 <要不重不漏> ：  f(i, j)  ← Max ( 不含i [ f(i-1, j) ]  |  含i [ f(i-1, j-v~i~) + w~i~ ]  )

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310250009868.png)

> 有 N件物品和一个容量是 V的背包。每件物品**只能使用一次**。  第 i 件物品的体积是 v~i~，价值是 w~i~。
>
> 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。 输出最大价值。

```c++
// 二维表示
int n,m;
int v[N], w[N];
int f[N][N];
int main(){
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> v[i] >> w[i];     // 如果涉及到[i-1],i下标最好从1开始，防止i-1越界
    
    for(int i=1; i<=n; i++) {     // f[0][0~n]肯定都是0，所以从1开始
        for(int j=0; j<=m; j++) {
            f[i][j] = f[i-1][j];     // 不含i的情况一定存在.
            if(j >= v[i]) {          // 当前体积要装得下vi，才会含i
                f[i][j] = max(f[i][j], f[i-1][j-v[i]] + w[i]);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}

// 转成一维优化(对代码做等价变形)： [因为f(i)只用到了f(i-1)∴滚动数组;且始终在j的左边，∴可用一维数组]
int n,m;
int v[N], w[N];
int f[N];      // f[j]
int main(){
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> v[i] >> w[i];
    
    for(int i=1; i<=n; i++) {     // f[0][0~n]肯定都是0，所以从1开始
        for(int j=m; j >= v[i]; j--) {   // 将循环从大到小，。
                f[j] = max(f[j], f[j-v[i]] + w[i]); 
            //  等价于 f[i][j] = max(f[i-1][j], f[i-1][j-v[i]] + w[i]);
            /* 如果从小到大, ∵ j-vi < j,先被运算 ∴求第i层的f[j]时，里面的f[j-vi]已经是第i层的值,但要求第i-1层；
               ∴改成从大到小循环，这样求第i层f[j]值时，f[j-vi]值还保留着第i-1层的值。                       */
            }
        }
    }
    cout << f[m] << endl;
    return 0;
}
```

#### 完全背包问题：

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310250133681.png)

f[i, j]对应有选择0个物品i，选择1个物品i，...，选择k个物品i。

> 有 N 种物品和一个容量是 V 的背包，每种物品都有**无限件**可用。    第 i 种物品的体积是 vi，价值是 wi。
>
> 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。    输出最大价值。

```c++
// 朴素做法（DP）-多重循环  {复杂度太高，会TLE}
int n,m;
int v[N],w[N];
int f[N][N];
int main() {
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> v[i] >> w[i];
    for(int i=1; i<= n; i++) {       // 对于前i个物品的所有选法
        for(int j=0; j<=m; j++) {    // 对于体积j
            for(int k=0; k*v[i] <= j; k++) {   // 选择k个物品i (注意体积限制)
                f[i][j] = max(f[i][j], f[i-1][j-v[i]*k] + w[i]*k);  // ∵k可以为0，∴包含f[i-1][j]
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
//————————————————————————————————————————————————————————————————————————————————————————————————————-————//
// 优化成二维
int main() {
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> v[i] >> w[i];
    for(int i=1; i<= n; i++) {       
        for(int j=0; j<=m; j++) {    //对状态计算表达式优化后，不用进行k个的循环 
        	f[i][j] = f[i-1][j]; 
            if(j >= v[i]) f[i][j] = max(f[i][j], f[i][j-v[i]] + w[i]);   //
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
//——————————————————————————————————————————————————————————————————————————————————————————————————————————//
// 再优化成一维（直接把 i 层删去）
int f[N];
int main() {
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> v[i] >> w[i];
    
    for(int i=1; i<= n; i++) {       
        for(int j=v[i]; j<=m; j++) {    // 完全背包是从小到大循环。 [0-1背包是从大到小]  ∵j-vi < j  
            f[j] = max(f[j], f[j-v[i]] + w[i]);   
            // f[i][j] = max(f[i-1][j], f[i][j-v[i]] + w[i]);
            /* 如果从小到大, ∵ j-vi < j,先被运算 ∴求第i层的f[j]时，里面的f[j-vi]已经是第i层的值。  */
        }
    }
    cout << f[m] << endl;
    return 0;
}
```

> 对状态计算表达式的优化 ↓ ：  不需要进行k的循环

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310250146958.png)

状态转移方程对比：  ①0-1背包： `f[i][j] = Max(f[i-1][j], f[i-1][j-v[i]] + w[i])`

​				  ②完全背包：`f[i][j] = Max(f[i-1][j], f[i][j-v[i]] + w[i])`

#### 多重背包问题

> 有 N 种物品和一个容量是 V 的背包。  **第 i 种物品最多有 s~i~ 件**，每件体积是 vi，价值是 wi。 {不同种物品的数量不同}
>
> 求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。  输出最大价值。
>
> #### 数据范围
>
> 0< N ≤1000
> 0< V ≤2000
> 0< vi, wi, si ≤2000

```c++
//  朴素暴力解法
int n,m;
int v[N], w[N], s[N];
int f[N][N];
int main() {
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> v[i] >> w[i] >> s[i];
    
    for(int i=1; i<= n; i++) {       // 对于前i个物品的所有选法
        for(int j=0; j<=m; j++) {    // 对于体积j
            for(int k=0; k <= s[i] && k*v[i] <= j; k++) {   // 选择k个物品i (注意体积限制)
                f[i][j] = max(f[i][j], f[i-1][j-v[i]*k] + w[i]*k);  // ∵k可以为0，∴包含f[i-1][j]
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```

> **多重背包的二进制优化方法：**
>
> 对于一个整数 s ，       使用： 1，2，4，8，...，2^k^ ，c ; (c < 2^k+1^) 即可凑出 0 ~ s 之间任意的整数 [且每个数至多用一次]。
>
> 即 给予 s 个物品， 可以将其拆分成 log(s)组物品。 此时每组物品看成新的完整个体， 再进行0-1背包。

```c++
// 多重背包的二进制优化：
const int N = 25000, M = 2010;    // 物品总个数 N 最多是 1000*[log(2000)]向上取整 = 1000 * 12
int n,m;
int v[N], w[N];
int f[N];         // 一维优化 
int main() {
    cin >> n >> m;
    int cnt = 0;
    for(int i=1; i<=n; i++) {        // 二进制拆分个数
        int a, b, s;
        cin >> a >> b >> s;     // 体积、价值、个数
        int k=1;               // 从1开始对s进行拆分 [k就是1,2,4,8,..,2^k]
        while(k <= s) {
            cnt++;
            v[cnt] = a*k;     // 每次是把 k 个第i个物品打包在一起，看成新的物品
            w[cnt] = b*k;
            s -= k;
            k *= 2;        //
        }
        if(s > 0) {        // 拆分完剩下的c
            cnt++;
            v[cnt] = a*s;
            w[cnt] = b*s;
        }
    }
    
    n = cnt;    // 记录共拆分成了多少组
    
    for(int i=1; i<=n; i++) {     // 做0-1背包问题
        for(int j=m; j >= v[i]; j--) {   // 将循环从大到小
                f[j] = max(f[j], f[j-v[i]] + w[i]);
            }
        }
    }
    cout << f[m] << endl;
    return 0;
}
```

#### 分组背包

> 有 N 组物品和一个容量是 V 的背包。   **每组物品有若干**个，**同一组内的物品最多只能选一个**。
> 每件物品的体积是 vij，价值是 wij，其中 i 是组号，j 是组内编号。
>
> 求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。输出最大价值。

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310250304256.png)

```c++
int n, m;
int v[N][N], w[N][N], s[N];
int f[N];   // 可以从两维优化到一维  【转移时如果用的上一层体积，就从大到小； 如果用本层的就从小到大】
int main() {
     cin >> n >> m;

    for (int i = 1; i <= n; i ++ )
    {
        cin >> s[i];
        for (int j = 0; j < s[i]; j ++ )
            cin >> v[i][j] >> w[i][j];
    }

    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= 0; j -- )
            for (int k = 0; k < s[i]; k ++ )    // 选择第i组的第k个物品
                if (v[i][k] <= j)         //
                    f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);         // k可为0，∴包含f[i-1][j]
    			   // f[i][j] = max(f[i][j], f[i-1][j-v[i][k]] + w[i][k])

    cout << f[m] << endl;

    return 0;
}
```

### 线性DP(动规)

#### 题目：数字三角形

> 给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。         【状态划分根据最后一步从哪个方向来的进行划分】
>
> ```c
>                     7
>                   3   8
>                 8   1   0
>               2   7   4   4
>             4   5   2   6   5
> ```

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310251325339.png)

时间复杂度：状态总数量 × 状态转移计算量。        如上题就是 n^2^ × O(1) = O(n^2^)

```c++
const int N=510, INF=1e9;
int n;
int a[N][N];
int f[N][N];
int main() {
    cin >> n;
    for(int i=1; i<=n; i++) 
        for(int j=1; j<=i; j++)    // j截止到i
            cin >> a[i][j];
    
    for(int i=0; i<=n; i++)       // 对状态初始化成负无穷值 (∵边界问题，最左侧和最右侧的点也要计算来自左上/右上的值)
        for(int j=0; j<=i+1; j++)   // j截止到i+1，多一列 (每行多初始化一个)
            f[i][j] = -INF;   
    
    f[1][1] = a[1][1];        // 第一个是确定的
    for(int i=2; i<=n; i++) 
        for(int j=1; j<=i; j++)
            f[i][j] = max(f[i-1][j-1] + a[i][j], f[i-1][j] + a[i][j]);
            
    int res = -INF;
    for(int j=1; j<=n; j++) 
        res = max(res, f[n][j]);  // ∵最后一行的每个点都可能是终点，∴遍历最后一行，挑出路径最大值的终点
    
    cout << ret << endl;
    return 0;
}
```

#### 最长上升子序列

> 给定一个长度为 N 的数列，求数值严格单调递增的子序列的长度最长是多少。   
>
> 【状态划分根据倒数第二个数来划分】

注：状态表示的维数越少越好，避免复杂度过高。先从一维开始考虑能否表示。

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310251750917.png)

以倒数第二个数为划分依据。

```c++
//  时间复杂度：O(n^2)
int n;
int a[N];
int f[N];      // 一维即可表示
int main() {
    cin >> n;
    for(int i=1; i<=n; i++) cin >> a[i];
    
    for(int i=1; i<=n; i++) {
        f[i] = 1;     // 子序列只有a[i]一个数
        for(int j=1; j < i; j++) {
            if(a[j] < a[i]) {
                f[i] = max(f[i], f[j]+1);    // 选择添加当前数a[i]并比较长度
            }
        }
    }
    
    int res = 0;
    for(int i=1; i<=n; i++) res = max(res, f[i]);
    
    cout << res;
    
    return 0;
}
// ——————————————————————————————————————————————————————————————————————————————————————————————————————//
// 记录最长子序列本身
int n;
int a[N];
int f[N];      
int g[N];      // 记录存储状态转移
int main() {
    cin >> n;
    for(int i=1; i<=n; i++) cin >> a[i];
    
    for(int i=1; i<=n; i++) {
        f[i] = 1;    
        for(int j=1; j < i; j++) {
            if(a[j] < a[i]) {
                if(f[j]+1 > f[i]) {
                    f[i] = f[j] + 1;
                    g[i] = j;      // 将上一个转移的数对应序号存下来
                }    
            }
        }
    }
    
    int k = 1;
    for(int i=1; i<=n; i++) {
        if(f[k] < f[i]) {
            k = i;
        }
    }
    cout << f[k];
    
    for(int i=0, len=f[k]; i<len; i++) {   // 逆序输出最长子序列
        cout << a[k];
        k = g[k];
    }
    
    return 0;
}
```

#### 最长公共子序列

> 给定两个长度分别为 N 和 M 的字符串 A 和 B，求既是 A 的子序列又是 B 的子序列的字符串长度最长是多少。
>
> 【状态划分根据两个字符串最后的字母来分类】

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310251933841.png)

**状态划分**很重要。  一般不写f[i-1, j-1]的情况，因为被包含在 f[i-1, j] , f[i, j-1]中。且f[i-1, j]并不完全对应不选a[i]一定选b[i]的情况，而是包含这种情况，但由于求的是Max，所以出现包含重叠的关系并不影响最终的结果。 【如果是求Max, Min可以包含重叠，但求数量时不行】

```c++
int n, m;
char a[N], b[N];    // 表示两个字符串A，B [注意类型]
int f[N][N];
int main()
{
    cin >> n >> m;
    scanf("%s%s", a + 1, b + 1);   // ∵状态转移的下标中用到了i-1, ∴下标从1开始.

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )   // '11'的情况不一定出现，需要a[i] = b[i]且满足条件。
        {
            f[i][j] = max(f[i - 1][j], f[i][j - 1]);     // '01'与'10'的情况
            if (a[i] == b[j]) {
                f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);    // 讨论'11'的情况
            }
        }

    cout << f[n][m] << endl;

    return 0;
}
```

#### 最短编辑距离

> 给定两个字符串 A 和 B，现在要将 A 经过若干操作变为 B，可进行的操作有：
>
> 1. 删除–将字符串 A中的某个字符删除。
> 2. 插入–在字符串 A 的某个位置插入某个字符。
> 3. 替换–将字符串 A 中的某个字符替换为另一个字符。
>
> 现在请你求出，将 A 变为 B 至少需要进行多少次操作。

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202310251955537.png)

```c++
int n, m;
char a[N], b[N];
int f[N][N];
int main()
{
    scanf("%d%s", &n, a + 1);    // 有i-1，∴下标从1开始
    scanf("%d%s", &m, b + 1);

    for (int i = 0; i <= m; i ++ ) f[0][i] = i;   // 初始化，需要i步  [空字符串通过增变成i个字符]
    for (int i = 0; i <= n; i ++ ) f[i][0] = i;   // 

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            // 考虑'删'与'增'的情况       
            f[i][j] = min(f[i-1][j] + 1, f[i][j-1] + 1);   
            // 考虑'改'的情况
            if (a[i] == b[j]) {              // 最后的字符相等就不用改了              
                f[i][j] = min(f[i][j], f[i - 1][j - 1]);
            } else {
                f[i][j] = min(f[i][j], f[i - 1][j - 1] + 1);
            }
        }

    cout << f[n][m] << endl;

    return 0;
}
```

#### 编辑距离

> 给定 n 个长度不超过 1010 的字符串以及 m 次询问，每次询问给出一个字符串和一个操作次数上限。
>
> 对于每次询问，请你求出给定的 n 个字符串中有多少个字符串可以在上限操作次数内经过操作变成询问给出的字符串。
>
> 每个对字符串进行的单个字符的插入、删除或替换算作一次操作。

```c++
const int N = 15, M = 1010;
int n, m;
char str[M][N];
int f[N][N];
int edit_distance(char a[], char b[])     // 求编辑距离 (将a字符串与b字符串进行比较)
{
    int len_a = strlen(a + 1), len_b = strlen(b + 1);

    for (int i = 0; i <= len_b; i ++ ) f[0][i] = i;    // 初始化
    for (int i = 0; i <= len_a; i ++ ) f[i][0] = i;

    for (int i = 1; i <= len_a; i ++ )             // 直接DP (同最短编辑距离)
        for (int j = 1; j <= len_b; j ++ )
        {
            f[i][j] = min(f[i-1][j] + 1, f[i][j-1] + 1);     // '删'、'增'的情况。
            f[i][j] = min( f[i][j], f[i-1][j-1] + (a[i] != b[j]) );    // '改'的情况。
        }

    return f[len_a][len_b];
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ ) scanf("%s", str[i] + 1);   // 下标从1开始，方便

    while (m -- )
    {
        char s[N];
        int limit;
        scanf("%s%d", s + 1, &limit);

        int res = 0;
        for (int i = 0; i < n; i ++ )
            if (edit_distance(str[i], s) <= limit)
                res ++ ;

        cout << res << endl;
    }

    return 0;
}
```



### 区间DP





## 第6章.贪心 [无模板]

贪心算法（Greedy Algorithm）属于一种思想。没有固定套路，需要积累。猜解法（直觉上猜一个解法）+证明正确（可用反证法、归纳法、== → ≥ 且 ≤）。    代码比较好写，难点在于证明贪心算法的正确性。    [但考试常常是把一个问题转化成之前见过的题而不是解决全新的题目。常考的贪心模型就那几种，但每种的证明都很难证，记住模型也行]

### ①区间问题

#### 区间选点

> 给定 N 个闭区间 [ai,bi]，请你在数轴上**选择尽量少的点**，使得**每个区间内至少包含一个选出的点**。
>
> 输出选择的点的最小数量。    （位于区间端点上的点也算作区间内）

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202311052154634.png" style="zoom: 50%;" />

```c++
const int N = 100010;
int n;
struct Range        // 定义一个结构体来记录一个区间
{
    int l, r;       // 一个区间的左右端点
    bool operator< (const Range &W)const      // 重载 '<' 运算符来实现按右端点大小排序
    {
        return r < W.r;
    }
}range[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d%d", &range[i].l, &range[i].r);
    sort(range, range + n);       // 将所有区间按右端点从小到大排序
    int res = 0, ed = -2e9;       // res记录选择点的数量，ed表示上一次选的点的值 （初始设为负无穷）
    for (int i = 0; i < n; i ++ )
        if (range[i].l > ed)     // 当前区间不包含选取的点
        {
            res ++ ;
            ed = range[i].r;
        }

    printf("%d\n", res);
    return 0;
}
```

#### 最大不相交区间数量

> 给定 N 个闭区间 [ai,bi]，请你在数轴上**选择若干区间**，使得**选中的区间之间互不相交**（包括端点）。
>
> 输出可**选取区间的最大数量**。

注：这种题转换成实际问题就可以是“让一段时间内选择活动数最多”。  **注意将问题转换成遇到的问题or模型**

```c++
const int N = 100010;
int n;
struct Range
{
    int l, r;
    bool operator< (const Range &W)const
    {
        return r < W.r;
    }
}range[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d%d", &range[i].l, &range[i].r);
    sort(range, range + n);    // 按照右端点的从小到大排序
    int res = 0, ed = -2e9;     
    for (int i = 0; i < n; i ++ )
        if (ed < range[i].l)    // 与上一题不同的地方：当前区间需要不与上次右端点相交，算作选中的区间之间互不相交
        {
            res ++ ;
            ed = range[i].r;    // 记录所选区间的右端点
        }

    printf("%d\n", res);

    return 0;
}
```

#### 区间分组

> 给定 N个闭区间 [ai,bi]，请将这些**区间分成若干组**，使得**每组内部**的区间两两之间（包括端点）**没有交集**，（组间可以有交集）并使**组数尽可能小**。                    输出最小组数。

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202311052225816.png" style="zoom:50%;" />

```c++
const int N = 100010;
int n;
struct Range
{
    int l, r;
    bool operator< (const Range &W)const   
    {
        return l < W.l;     // 按左端点排序
    }
}range[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        range[i] = {l, r};
    }
    sort(range, range + n);   // 所有区间按左端点从小到大排序

    priority_queue<int, vector<int>, greater<int>> heap;  // 用小根堆来维护所有组的右端点 [小根堆语法就如此]
    for (int i = 0; i < n; i ++ )    //从前往后处理一下每个区间，
    {
        auto r = range[i];
        if (heap.empty() || heap.top() >= r.l) {  //堆为空-目前一个组都没有or堆顶-所有组右端点的最小值都与当前区间有交集
            heap.push(r.r);
        }  else {        // 否则当前区间可放进组里
            heap.pop();   // 弹出当前要加入组的右端点
            heap.push(r.r);    // 更新一下组的右端点Max_r
        }
    }

    printf("%d\n", heap.size());    // 输出分组数量
    return 0;
}
```

证明思路：   1.ans(最优答案) <=  cnt(按照贪心算法得到的数量)   2. ans >= cnt.      得证 ans == cnt

> 先证1.：按照上述贪心思路获得的答案一定是合法的方案，而ans是所有合法方案里的最小值，所以得 ans <= cnt;
>
> 再证2.：选择某一时刻比如对第cnt组区间进行分组，此时枚举时该区间与之前的`cnt-1`组都有交点，算上该区间，则有cnt个区间的交集不为空，有公共点。即所有合法方案中这cnt个区间一定都在单独的组里。 所以得 ans >= cnt; 

#### 区间覆盖

> 给定 N 个闭区间 [ai,bi] 以及一个线段区间 [s,t]，请你**选择尽量少的区间**，**将指定线段区间完全覆盖**。（覆盖的是实数）
>
> 输出最少区间数，如果无法完全覆盖则输出 −1。

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202311052304524.png)

```c++
const int N = 100010;
int n;
struct Range
{
    int l, r;
    bool operator< (const Range &W)const
    {
        return l < W.l;     // 区间按左端点从小到大排序
    }
}range[N];

int main()
{
    int st, ed;       // 目标线段区间    st-start, ed-end
    scanf("%d%d", &st, &ed);    
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        range[i] = {l, r};
    }
    sort(range, range + n);     // 所有区间按左端点从小到大排序

    int res = 0;                   // 记录选取的区间数
    bool success = false;       // 无法完全覆盖——选取的最后一个区间无法覆盖目标end的情况
    for (int i = 0; i < n; i ++ )
    {
        int j = i, r = -2e9;    // r表示右端点最大值（初始为负无穷）
        while (j < n && range[j].l <= st)  //所有能覆盖start的区间中
        {
            r = max(r, range[j].r);    // 选择右端点最大的区间
            j ++ ;
        }

        if (r < st)              // 无法完全覆盖——没有能覆盖st的区间的情况
        {
            res = -1;
            break;      // 结束
        }

        res ++ ;
        if (r >= ed)      //选取的最后一个区间覆盖掉目标区间的end，结束循环
        {
            success = true;
            break;
        }

        st = r;         // 更新目标start。  （注意题目要覆盖的是区间内全体实数，而不是全体整数，注意判断边界）
        i = j - 1;     // 更新i，更新下次循环的位置（避免重复遍历区间，j循环过的就不用再遍历了）
    }

    if (!success) res = -1;   // success的存在是因为无法完全覆盖有两种可能情况，一种没能覆盖st，另一种没覆盖ed；
    printf("%d\n", res);

    return 0;
}
```

证明思路：

> 先证1. ans <= cnt：按上述贪心算法一定可得一个合法方案，结果记为cnt；ans为所有方案结果的最小值。 所以ans一定≤cnt
>
> 再证2.ans >= cnt：思路就是对最优方案一定可通过替换变成贪心方案。如下图，对比两种对应方案，从前往后找到每一个不一样的区间；比如对于L1与L2，由贪心算法得出的L2的右端点一定大于L1,又L3左端点严格小于L2，所以把最优解方案里的区间L1替换成算法所取的区间L2。替换后还是最优解且区间数量不变。依此类推，不断替换，最终还是一样，所以 ans ==(>=) cnt。

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202311052321215.png" style="zoom:67%;" />

### ②Huffman树 问题

#### 合并果子

> 一个果园里，达达已将所有的果子打了下来，且按果子的不同种类分成了不同的堆。
>
> 达达决定把所有的果子合成一堆。每一次合并，达达可以把**两堆果子合并**到一起，**消耗的体力等于两堆果子的重量之和**。
>
> 可看出，所有的果子经过 n−1次合并之后，就只剩下一堆了。达达在合并果子时**总共消耗的体力等于每次合并所耗体力之和**。
>
> 达达在合并果子时**要尽可能地节省体力**。
>
> **假定每个果子重量都为 1**，且已知果子的种类数和每种果子的数目，你的任务是**设计出合并的次序方案**，使达达耗费的体力最少，并输出这个最小的体力耗费值。
>
> 例如有 3 种果子，数目依次为 1，2，9。
>
> 可以先将 1、2 堆合并，新堆数目为 3，耗费体力为 3。接着将新堆与原先的第三堆(9)合并，又得新堆，数目为 12，耗费体力为 12。    所以达达总共耗费体力=3+12=15=3+12=15。   可证 15为最小的体力耗费值。

```c++
/*(贪心,哈夫曼树,堆,优先队列)   O(nlogn)
	经典哈夫曼树的模型，每次合并重量最小的两堆果子即可。
时间复杂度
	使用小根堆维护所有果子，每次弹出堆顶的两堆果子，并将其合并，合并之后将两堆重量之和再次插入小根堆中。每次操作会将果子的堆数减一，一共操作 n−1次即可将所有果子合并成1堆。每次操作涉及到2次堆的删除操作和1次堆的插入操作，计算量是 O(logn)。因此总时间复杂度是 O(nlogn)*/
int main()
{
    int n;
    scanf("%d", &n);
    priority_queue<int, vector<int>, greater<int>> heap;    // 使用小根堆
    while (n -- )
    {
        int x;    
        scanf("%d", &x);    // 第i种果子的数目
        heap.push(x);       // 插入堆中
    }

    int res = 0;         // 记录答案
    while (heap.size() > 1)     // 只要堆中元素大于1就要进行合并
    {
        int a = heap.top();     // 每次合并把堆中最小的两个值取出来，为a,b。 
        heap.pop();
        int b = heap.top(); 
        heap.pop();
        
        res += a + b;       // 加到体力消耗里
        
        heap.push(a + b);   // 将合并后的值插入回堆中
    }

    printf("%d\n", res);
    return 0;
}
```

证明思路

> 先证每次合并两个最小值是最优解：最小的两个点深度一定最深，且可互为兄弟(最深层的各叶节点可以互相交换）
>
> 再证 对于 n-1 的最优解 对于 n 仍是最优解： f(n) = f(n-1) + a + b; [因为第一步总是合并两个最小值] 两边减去ab后递归证明          

### ③排序不等式

排序不等式表述如下。设有两数列$a_1, a_2, a_3, ..., a_n$和$b_1, b_2, b_3, ..., b_n$，满足$a_1 \leq a_2 \leq ... \leq a_n$， $b_1 \leq b_2 \leq ... \leq b_n$ ，

有 $c_1, c_2, ..., c_n$是$b_1, b_2, ..., b_n$  的乱序排列，则有：      [最小×最小 <= ... <=最大×最大]

​						$\sum\limits_{i=1}^{n}a_ib_{n+1-i} \leq \sum\limits_{i=1}^{n}a_ic_i \leq \sum\limits_{i=1}^{n}a_ib_i$             (当且仅当$a_i = a_j$或$b_i=b_j(1\leq i,j \leq n)$时等号成立)                      

#### 排队打水

> 有 n个人排队到 1个水龙头处打水，**第 i 个人装满水桶所需的时间是 ti**，请问如何**安排他们的打水顺序**才能使**所有人的等待时间之和最小**？  
>
> 示例：   排队打水时间顺序：           3  ，  6  ，  1，  4  ， 2
>
> ​              对应每个人的等待时间：    0  ，  3，    9， 10 ，14
>
> ​             总共用时：`3*4 [3被4个人等待] + 6*3 [] + 1*2 + 4*1`

总等待时间： t = t[1] × (n-1) + t[2] × (n-2) + t[3] × (n-3) + ... + t[n-1] ×1 ；  

最优解：按照排队时间从小到大排序，则总等待时间一定最小。

```c++
typedef long long LL;
const int N = 100010;
int n;
int t[N];
int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &t[i]);

    sort(t, t + n);      // 按照打水时间排序从小到大排序
    
    reverse(t, t + n);     // reverse是方便后续for循环里面 计算直接 t[1]×1 + t[2]×2 + ...
    					 // t的顺序反过来的时候从小到大遍历乘起来比较方便，不reverse的话就每次乘n - i -1
    LL res = 0;         // 该题目结果可能爆int
    for (int i = 0; i < n; i ++ ) 
        res += t[i] * i;      // 按照用时公式计算  [也可以直接前缀和计算，注意不要加最后一组，自己接水不算等待]

    printf("%lld\n", res);
    return 0;
}
```

证明思路

> 反证法：假设不按从小到大的顺序，则必然存在两个相邻的人，前者时间比后者大，即t[i] > t[i+1]。 交换前用时T1：t[i] × (n-i) +  t[i+1] × (n-i-1) ； 交换后用时T2：t[i+1] × (n-i) + t[i] × (n-i-1)； 又T1-T2 = t[i] - t[i+1] > 0，所以比最优解差。

### ④绝对值不等式

绝对值不等式：        $\vert \vert a \vert - \vert b \vert \vert \leq \vert a \pm b \vert \leq \vert a \vert + \vert b \vert$         （注意等号成立条件）

​                                  $\vert a - c \vert + \vert b - c \vert \geq \vert a - b \vert$

#### 货仓选址

> 在一条数轴上有 N 家商店，它们的坐标分别为 A1∼AN。 现在需要在数轴上建立一家货仓，每天清晨，**从货仓到每家商店**都要运送一车商品。    为了提高效率，求**把货仓建在何处**，可以**使得货仓到每家商店的距离之和最小**。 
> 输出一个整数，表示距离之和的最小值。

距离： f(x) = |x1 - x| + |x2 - x| + ... + |xn - x|  ; （假设货仓在数轴上坐标为x）   求f(x)最小值：  [分组后由绝对不等式放缩]

​	f(x) = (|x1-x| + |xn-x|) + (|x2-x| + |xn-1 - x|) + ...  ≥  |xn-x1| + |xn-1 - x2| + ...   (所有等号成立条件：x为中位数或中间两个数的中间)

```c++
const int N = 100010;
int n;
int q[N];
int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )   scanf("%d", &q[i]);

    sort(q, q + n);      // 先将 x1~xn按从小到大数轴上排序

    int res = 0;        // 记录货仓坐标位置
    for (int i = 0; i < n; i ++ ) 
        res += abs(q[i] - q[n / 2]);    // q[n/2]即中位数位置(货仓选址点)，求距离之和。

    printf("%d\n", res);
    return 0;
}
```

### ⑤推公式

很多贪心问题是先把公式推出来，然后从公式的角度出发利用一些不等式的原理（比如均值、调和、柯西、排序不等式）来得到最优解。

贪心涉及到的模型绝大部分都是在数学里研究过的。

#### 耍杂技的牛

> 农民的 N 头奶牛（编号为 1..N），提出了一个杂技表演：**叠罗汉**，表演时，奶牛们站在彼此的身上，形成一个高高的垂直堆叠。
>
> 奶牛们正试图找到自己在这个堆叠中应该所处的位置顺序。这 **N 头奶牛**中**每一头都有着自己的重量 Wi 以及自己的强壮程度 Si**。
>
> 一头牛支撑不住的可能性取决于**它头上所有牛的总重量**（不包括它自己）**减去它的身体强壮程度的值**，现在称**该数值为风险值**，风险值越大，这只牛撑不住的可能性越高。
>
> 您的任务是**确定奶牛的排序**，使得**所有奶牛的风险值中的最大值尽可能小**(每头牛都有对应风险值，找出其中的最大值。确保算法保证的该最大值尽可能小)。                      输出一个整数，表示最大风险值的最小可能值。

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202311060130094.png" style="zoom:67%;" />

```c++
typedef pair<int, int> PII;
const int N = 50010;
int n;
PII cow[N];
int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
    {
        int s, w;
        scanf("%d%d", &w, &s);
        cow[i] = {w + s, w};      // <wi+si, w> 便于排序
    }

    sort(cow, cow + n);   // 按照 wi+si从小到大的顺序排序，同时也是从上到下叠罗汉的顺序

    int res = -2e9, sum = 0;
    for (int i = 0; i < n; i ++ )
    {
        int s = cow[i].first - cow[i].second;
        int w = cow[i].second;
        res = max(res, sum - s);   // 首先求出当前已求出的所有风险值的最大值 （sum-s是当前位置的风险值）
        sum += w;   // sum记录从最上层当这一层的所有重量之和
    }

    printf("%d\n", res);
    return 0;
}
```

证明思路：

> 假设最优解不是按照贪心算法，则必然存在相邻的两头牛使 wi+si > w[i+1] + s[i+1];    则 交换前：第i个位置上的牛 风险值D[i] = w1 + ...+wi-1 - si ;  第i+1个位置上的牛 D[i+1] = w1+...+wi - si+1  ;    交换后：D[i]’ = w1+...+wi-1 - si+1 ;  D[i+1]’ = w1+...+wi-1 + wi+1 - si  ;            整理把D值删去相同数并同时加上si与si+1变为正数 →  交换前：D[i] = s[i+1] ,  D[i+1] = wi + si;
>
> 交换后：D[i]’ = s[i] , d[i+1]’ = w[i+1] + s[i+1] ;又有wi+si > w[i+1] + s[i+1]  > {si, si+1}，所以 max(D[i], D[i+1]) > max(D[i]’, D[i+1]’) 
>
> 所以交换位置后可以让风险值的最大值尽可能小。依此类推，所有相邻都交换后,最终一定会变成wi+si从小到大的顺序，即贪心算法得到的答案 ≤ 最优解， 即证贪心即是最优解。       
