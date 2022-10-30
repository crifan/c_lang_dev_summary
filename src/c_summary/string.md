# 字符串

TODO：

* 【整理】C语言中字符串操作相关函数
* 【整理】C语言中字符串拷贝strdup
* 【已解决】C语言中如何判断字符串是以特定字符串开始startWith
* 【已解决】C语言中字符串指针加index索引设置字符值但无效
* 【已解决】C语言中申请空间后再去字符串拷贝
* 【已解决】C代码报错：Invalid application of sizeof to an incomplete type const char *[]
* 【已解决】XCode中C代码strlen报错：Implicitly declaring library function strlen with type unsigned long const char
* 【已解决】C语言中初始化字符串空数组后续添加字符串或者字符串依次相加
* 【已解决】C语言中如何合并2个静态const的字符串列表
* 【记录】别人自定义strstr字符串检测失败导致越狱检测误判
* 
* 计算字符串数组个数
  * 【已解决】C语言中如何判断字符串数组列表的个数
* 判断相等
  * 【已解决】C语言中判断2个字符串是否相等
* 格式化
  * 【已解决】C中如何非打印输出的格式化字符串
* 去除最开始和最后一个字符
  * 【已解决】C语言中如何去除字符串的最开始和最后一个字符

## 判断字符串是否以特定字符结尾

TODO：

* 【已解决】C语言中判断字符串是否以特定字符结尾

```c
// "/Library/dpkg/", "/" -> true
bool strEndsWith(const char* fullStr, const char* endStr)
{
    if (!fullStr || !endStr){
        return false;
    }

    size_t fullStrLen = strlen(fullStr);
    size_t endStrLen = strlen(endStr);

    if (endStrLen >  fullStrLen){
        return false;
    }

//    return strncmp(fullStr + fullStrLen - endStrLen, endStr, endStrLen) == 0;
    const char* partStr = fullStr + (fullStrLen - endStrLen);
    bool isEndEqual = (0 == strcmp(partStr, endStr));
    return isEndEqual;
}
```

## NULL terminated string

字符串都是`0`结尾的，叫做：`NULL terminated string`

其实：

* `\0` == `NULL`
  * `\0`的字符，对应写成：`'\0'`

举例：

（1）关于普通的一个字符串：

```c
char* name = “Crifan”;
```

其内部存储的是：

```c
C r i f a n 0
```

即

```c
C r i f a n \0
```

---

TODO：

* 把上面的字符串存储，用表格画出来
  * 可以考虑用之前别人介绍的：
    * 【记录】在线绘图画表格图等：GraphvizOnline

---

（2）判断字符串的某个字符，是否是NULL，一般可以写成：

```c
curChar == '\0'
```

或：

```c
curChar == NULL
```

比如：

```c

// "CYDIA://xxx" -> "cydia://xxx"
char* strToLowercase(const char* origStr){
    char* lowerStr = strdup(origStr);
    char curChar = lowerStr[0];
    for(int i = 0; curChar != '\0'; i++){
        curChar = lowerStr[i];
        char curCharLower = tolower(curChar);
        lowerStr[i] = curCharLower;
    }
    return lowerStr;
}
```

就是：

```c
curChar != '\0'
```

去比较：当前字符是否是\0

== 判断当前字符串是否结束

（3）对应的，把字符串中某个位置直接设置为NULL = ‘\0’，就可以起到：

* 截断字符串
* 结束字符串

等效果

比如之前的函数：

```c
// file mode to string
void fileModeToStr(mode_t mode, char * modeStrBuf) {
    // buf must have at least 10 bytes
    const char chars[] = "rwxrwxrwx";
    for (size_t i = 0; i < 9; i++) {
//        buf[i] = (mode & (1 << (8-i))) ? chars[i] : '-';
        bool hasSetCurBit = mode & (1 << (8-i));
        modeStrBuf[i] = hasSetCurBit ? chars[i] : '-';
    }
    modeStrBuf[9] = '\0';
}
```

最后就是：

```c
    modeStrBuf[9] = '\0';
```

去结束字符串的

以及：

```c
// "/./Library/../Library/dpkg/.", "/." -> "/./Library/../Library/dpkg"
char* removeTail(const char* fullStr, const char* tailStr){
    char *newStr = strdup(fullStr);
    
    size_t fullLen = strlen(fullStr);
    size_t tailLen = strlen(tailStr);

    if (tailLen > fullLen){
        return newStr;
    }

    if (strEndsWith(fullStr, tailStr)){
        size_t endIdx = fullLen - tailLen;
        newStr[endIdx] = '\0';
    }

    return newStr;
}
```

中的：

```c
newStr[endIdx] = '\0';
```

去把最后部分的某个位置的字符，设置为\0，从而起到`截断字符串`的效果。
