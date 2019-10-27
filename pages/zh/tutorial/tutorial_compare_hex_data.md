十六进制数的对比与分析在RFID相关的学习与研究上是很常见的，然而大部分都是面对电脑上，白底黑字的写字板，或者是Excel表格中进行对比，计算时还得不断切换计算器应用与原始数据。
这里给大家介绍一下MTools里的嗅探对比工具，以及分析数据的思路。

1.首先手动添加或直接采样添加每块16字节的数据
![](https://i.loli.net/2018/04/16/5ad4275564743.jpg)

2.添加完成后，可以看到两行数据间的浅蓝色与浅红色间隔，表示的是上下的数据相同或不同
[![5de9497071839170e00ca37be4b67c68.jpg](https://i.loli.net/2018/04/16/5ad42896bbca1.jpg)](https://i.loli.net/2018/04/16/5ad42896bbca1.jpg)

3.点击顶部第一个按钮，开始标记数据
通过转化可以知道0x2710表示十进制的10000，调整下参数
![](https://i.loli.net/2018/04/16/5ad4275560d87.jpg)

4.接着标记出第2步中浅红色的字节，即校验位
![](https://i.loli.net/2018/04/16/5ad427555d54f.jpg)
接着点击OK，稍后再分析算法

5.刚标记完的数据都显示横线，我们用b0,b1,b2...b15分别表示16个字节的数据
![](https://i.loli.net/2018/04/16/5ad4275556c79.jpg)

6.目测很容易看出b1,b5,b15的关系，如下  
```
b1=b2+b3;
b5=@~b1;   //@~b1表示取反
b15=b5+1;
```
再点击相应字节，添加表达式，数据完美适配
[![c1f1338d10edb6d4452bb7ae03a996fb.jpg](https://i.loli.net/2018/04/16/5ad428e668a5d.jpg)](https://i.loli.net/2018/04/16/5ad428e668a5d.jpg)

7.接下来分析b0与b1，有时候b0=b1+1，有时候则是减一，造成这样的结果只有可能是异或的运算，分析后得出如下公式  
```
    b0=b2@^b3@^1  //@^表示异或 xor
```

8.待所有删除线都消失，表示表达式完全正确
![](https://i.loli.net/2018/04/16/5ad4275543f85.jpg)
至此，规则添加完成。