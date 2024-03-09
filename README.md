# NJMU-Network-Connect

## TODO

2024.3.7（v1.0开发过程中，2024.3.9修改）

- [x] 通过网页获取加密密码，写在spyder.py

- [x] 编写新建json文件的代码，同时写入基础参数，并编写获取参数的代码

- [x] 处理加密密码并放置在json中便于后续调用，(通过update)
- [ ] 编写下载浏览器驱动or选择浏览器驱动并解压到对应目录
- [ ] 自动判断浏览器驱动，并自动设置
- [ ] 编写发送请求模块
- [ ] 自动连接，设置判断参数
- [ ] ……
- [ ] 开发核心程序，v1.0结束

## 研究过程

*（如果只想要了解程序背后的原理，请移至下一个板块。）*

在深入研究南京医科大学的网络之前，就已经知道了很多信息，例如，学校网络分为三个网络系统（有线网、无线网、不连互联网的内部网）、使用互联网需要用户登录、在凌晨0点会断电等。

我们在深入研究南京医科大学的网络之前，就已经知道了很多信息，例如，学校网络分为三个网络系统（有线网、无线网、不连互联网的内部网）、使用互联网需要用户登录、在凌晨0点会断电等。

可是在我们使用过程，发现在不使用网络一段时间会自动断开连接，或是每次开机，会弹出浏览器要求登录（虽然浏览器会保存密码，但是会打开浏览器需要点击），所以有没有一种优雅的方式来持续使用校园网？或者这样说，是否可以弄一个客户端来登录。于是，我开始研究有线网和无线网这两个连互联网的网络登录页面。

### 无线网

我先研究了无线网络，我首先通过抓包程序，获取通讯行为，并通过浏览器的开发者工具的网络模块中发现了，浏览器是通过发送post来实现用户登录。通过分析，不难发现，发送的cookies中简单的展示了账号和密码，我认为该通讯方式是极度不安全的，不过这样的好处是，程序模拟登录的方式十分简单，易于实现。我也验证了可行性。

### 有线网

有线网的研究过程还是比较困难的，我照着葫芦画瓢，也尝试使用研究无线网的方式来研究有线网，结果发现，在按钮按下时，会发送一个数据包，但是里面的信息还是比较多的，出现了url编码，于是通过解码，发现是运营商的中文等等。但是我目标明确，发现了userid和pwd这两个变量，可是难点出现在了pwd。一开始认为pwd应该与无线网登录类似，使用明文传输。看到值之后，我就觉得这并不简单，是一串“乱码”，我知道，只是通过某种加密方式实现的。~~于是，我打开网页源代码，进行研究。这个研究的过程比较曲折，主要是对于js语言并不熟悉，只能通过仅有的一定知识来研究。终于，发现了编码密码的函数。我尝试在控制台中，复现一下，结果成功了。可是下面又遇到了难题，代码使用了公私钥加密（AES加密算法），使用控制台是可以确认公钥的，但是写程序是需要公钥的，在不知道校园网公钥是否唯一的情况下，我是需要通过网络来获取的。于是在各种文件中寻找线索，终于在元素中发现了玄机，公钥是写在一个entry元素中的，只不过被隐藏了。~~（2024.3.7，2024.3.8删除）

在写代码的过程中发现我无法理解加密方法，同时我想到，也没有必要通过从公钥来获得加密后的密码，毕竟每台电脑的mac一般不变，就算认为每台电脑给的公钥不同，存在电脑里的加密密码只会给该电脑来使用，所以从网页上爬取加密后的密码即可，之后只需要向入口发包就可以实现上网了。（2024.3.8）

此时，一切都研究明白了，开始实现功能。

## 原理实现

采用熟悉的python语言来实现，网络通讯是需要用到一个内置库requests库；获取网页信息需要使用爬虫，所以需要使用selenium库中的webdriver函数~~或使用bs4库中的BeautifulSoup函数~~；为了适应多样性和在出现漏洞时便于分析，这里我们需要使用sys库中的argv函数（各个操作系统可能不同）来传入外部参数（可能会打包成程序，不知道该函数是否有效）。

上面是实现核心功能所需的库，为了让小白也可以简单使用，我决定使用tkinter库（这个比较熟悉些，可能会使用这个）或wxpython库来实现，可能会在v2.0版本发布。

目前，前期准备都已经完成，下面就开始程序开发。

## 程序开发

*v1.0 编写核心程序

## 参考链接

[webdriver]https://blog.csdn.net/hao_13/article/details/132701205 python 学习笔记（4）—— webdriver 自动化操作浏览器（基础操作）
[Beautiful Soup]https://blog.csdn.net/qq_44667896/article/details/104793547 Python在HTML内提取元素
