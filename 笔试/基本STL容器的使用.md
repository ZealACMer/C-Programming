**2019-12-24 1:27**
记录编程提高过程，加油～
- [x] acwing:数字对生成树
根据输入生成一棵树，并打印该树。
输入为N行数字对序列（例如：N = 2， 数字对序列为(1, 2), (2, 3))，其中数字代表该树的节点，
左边数字代表的节点是右边数字代表的节点的父节点。
请根据输入的数字对序列生成一棵树，并根据广度优先的顺序打印该树。
注意，存在输入无法生成树的情况，例如(1, 2), (1, 3), (2, 4), (3, 4)，根据该序列生成的图中，
2节点和3节点同时为4节点的父节点，所以根据定义该图不是树。
如遇无法生成树的情况，请输出字符串“Not a tree"。
广度优先遍历树，并打印输出时，同一层级的节点根据输入时节点出现的顺序打印输出。

输入格式
第一行包含整数N。
接下来的N行，每行包含两个不同的正整数，表示一个数字对，两数之间用英文逗号分隔，例如：1, 2。
没有默认的输入顺序，第一行可能不是根节点，最后一行可能不是叶子节点。
可能有完全重复的行。

输出格式
输出共一行，如果可以生成树，请根据广度优先顺序，输出每个节点对应的数字，并用英文逗号隔开，同一层级
的节点根据输入顺序输出

如果无法生成树，请输出字符串“Not a tree”

数据范围：
1 <= N <= 10000
整数对中的整数均小于2的31次方

输入样例：
5
1,2
1,3
2,4
2,5
3,6
输出样例1：
1,2,3,4,5,6

输入样例2：
4
1,2
1,3
2,4
3,4
输出样例2：
Not a tree

输入样例3:
5
3,6
2,4
1,3
2,5
1,2
输出样例3：
1,3,2,6,4,5
```cpp
/*
STL set 动态维护有序序列
lower_bound(x) 返回>=x的最小的数
upper_bound(x) 返回>x的最小的数
upper_bound(x) 返回的是一个iterator，可以通过--或者++选择前后的元素
*/

#include <iostream>
#include <algorithm>
#include <map>
#include <vector>
#include <queue>

using namespace std;

map<int, vector<int>> h; //存储整个图，例如h[5]存储以5为起点的所有边
map<int, int> timestamp, dist;
map<int, bool> has_father;
map<pair<int, int>, bool> edges;

vector<int> bfs(int root){
	queue<int> q;
	q.push(root);
	dist[root] = 0;
	vector<int> nodes;

	while(q.size()){
		auto t = q.front();
		q.pop();

		nodes.push_back(t);

		for(auto ver : h[t]){
			if(dist.count(ver)) return vector<int>();
			dist[ver] = dist[t] + 1;
			q.push(ver);
		}
	}

	vector<vector<int>> ans;
	for(auto ver : nodes) ans.push_back({dist[ver], timestamp[ver], ver});

	sort(ans.begin(), ans.end());

    nodes.clear();
    for(auto t : ans) nodes.push_back(t[2]);

    return nodes;
}

int main(){
	int n;
	cin >> n;

	int tm = 0;
	while(n--){
		int a, b;
		scanf("%d,%d", &a, &b);

		if(edges[{a, b}]) continue; //过滤掉重复的边
		edges[{a, b}] = true;

		if(timestamp.count(a) == 0) timestamp[a] = tm++;
		if(timestamp.count(b) == 0) timestamp[b] = tm++;

		has_father[b] = true;
		h[a].push_back(b);
	}

	int root = -1;
	for(auto x : timestamp){
		int p = x.first;
		if(!has_father[p]){
			root = p;
			break;
		}
	}
	if(!root) puts("Not a tree");
	else{
		auto res = bfs(root);

		if(res.size() < timestamp.size()) puts("Not a tree");
		else{
			cout << res[0];
			for(int i = 1; i < res.size(); i++) cout << "," << res[i];
			cout << endl;
		}
	}
	return 0;
}
```
