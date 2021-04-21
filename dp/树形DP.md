# 树形DP

## Algorithm

树形dp主要以dfs的形式表现，在dfs中进行dp，主要实现形式是dp[i]\[j\][0/1]，i是以i为根的子树，j是在以i为根的子树中选择j个字节点，0表示节点不选，1表示选择该节点。有时j或0/1这一维可以压掉。

基本dp方程：

```c++
// 选择节点类
dp[i][0] += dp[j][1];
dp[i][1] += max/min(dp[j][0], dp[j][1]);

// 树形背包类
dp[v][k] = dp[u][k] + val;
dp[u][k] = max(dp[u][k], dp[u][k-1]);
```

## Problems

### [luogu P1352 没有上司的舞会](https://www.luogu.com.cn/problem/P1352)

dfs函数：

```c++
void dfs(int i){
    // 用dp[i][0/1]记录i节点不参加/参加的最大快乐指数
    dp[i][0] = 0;
    dp[i][1] = r[x];
    for (auto j:son[x]){
        dfs(j);
        dp[i][0] += max(dp[j][0], dp[j][1]); // j为i的儿子节点
        dp[i][1] += dp[j][0]; // i参加时j必不参加
    }
}
```

### [luogu P1122 最大子树和](https://www.luogu.com.cn/problem/P1122)

相对于上题，本题没有明确的父子关系，所以需要建立双向边。因为输入是一颗最小生成树，树的任何一个节点都能遍历完整棵树，所以可以从任意点dfs。并且在进行dp时不能返回父亲节点。

dfs函数：

```c++
// dp[i]储存以i为root的最大子树和
void dfs(int x, int p){ // x为当前节点，p为父亲节点
    dp[x] = v[x];
    for (auto y:edge[x]){
        if (y!=p){ // 防止返回已经处理过的父亲节点
            dfs(y, x);
            dp[x] += max(dp[y], 0); // 包含子树/不包含子树中最大的
        }
    }
    res = max(res, dp[x]); // 需要更新res
}
```

### [luogu P2014 [CTSC1997]选课](https://www.luogu.com.cn/problem/P2014)

树形背包问题，需要注意dp的顺序。

```c++
// dp[i][j]储存i为root，还可以选j个元素的最大学分
int dfs(int x){
    int sum=0;
    for (auto y:son[x]) {
        int t = dfs(y);
        sum+=t+1;
        for (int j=sum; j>=0; j--){ // 注意倒序
            for (int k=0; k<=t; k++){ // 顺序/倒序都可以
                if (j-k-1>=0) dp[x][j] = max(dp[x][j], dp[x][j-k-1]+dp[y][k]+v[y]); // 0-1背包
            }
        }
    }
    return sum;
}
```

## Reference

https://www.cnblogs.com/ifmyt/p/9588872.html