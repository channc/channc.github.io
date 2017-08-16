---
layout: post
title:  "[Nowcoder]华为机试题-拓扑排序-循环依赖"
date:   2017-08-16 21:01:45 +0800
categories: share
---


这题比较可恶的地方是输入和输出的处理，输入输出十六进制的数字，百度上没看到什么资料，而且题目描述也不清晰。

大意是输入一堆依赖关系，然后求出特定的节点是否循环依赖，直接拓扑排序

看来拓扑以后要准备个模板常备着

```
using namespace std;
typedef long long LL;
inline void read(LL&a){char c;while(!(((c=getchar())>='0')&&(c<='9')));a=c-'0';while(((c=getchar())>='0')&&(c<='9'))(a*=10)+=c-'0';}
class Dependency {
public:
	void AddDependency(unsigned int ModuleId, unsigned int DependModuleId) {
		if (degree.find(ModuleId) == degree.end()) { degree[ModuleId] = 0; }
		if (degree.find(DependModuleId) == degree.end()) { degree[DependModuleId] = 0; }
		++degree[DependModuleId];
		Graph[ModuleId].push_back(DependModuleId);
	}
	void CalcDependency() {
		for (map<int, int>::iterator it = degree.begin(); it != degree.end(); ++it) {
			if (it->second == 0) {
				que.push(it->first);
				degree[it->first] = -1;
			}
		}
		while (!que.empty()) {
			int x = que.front(); que.pop();
			for (int i = 0; i < Graph[x].size(); ++i) {
				--degree[Graph[x][i]];
				if (degree[Graph[x][i]] == 0) {
					que.push(Graph[x][i]);
					degree[Graph[x][i]] = -1;
				}
			}
		}
	}
	bool MouldelsCycularDependency(unsigned int ModuleId) {
		if (degree[ModuleId] > 0) return false;
		return true;
	}
	void clear(void) {
		degree.clear();
		Graph.clear();
	}
	queue<int> que;
	map<int, int> degree;
	unordered_map<int, vector<int>> Graph;
};
int main() {
	Dependency dep;
	int a, b;
	while (scanf("{%x, %x}", &a, &b) != EOF) {
		dep.AddDependency(a, b);
		char ch = getchar(); 
		if (ch != ',') break;
		getchar();
	}
	dep.CalcDependency();
	vector<pair<int, int>> ans;
	for (map<int, int>::iterator iter = dep.degree.begin(); iter != dep.degree.end(); ++iter) {
		//if (iter->second > 0) printf("(0x%.02x, true)\n", iter->first);
		//else printf("(0x%.02x, false)\n", iter->first);
		ans.push_back(make_pair(iter->first, iter->second));
	}
	for (int i = 0; i < ans.size(); ++i) {
		if (ans[i].second != -1) printf("{0x%.02x, true}", ans[i].first);
		else printf("{0x%.02x, false}", ans[i].first);
		if (i != ans.size()-1) printf(",\n");
		else printf("\n");
	}
	return 0;
}
```
