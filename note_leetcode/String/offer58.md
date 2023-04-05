##### 题目描述：

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof



##### 解法：

两种解法，一种调包的，一种自己写。

都很简单



```java
public String reverseLeftWords(String s, int n) {
        String s1 = s.substring(0,n);
        String s2 = s.substring(n,s.length());
        return s2 + s1;

    }
```







```java
public String reverseLeftWords2(String s, int n) {
        char[] chars = s.toCharArray();
        char[] ret = new char[s.length()];
        for (int i = 0,start = 0,end = n ; i < chars.length; i++,end--) {
            if(i < n) ret[s.length() - end] = chars[i];
            else ret[start++] = chars[i];
        }

        return new String(ret);
    }
```

