#C++动态库加载顺序

##问题描述
编译调试系统过程中遇到这样一个问题，某个程序P运行过程中需要加载同一个动态库的两个不同版本，简称D.0与D.1。两个版本的动态库存在一些不兼容的函数，但是函数名相同，如F.0与F.1，而P的几个功能明确需要调用F.1。如何编写Makefile.am。
其实问题很简单，只需要Makefile.am中动态库先写D.1再写D.0即可。比如我的问题中：
```sh
LDADD =	/usr/local/lib/libcrypto.so.1.1 \
	/usr/local/lib/libssl.so.1.1 \
	/usr/lib/x86_64-linux-gnu/libcrypto.so	\
	/usr/lib/x86_64-linux-gnu/libssl.so	\
```


