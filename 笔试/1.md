**2019/11/4 15:12**  
记录编程提高过程，加油～
- [x] Acwing：879. 幸存者游戏 ===> 约瑟夫环问题（一般的约瑟夫环问题，可以用dp来解决，时间复杂度为o(N)）
有n个同学围成一圈，其id依次为1～n（n号挨着1号）。
现在从1号开始报数，第一回合报到m的人就出局，第二回合从出局的下一个人开始报数，报到m^2	的同学出局。
以此类推，直到最后一个回合报到m^(n - 1)的人出局，剩下最后一个同学。
输出这个同学的编号。

输入格式
共一行，包含两个整数n和m。

输出格式
输出最后剩下的同学的编号。

数据范围
n <= 15, m <= 5

输入样例：
5 2

输出样例：
5
> 时间限制：1秒  空间限制：64MB
```cpp
/*
题目分析：模拟问题。如果直接暴力解题，时间复杂度太高，进行一定的剪枝，被淘汰人的数字超过一圈时，取模，
这样每次数数时，最多只需数一圈.时间复杂度o(n^2)，循环n圈，每次循环n个元素.
*/
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 16; // 一圈多少人

bool st[N];

int main(){
	int n, m;
	cin >> n >> m;

	int p = 1;
	for(int i = 1, r = n; i <= n; i++, r--){ // 迭代n次，等价于删除n次，最后一次被删除的人，就是最后剩下的人
		// r-- 表示最后一个元素的编号依次递减，也就是每删除一个人，一圈的总人数减1
		int k = 1; // k 用于表示m的i次方的结果
		for(int j = 1; j <= i; j++) k = k * m % r; // 对r取余，是暴力模拟的剪枝操作
		if(k == 0) k = r; // 特判，如果k == 0的话，说明删除的是最后一个元素r
		while(true){ 
			if(!st[p]){
				k--;
				if(!k){
					st[p] = true;
					break;
				}
			}
			p++;
			if(p > n) p = 1; // 如果超出一圈，p重新赋值为1
		}
	}
	cout << p << endl;
	return 0;
}

Solution 2:
/*
测试的过程中遇到 Float Point Exception错误，经查询，是由 % 0导致，出错位置为 
(i = 0, r = n; i <= n; i++, r--),因为i从0到n共循环了n + 1次（实际为n次），
所以r减少为0，下面的代码 int k = x % r，出现模0错误。
*/
#include <iostream>

using namespace std;

const int N = 16;

bool st[N];

int main(){
    int n, m;
    cin >> n >> m;
    int p = 1, x = m;
    for(int i = 1, r = n; i <= n; i++, r--){
        int k = x % r; //利用变量x存放m的r次方，减少了一层循环
        if(k == 0) k = r;
        while(true){
            if(!st[p]){
                k--;
                if(!k){
                    st[p] = true;
                    break;
                }
            }
            p++;
            if(p > n) p = 1;
        }
        x *= m;
    }
    cout << p << endl;
    return 0;
}

```
