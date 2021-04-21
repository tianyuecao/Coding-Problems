# Manacher算法

## Algorithm

Manacher算法可以在时间复杂度O(n)的情况下求解一个字符串的最长回文子串长度的问题。

回文子串的一般解法是每个字符作为回文子串的中心对称点，每次保存前面求得的回文子串的最大值，最后得到的就是最长的回文子串的长度，时间复杂度为O(n^2)。由于奇数和偶数长度的回文子串的对称中心不同，如"aba"对称中心为b，"abba"对称中心在两个b之间，为了解决这个问题，可以在每个字符串两边加一个符号，具体符号对结果没有影响，如加上"#"，上述序列

![image](https://github.com/tianyuecao/Coding-Problems/blob/main/images/kmp2.gif)

KMP算法的核心是前缀表（代码中的next数组），用于记录待匹配模式串中0至i-1个字符的相同前缀和后缀的最大长度。

![image](https://github.com/tianyuecao/Coding-Problems/blob/main/images/kmp1.png)

如对于"abcab"子串相同前缀和后缀的最大长度为2，通过记录的next可以在出现不匹配的时候不跳转回待匹配字符串头部，而是跳转到下一个需要匹配的位置。

在实现时，通常把前缀表统一减一，即右移一位，初始位置为-1。

时间复杂度为O(m+n)。

## Code


```cpp
# 获得next数组
vector<int> getnext(string& s){
    int n = s.size();
    vector<int> next(n, -1);
    int j = -1;
    for (int i=1; i<n; i++){
        while (j>=0 && s[j+1]!=s[i])j = next[j];
        if (s[j+1]==s[i]) j++;
        next[i] = j;
    }
    return next;
}

# 在字符串a中匹配字符串b，返回a中匹配到b的位置
int kmp(string a, string b){
    if (b.size()==0) return 0;
    vector<int> next = getnext(b);
    int j = -1;
    for (int i=0; i<a.size(); i++){
        while (j>=0 && a[i]!=b[j+1]) j = next[j];
        if (a[i]==b[j+1]) j++;
        if (j==b.size()-1) return i-j;
    }
    return -1;
}
```

## Problems

[LeetCode 28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

[LeetCode 214. 最短回文串](https://leetcode-cn.com/problems/shortest-palindrome/)

[LeetCode 459. 重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)

## Reference

https://leetcode-cn.com/problems/implement-strstr/solution/dai-ma-sui-xiang-lu-kmpsuan-fa-xiang-jie-mfbs/
