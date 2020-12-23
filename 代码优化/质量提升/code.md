## # 代码优化 # ##
## 1：纯函数 ##
纯函数是函数编程中的一个概念，指的是一个方法函数，就像一个数学的函数式一样，同样的参数输入会得到同样的输出。
可以近似理解为函数内部不依赖任何的外部状态，外部变量。
## 2：空安全 ##
java是强对象类型语言，故对于java来说我们很容易关注到类型强转的错误

![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/image1.png)

这里引进一个概念，程序其实是一个由函数组成的树型或者图型结构，这其实意味着我们需要在接近根节点的地方，对传入的  不应为空的参数或变量进行校验，从而避免参数往后传递而造成在后面叶子节点进行空判断的现象。
这一行为通常可以通过对函数入参加入注解：@Nullable/@NonNull

![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/image3.png)

在java中，要尽量保证代码质量，排除空安全
而所谓的空安全的语言，实际上也没有解决空安全的问题，因为空这个概念是被需要的，所以如kotlin之类的空安全语言只是把空校验从运行时提前到编译时

![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/image2.png)

## 3：控制逻辑 ##
对于 嵌套循环或if 是否要通过continue/break/return等操作，简化嵌套的问题。其实是存在分歧的，分歧点在于：
**continue/break/return操作虽然可以简化嵌套但同时也会增加代码复杂度，直观度。**

所以，嵌套是否要continue/break/retrun，还是需要根据具体的业务情况才能决定，大致上有这么几种考虑：
1. 对于异常情况引起的if嵌套，可以通过在提前return的方式，减低嵌套
2. 如果业务确实需要，不需为你的 嵌套的控制逻辑 感到不好意思。如果嵌套确实能达到直观的业务表达效果，那就用它
3. continue/break/return操作，尽量不要在控制逻辑的中或者尾逻辑出现，这会很大程度的使逻辑可读性降低。如果要用，尽量提前使用它们。
