# data-qustion
could not be solved myself

#include <iostream>
#include <fstream>
#include <stdlib.h>
#include <string>
using namespace std;

//定义二叉搜索树的结构体
typedef struct TreeNode {
	int data;//数据
	TreeNode* left;//左节点
	TreeNode* right;//右节点
}node;

// 创建一个新的二叉搜索树节点，进行初始化并返回
node* newNode(int data) {
	node* newNode = (node*)malloc(sizeof(node));
	newNode->data = data;
	newNode->left = newNode->right = NULL;
	return newNode;
}

// 在二叉搜索树中插入一个新的节点，并返回根节点
node* insert(node* root, int data) {
	if (root == NULL)
		return newNode(data);
	if (data < root->data)
		root->left = insert(root->left, data);
	else if (data > root->data)
		root->right = insert(root->right, data);
	return root;
}

// 在二叉搜索树中查找数据data，找到返回1，否则返回0
int search(node* root, int data) {
	while (root != NULL) {
		if (root->data == data)
			return 1;
		if (data > root->data)
			root = root->right;
		else
			root = root->left;
	}
	return 0;
}

int main() {
	clock_t start, finish;
	//clock_t为CPU时钟计时单元数
	start = clock();
	//clock()函数返回此时CPU时钟计时单元数
	ifstream ifs;        // 用于读取文件的输入流
	ofstream ofs;        // 用于写入文件的输出流
	string str;          // 存储从文件中读取的字符串
	char area[3000] = { 0 };
	node* root[3000] = { NULL };   // 二叉搜索树的根节点

	// 打开输入文件 "in.txt" 和输出文件 "out.txt"
	ifs.open("P02_TextData500.in", ios_base::in);
	ofs.open("P02_TextData500.out");

	// 检查文件是否成功打开
	if (!ifs.is_open() || !ofs.is_open()) {
		cout << "Failed" << endl;
		return 1;
	}

	// 从输入文件中读取剩余的数字字符串，进行去重处理，并写入输出文件
	while (ifs >> str) {
		int area_num = 0;
		int serial_num = 0;
		for (int i = 2; i < 6; i++)
			area_num = area_num * 10 + (str[i] - '0');
		for (int i = 6; i < 12; i++)
			serial_num = serial_num * 10 + (str[i] - '0');

		if (area[area_num] == 0) {
			root[area_num] = insert(root[area_num], serial_num);
			ofs << str << endl;
			area[area_num] = 1;
		}
		else {
			if (!search(root[area_num], serial_num)) {
				insert(root[area_num], serial_num);
				ofs << str << endl;
			}
		}
	}
	finish = clock();
	//clock()函数返回此时CPU时钟计时单元数
	cout << endl << "the time cost is:" << double(finish - start) / CLOCKS_PER_SEC << endl;
	//finish与start的差值即为程序运行花费的CPU时钟单元数量，再除每秒CPU有多少个时钟单元，即为程序耗时

}
