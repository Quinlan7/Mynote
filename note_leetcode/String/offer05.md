##### 题目描述：

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

##### 解法：

 偷个懒，直接调函数了。

```java
public String replaceSpace(String s) {
        s = s.replace(" ","%20");
        return s;
    }
```

##### 正经的解法

可以new一个String来存储替换后的字符串。

```java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size);
        return newStr;
    }
}

```

