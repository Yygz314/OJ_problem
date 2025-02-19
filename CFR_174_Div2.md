# [B. Set of Strangers](https://codeforces.com/contest/2069/problem/B)
## Description
You are given a table of $n$ rows and $m$ columns. Initially, the cell at the $i$\-th row and the $j$\-th column has color $a_{i, j}$.

Let's say that two cells are strangers if they **don't** share a side. Strangers are allowed to touch with corners.

Let's say that the set of cells is a set of strangers if all pairs of cells in the set are strangers. Sets with no more than one cell are sets of strangers by definition.

In one step, you can choose any set of strangers **such that all cells in it have the same color** and paint all of them in some other color. You can choose the resulting color.

What is the minimum number of steps you need to make the whole table the same color?
## Input
The first line contains a single integer $t$ ($1 \le t \le 10^4$) — the number of test cases. Next, $t$ cases follow.

The first line of each test case contains two integers $n$ and $m$ ($1 \le n \le m \le 700$) — the number of rows and columns in the table.

The next $n$ lines contain the colors of cells in the corresponding row $a_{i, 1}, \dots, a_{i, m}$ ($1 \le a_{i, j} \le nm$).

It's guaranteed that the total sum of $nm$ doesn't exceed $5 \cdot 10^5$ over all test cases.
## Output
For each test case, print one integer — the minimum number of steps to paint all cells of the table the same color.
## Sample Input
```
4
1 1
1
3 3
1 2 1
2 3 2
1 3 1
1 6
5 4 5 4 4 5
3 4
1 4 2 2
1 4 3 5
6 6 3 5
```
## Sample Output
```
0
2
1
10
```
## Note
In the first test case, the table is painted in one color from the start.

In the second test case, you can, for example, choose all cells with color $1$ and paint them in $3$. Then choose all cells with color $2$ and also paint them in $3$.

In the third test case, you can choose all cells with color $5$ and paint them in color $4$.
## Hint
- 如何准确简单的表示出一次修改颜色的过程？
- 如何准确描述出现共用一侧的颜色？
- 如何准确简单的计算修改的最小步数？
## Thinking
- ~~这道题对于目前水平（几乎没有学任何算法和数据结构）的我来说还是太难了~~ 这是我第二次打cf的div2难度，~~深夜真的一点思路也没有。~~
- 当时的第一想法：按照贪心策略是需要在所有颜色中找到一个共用一边出现次数最多的颜色，并使其他颜色都变成改颜色。
但是对于每一次修改颜色，都需要重新讨论分析，~~太繁琐了~~，完全没有解题思路和解题欲望。
- 好 回归正题。我们从整体上重新分析这道题：这道题的每一次修改单元格中的颜色都是将所有满足题意的同一种颜色进行修改，
而对于有共用一侧的同一种颜色则需要每次单独修改颜色。\
于是我们可以假定两个集合s1,s2,分别记录颜色的种类和出现共用一侧的颜色的种类。而对于出现共用一侧的颜色，
我们从左往右从上往下遍历矩阵，当该单元格右边或者下面的单元格和它颜色相同时，我们认定它为共用一侧的颜色并存入集合s2中。 \
接下来就是计算最少需要的修改步数。~~不难发现~~ s1和s2集合的大小恰好可以形象的理解成改变颜色的次数（因为同一种颜色是一次性变换）无论如何，
即使S2的大小为0即不存在共用一侧的颜色，我们也需要用（s1大小-1）的步数将其它颜色转换为一种颜色。那么在s2大小非0的情况下，我们还需要
变换（s2大小-1）的步数（通过贪心知道变换成的颜色一定是在s2集合中的颜色）。综上，我们完成了本题。
## Personal Solution
```
#include<bits/stdc++.h>
using namespace std;
int num[705][705];
int main(){
	int t;
	cin>>t;
	while(t--){
		set<int> s1,s2;
		int n,m;
		cin>>n>>m;
		for(int i=0;i<n;i++){
			for(int j=0;j<m;j++){
				cin>>num[i][j];
				s1.insert(num[i][j]);	//统计不同种类的颜色 
			}
		}
		for(int i=0;i<n;i++){
			for(int j=0;j<m;j++){		//遍历检查是否是共用一边的颜色 
				if(j+1<m&&num[i][j]==num[i][j+1]) s2.insert(num[i][j]);
				if(i+1<n&&num[i][j]==num[i+1][j]) s2.insert(num[i][j]); 
			}
		}
		int x=s1.size();
		int y=s2.size();
		cout<<x-1+y-(y!=0)<<endl;
	}
	return 0;
} 
```


























