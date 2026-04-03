```c++
## 二叉树的数据结构
class TreeNode {
	T val;
	TreeNode* left;
	TreeNode* right;
}

void printTraverse(TreeNode* input) {
	std::vector<T> res;
	TreeNode* curr_node = input;
	bool fromLeftToRight = true; //记录从左往右打印还是从右往左打印
	if(input == nullprt) {
		std::cout << ...
	}
	std::queue<TreeNode*> curr_queue;
  curr_queue.push_back(input);
	while(!curr_queue.empty()) {
		int capacity = curr_queue.size(); // 因为拿出来的时候还在往后放
		for(int i = 0; i < capacity; ++i) {
				TreeNode* curr = curr_queue.pop()
				if(fromLeftToRight){
					res.push_back(curr->val);
				} else {
					res.insert(res.size()-i, curr->val);
				}
				if(curr_node->left!=nullptr) {
					curr_queue.push_back(curr_node->left);		
				}
				if(curr_node->right!=nullptr) {
					curr_queue.push_back(curr_node->right);		
				}
		}
		fromLeftToRight = !fromLeftToRight
	}
}
```

