给你一个字符串 s，找到 s 中最长的回文子串。

示例 1：

输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
示例 2：

输入：s = "cbbd"
输出："bb"

提示：

1 <= s.length <= 1000
s 仅由数字和英文字母组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-palindromic-substring
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function (s) {
  const strLen = s.length;
  let numOfPalindromicStr = 0;
  let dp = Array.from(Array(strLen), () => Array(strLen).fill(false));
  let maxLen = 0;
  let str = "";
  for (let j = 0; j < strLen; j++) {
    for (let i = 0; i <= j; i++) {
      if (s[i] === s[j]) {
        if (j - i <= 2) {
          dp[i][j] = true;
        } else {
          dp[i][j] = dp[i + 1][j - 1];
        }
        // numOfPalindromicStr += dp[i][j] ? 1 : 0;
        let tmpLen = j - i + 1;
        if (dp[i][j] && tmpLen > maxLen) {
          maxLen = tmpLen;
          str = s.slice(i, j + 1);
        }
      }
    }
  }

  return str;
};
```

How can we reuse a previously computed palindrome to compute a larger palindrome?
If “aba” is a palindrome, is “xabax” a palindrome? Similarly is “xabay” a palindrome?
Complexity based hint:
If we use brute-force and check whether for every start and end position a substring is a palindrome we have O(n^2) start - end pairs and O(n) palindromic checks. Can we reduce the time for palindromic checks to O(1) by reusing some previous computation.

方法一：动态规划
思路与算法

对于一个子串而言，如果它是回文串，并且长度大于 22，那么将它首尾的两个字母去除之后，它仍然是个回文串。例如对于字符串 \textrm{`ababa''}“ababa”，如果我们已经知道 \textrm{`bab''}“bab” 是回文串，那么 \textrm{`ababa''}“ababa” 一定是回文串，这是因为它的首尾两个字母都是 \textrm{`a''}“a”。

根据这样的思路，我们就可以用动态规划的方法解决本题。我们用 P(i,j)P(i,j) 表示字符串 ss 的第 ii 到 jj 个字母组成的串（下文表示成 s[i:j]s[i:j]）是否为回文串：

P(i,j) = \begin{cases} \text{true,} &\quad\text{如果子串~} S_i \dots S_j \text{~是回文串}\\ \text{false,} &\quad\text{其它情况} \end{cases}
P(i,j)={
true,
false,
​

如果子串  S
i
​
…S
j
​
  是回文串
其它情况
​

这里的「其它情况」包含两种可能性：

s[i, j]s[i,j] 本身不是一个回文串；

i > ji>j，此时 s[i, j]s[i,j] 本身不合法。

那么我们就可以写出动态规划的状态转移方程：

P(i, j) = P(i+1, j-1) \wedge (S_i == S_j)
P(i,j)=P(i+1,j−1)∧(S
i
​
==S
j
​
)

也就是说，只有 s[i+1:j-1]s[i+1:j−1] 是回文串，并且 ss 的第 ii 和 jj 个字母相同时，s[i:j]s[i:j] 才会是回文串。

上文的所有讨论是建立在子串长度大于 22 的前提之上的，我们还需要考虑动态规划中的边界条件，即子串的长度为 11 或 22。对于长度为 11 的子串，它显然是个回文串；对于长度为 22 的子串，只要它的两个字母相同，它就是一个回文串。因此我们就可以写出动态规划的边界条件：

\begin{cases} P(i, i) = \text{true} \\ P(i, i+1) = ( S*i == S*{i+1} ) \end{cases}
{
P(i,i)=true
P(i,i+1)=(S
i
​
==S
i+1
​
)
​

根据这个思路，我们就可以完成动态规划了，最终的答案即为所有 P(i, j) = \text{true}P(i,j)=true 中 j-i+1j−i+1（即子串长度）的最大值。注意：在状态转移方程中，我们是从长度较短的字符串向长度较长的字符串进行转移的，因此一定要注意动态规划的循环顺序。

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
