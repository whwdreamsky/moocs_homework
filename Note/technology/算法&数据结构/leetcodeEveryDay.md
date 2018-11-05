## 2018.1.29

### 03. Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a *subsequence*and not a substring.

 #### 实践

出现很大问题，结果正确但是时间上耗时太长



### 09.Palindrome Number

Determine whether an integer is a palindrome. Do this without extra space.

Some hints:

Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

 You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

这个有两个点要注意

1. Int 的最大范围 ： 
2. 除法耗时非常多，尽量使用*,+,-,另外在做int 相关的事情的时候可以枚举一些值，例如这道题，要求得到一个数x 的位数，我就枚举9，99….9999999999 来和x 比较大小，而不是通过除10，这样效率大大提升

### 11. [Container With Most Water](https://leetcode.com/problems/container-with-most-water) 

算法上使用贪心算法

又根据题目进行了优化



### 15. [3Sum](https://leetcode.com/problems/3sum) 

​	

###14.Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.



### 20.Valid Parentheses

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

### 