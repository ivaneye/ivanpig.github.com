---
layout: post
title: 从1到n:是非题
categories: 从1到n
tags: [从1到n]
avatarimg: "/img/head.jpg"
author: Ivan

---

# 是非题

现实生活中我们经常会遇到很多是非题！比如:

- 1 + 1 = 2 吗？
- 你满18岁了吗？
- 你是中国人吗？
......

这些问题我们都可以用简单的是或者否来回答!

# 十进制
## 为什么十进制"最流行"?

从网上搜集到的资料，个人感觉最靠谱的原因是人类有十根手指。

回忆自己学数字，或者电视里，或者教自己的小孩数字，应该都是从数手指头开始的。然后就出现了如下奇葩图:

![]({{site.CDN_PATH}}/assets/my/1n/binary/01.jpg)

这样数数很方便，伸手就能数，十个手指头不够还能数脚趾头，不够再借别人的接着数。

## 为0正名

数指头的方式很方便，但是也带来了一个问题:大家可以回忆一下，自己在学数数的时候是不是从1开始教的?1-9,然后10-19,后来才学的0?然后学到了负数，数字就被划分成了正数，负数和0。

从一开始，0一直被特殊对待着。所以对于十进制来说,脑海里的印象都是从1开始。别人让你数10个数，你的第一反映应该是

```
1   2   3   4   5   6   7   8   9   10
```

再多数几个数，就是

```
1   2   3   4   5   6   7   8   9   10
11  12  13  14  15  16  17  18  19  20
21  22  23  24  25  26  27  28  29  30
```

能看出来问题吗？10其实是两位数，但是和一位数归类到一起了。这导致了数数和进制产生了差别!

请看[百度经验](http://jingyan.baidu.com/article/5bbb5a1b475d2e13eba1791e.html)
对十进制的描述时，第一句话对十进制的定义就是有问题的:"从 1～～10 然后进位，再从 11～～20 再进位，这种逢十进位的方法，就叫做十进制。"

"从 1～～10 然后进位"?10已经是进位后的数了，怎么再进位！！！

可以把这个看成是生活经验与学术上的差异吧！

我们把上面的数字按照进制来重新排列一下：

```
    1   2   3   4   5   6   7   8   9
10  11  12  13  14  15  16  17  18  19
20  21  22  23  24  25  26  27  28  29
......
```

如上可见，一位数和其它位数的数比较，会少了一位！其它位数的数字都是从n0开始，而一位数是从1开始的。补上0后，才是完整的十进制。

```
0   1   2   3   4   5   6   7   8   9
10  11  12  13  14  15  16  17  18  19
20  21  22  23  24  25  26  27  28  29
......
```

所以十进制的定义应该是：**由0-9这十个数字组成，逢10进1。**

个人感觉十进制被理解错的另一个原因是:一位数到两位数，逢10进1，就是10，没有感官上的进制的动作。

# 二进制

从上面十进制的定义我们可以给出二进制的定义:**由0-1这两个数字组成，逢2进1**

| 十进制        | 二进制           | 
| ------------- |:-------------:|
| 0      | 0 | 
| 1      | 1      |
| 2 | 10      |
| 3 | 11      |
| 4 | 100      |
| 5 | 101      |
| 6 | 110      |
| 7 | 111      |
| 8 | 1000      |
| 9 | 1001      |

十进制的8在二进制中已经需要4位来表示了，这么麻烦的进制表示有什么用呢?

其实你时时刻刻都在和二进制打交道:

- 你开灯了是1，关灯是0
- 有些店门口会有个牌子，开门就是open(就是1),关门就是close(就是0)

实际上，电子产品使用的都是以二进制为基数的进制表示法

# 计算机中的二进制
# 二进制的计算
# 参考资料
## 为什么十进制"最流行"?
- [百度经验](http://jingyan.baidu.com/article/5bbb5a1b475d2e13eba1791e.html)
- [果壳问答](http://www.guokr.com/question/194124/)
- [知乎](http://www.zhihu.com/question/19588617)

## 其它书籍

- 《Code》
