## 题目
这里这里 http://pan.baidu.com/s/1kTl5wxD

## write up

看到是Elf文件，表示不懂，所以直接上write_up了。

Write_up里说跟PE文件差不多，自己用IDA一看还真是差不多，所以按照同样的思路跑去找字符串，但是没找到。

回头看到write_up说是在函数开头部分重新定义了，所以关键是跑到那一串地方去把字符串给搞出来：

![1](https://github.com/L1nwatch/CTF/blob/master/IDF%20%E5%AE%9E%E9%AA%8C%E5%AE%A4/REVERSE%E5%80%92%E8%A1%8C%E9%80%86%E6%96%BD/%E7%AE%80%E5%8D%95%E7%9A%84ELF%E9%80%86%E5%90%91/1.png?raw=true)

把这些全都转成10进制，然后又由于这回代码取字符串的方式不太一样：

![2](https://github.com/L1nwatch/CTF/blob/master/IDF%20%E5%AE%9E%E9%AA%8C%E5%AE%A4/REVERSE%E5%80%92%E8%A1%8C%E9%80%86%E6%96%BD/%E7%AE%80%E5%8D%95%E7%9A%84ELF%E9%80%86%E5%90%91/2.png?raw=true)

算成10进制后还得除以2，所以用python脚本解决一下：

```python
List = ["EF","C7","E9","CD","F7","8B","D9","8D","BF","D9","DD","B1","BF","87","D7","D8","BF"]
for each in List:
	print(chr(int(each, 16) // 2), end="")
```

得到前半部分：`wctf{ElF_lnX_Ckl_`

后半部分还是跟上次的一样：

![3](https://github.com/L1nwatch/CTF/blob/master/IDF%20%E5%AE%9E%E9%AA%8C%E5%AE%A4/REVERSE%E5%80%92%E8%A1%8C%E9%80%86%E6%96%BD/%E7%AE%80%E5%8D%95%E7%9A%84ELF%E9%80%86%E5%90%91/3.png?raw=true)

提取出来是 `0823}`
组合一起提交了：`wctf{ElF_lnX_Ckl_0823}`
居然说答案不对，好吧，再观察一下：

```c++
if ( v23 != 22 )
	v24 = 1;
```

关键是多了这句话，而且对比一下正确答案就是v23出了问题。现在知道v23应该=22了，然后怎么运用得到ascii码？

好吧，找到原因了，原来是自己的List抄错了，不是这个22的问题= =

正确的：

```python
List = ["EF","C7","E9","CD","F7","8B","D9","8D","BF","D9","DD","B1","BF","87","D7","DB","BF"]
wctf{ElF_lnX_Ckm_0823}
```
