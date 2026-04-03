#### baby lfi（所属赛事HackINI 2022）

![](1.png)

靶场：http://49.232.142.230:10937/

提示：What about making things a bit harder ?（要不咱们把难度调高一点？）

思路：打开后发现是一道LFI（本地文件包含） 渗透测试题

让选择语言en/fr 提示：用 language 参数

猜测使用language参数

![](2.png)

输入language参数选择语言为en

![](3.png)

跳转出来多了：Hello there, this is a basic example, just a proof of concept（你好呀，这只是一个基础示例，纯粹是为了演示原理而已）

这只是一个例子，回到题目 why not /etc/passwd

猜测使用 etc/passwd 做参数值，构造payload

![](4.png)



使用 etc/passwd 做参数值，构造payload

![](5.png)

![](6.png)

在最下方找到flag

shellmates{10CA1_F11e_1Nc1US10n_m4y_r3ve4l_in7Er3st1nG_iNf0Rm4t1on}



baby lfi 2（所属赛事HackINI 2022）

靶场：http://49.232.142.230:16518/

提示： What about making things a bit harder ?（要不咱们把难度调高一点？）

思路：![](1.1.png)

和第一题感觉相差不大

提示：已批准路径,有languages 目录,我不会告诉你里面有什么库存的

和第一题一样使用language参数选择en

![](1.2.png)

依旧只是一个例子

![](1.3.png)

使用 etc/passwd 做参数值，构造payload

![](1.4.png)

发现并没有和上一题一样直接给出flag

并给出：指定的路径不合法！  说明后端对 language 参数做了路径限制

提示里说 there is a languages directory，说明程序默认只会在 languages/ 目录下寻找文件，直接访问 /etc/passwd 被判定为越界

然限制在 languages/ 目录，可以用 ../ 向上跳转到根目录，再定位目标文件

![](1.5.png)

![](1.6.png)

发现并没有用，依旧被拦截

![](1.7.png)

构造绕过 payload

使用./languages/主动进入后端允许的合法目录，规避路径拦截

![](1.8.png)

最终找到flag  shellmates{yOU_M4De_yOUr_waY_7hRough_iT}

![](1.9.png)

原理：利用路径前缀欺骗：用后端允许的languages/作为前缀，让后端认为访问的是合法目录内的文件；

利用目录遍历特性：../是操作系统的通用目录跳转符，可突破目录限制，从合法目录跳转到任意目录；../../../../：每层../表示向上跳转一级目录，多次拼接确保跳出到系统根目录/
