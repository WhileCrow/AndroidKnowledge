## # 代码优化 # ##
## 1：纯函数 ##
## 2：空安全 ##
java是强对象类型语言，故对于java来说我们很容易关注到类型强转的错误
![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/image1.png)

这里引进一个概念，程序其实是一个由函数组成的树型或者图型结构，这其实意味着我们需要在接近根节点的地方，对传入的  不应为空的参数或变量进行校验，从而避免参数往后传递而造成在后面叶子节点进行空判断的现象。
这一行为通常可以通过对函数入参加入注解：Nullable/NonNull
![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/image3.png)
在java中，要尽量保证代码质量，排除空安全

而所谓的空安全的语言，实际上也没有解决空安全的问题，因为空这个概念是被需要的，所以如kotlin之类的空安全语言只是把空校验从运行时提前到编译时
![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/image2.png)

