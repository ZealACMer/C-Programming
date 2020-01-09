**2019-1-9**
记录编程提高过程，加油～
- [x] acwing:摘花生
Hello Kitty想摘点花生送给她喜欢的米老鼠。

她来到一片有网格状道路的矩形花生地(如下图)，从西北角进去，东南角出来。

地里每个道路的交叉点上都有种着一株花生苗，上面有若干颗花生，经过一株花生苗就能摘走该它上面所有的花生。

Hello Kitty只能向东或向南走，不能向西或向北走。

问Hello Kitty最多能够摘到多少颗花生。

![Image text](https://cdn.acwing.com/media/article/image/2019/09/12/19_a8509f26d5-1.gif)

输入格式
第一行是一个整数T，代表一共有多少组数据。

接下来是T组数据。

每组数据的第一行是两个整数，分别代表花生苗的行数R和列数 C。

每组数据的接下来R行数据，从北向南依次描述每行花生苗的情况。每行数据有C个整数，按从西向东的顺序描述了该行每株花生苗上的花生数目M。

输出格式
对每组输入数据，输出一行，内容为Hello Kitty能摘到得最多的花生颗数。

数据范围
1≤T≤1001≤T≤100,
1≤R,C≤1001≤R,C≤100,
0≤M≤10000≤M≤1000
输入样例：
2
2 2
1 1
3 4
2 3
2 3 4
1 6 5
输出样例：
8
16
> 时间限制：1秒  空间限制：64MB
```cpp
#include<iostream>
#include<algorithm>
using namespace std;
const int N = 110;
int n,m;
int f[N][N],w[N][N];
int main(){
    int T;
    scanf("%d", &T);
    while(T--){
        scanf("%d %d", &n, &m);
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= m; j++)
                scanf("%d", &w[i][j]);
                
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= m; j++)
                f[i][j] = max(f[i-1][j], f[i][j-1]) + w[i][j];
        
        printf("%d\n",f[n][m]);
    }
    return 0;
}
// 动态规划的状态表示，如果是网格图的话,表示为f[i][j];如果是线性图的话，表示为f[i];
// 如果是背包问题，第一维是物品，第二维是体积
```
