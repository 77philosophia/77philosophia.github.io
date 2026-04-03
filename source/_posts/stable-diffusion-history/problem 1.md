# problem 1

array max sum连续子数组 return max sum

Ex: [-2,1,-3,4,-1,2,1,-5,4] ->6

定义：s[i]以index i为结尾的最长子数组的max sum

s[i] = if s[i-1] > 0 s[i-1]+s[i].  Or s[i]



```c++
int maxLianXuSum(const std::vector<int>& array) {
  std::vector<int> array_s;
  int max_sum = -9999;
  for(int i = 0; i< array.size(); ++i) {
    if(i > 0 &&s[i-1] > 0) {
      array_s[i] = array[i] + array_s[i-1];
    } else {
      array_s[i] = array[i]
    }
    max_sum = max(max_sum, array_s[i]);
  }
  return max_sum;
}
```



Pro 2:

活动start end

Max number activities no overlap -> retun num 

[1,3] 2.5 4,7.    6,8. -> 2



1,3  -> >3 -> 4,7and 6.8 -> add 4,7 ->找到的就是以1,3开头尽可能多的活动

2,5 -> 5-> 6,8 -> add 6,8 ....2,5

4,7 没有必要因为1,3的尽可能多的会包括4,7



6,8 开头 ->add 6,8 -> 1

4,7 -> 4,7 ->1

2,5 -> add 6, 8.  Ans[2,5] = ans[6,8]+1
1, 3 ->



6,8 -> 2,5 

4,7 ->1,3

2,5 ->

1,3 

```c++
class Activity {
  int start_time;
  int end_time;
  ...
  
}
int findMaxNumActivities(const std::vector<Activity>& act) {
  int n = act.size();
  // 1. 按照start time排序
  std::vector<Activity> act_by_start_time;
  for(int j = act.size()-1; j > 0; --j) {
    for(int i = 1; i< j; ++i) {
    	if(act[i].start_time<act[i-1].start_time) swap(act, i, i-1);
  	}
  }
  // 2.数组ans的定义：记录以活动i开头的max num act
  std::vector<int> ans;
  int has_compared_min_end_time;
  
  for(int i = n; i > 0; --i) {
		ans[i] = 1;
    // 找start time大于当前的end time的活动
    for(int j = i+1; j < n; ++j) {
      if(act[j].start_time > end_t) {
        ans[i] = max(ans[i], ans[j] +1);
      }
    }        
  }
  // 3. return max element in ans as return num
  int res = 0;
  for(int i = 0; i < ans.size(); ++i) {
    res = max(res, ans[i]);
  }
	return res;
}
```





Pro 3:

Array longest递增子序列的长度

10,9,2,5,3,7,101,18 -> 4



定义ans_array: index i表示以array[i]结束的子序列的最长的长度

Ans[0] =1;

Ans[i] = max(ans[i-k]...)+1 which array[i-k] < array[i]

```c++
int findLongestSequence(const std::vector<int>& array) {
  if(array.size() < 2) return array.size();
  std::vector<int> ans;
  ans[0] = array[0];
  for(int i = 1; i < array.size(); ++i) {
    ans[i] = 1;
 		for(int j = 0; j < i; ++j) {
      if(array[j] < array[i]) ans[i] = max(ans[i], ans[j]+1);
      
    }   
  }
  // find max element in ans
  int res = 0;
  for(int i = 0; i < ans.size(); ++i) {
    res = max(res, ans[i]);
  }
	return res;
}
```





n堆硬币 正整数

任意一堆硬币的顶部取一个硬币

取k个硬币硬币面值和的最大值

Ex1: 1,100,3.      7,8,9.    k=2       101

Ex2:

100

100

100

100

100

100

1，1，1，1，1，1，700

k=7

706



Ans:列数是n（硬币的堆数），行数 max_coin_num = 7

Ans: 每一列从上到下1，101， 104.。。

k = 1, vector = 100,100, ... ,1

k=2,