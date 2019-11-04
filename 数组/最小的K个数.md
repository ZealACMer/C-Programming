**2019/11/4 15:12**  
记录编程提高过程，加油～
- [x] 第七题：输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。
> 时间限制：1秒  空间限制：32768K
```cpp
class Solution{
    public:
        vector<int> GetLeastNumbers_Solution(vector<int> input, int k){
            vector<int> res;
            if(k > input.size()) return res;
            priority_queue<int> heap; 
            /*
            创建一个大根堆(first in, largest out),因为堆内有k个元素，
            所以时间复杂度为klogk
            */
            for(auto x : input){
                heap.push(x);
                if(heap.size() > k) heap.pop();
            }
            while(heap.size()) res.push_back(heap.top()), heap.pop();
            reverse(res.begin(), res.end());
            return res;
        }
};
```
