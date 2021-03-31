resource  /res目录 ： 大多数文件//如xml 会被编译成二进制文件（体积更小，解析速度更快），同时会被映射到R文件，访问的时候直接使用资源ID即R.id.filename具有有包括屏幕、系统、语言支持功能，；另外，依赖库中的resource也会被引入而assets不会




assets      /assets 目录：会被原封不动打包进apk中，没有R文件映射，访问的时候需要AssetManager类。


----------


//R8 是在 Android Gradle 插件 3.3.0 中引入的，对于使用插件 3.4.0 及更高版本的应用和 Android 库项目，R8 现已默认处于启用状态。

![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/0001.png)


Gladle插件版本  build.gradle文件  classpath'com.android.tools.build:gradle:3.0.0' 指定

Gladle版本：    gradle/wrapper/gradle-wrapper.properties文件中 distributionUrl=https\://services.gradle.org/distributions/gradle-x.x.x.zip 指定


![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/0002.png)



----------


![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/0003.webp)





----------



本地库模块依赖项  //module

implementation project(':mylibrary')



这声明了对一个名为“mylibrary”（此名称必须与在您的 settings.gradle 文件中使用 include: 定义的库名称相符）的 Android 库模块的依赖关系。在构建您的应用时，构建系统会编译该库模块，并将生成的编译内容打包到 APK 中。

本地二进制文件依赖项  //jar  java ARchive  .class文件+MENIFEST.MF文件

implementation fileTree(dir: 'libs', include: ['*.jar'])



Gradle 声明了对项目的 module_name/libs/ 目录中 JAR 文件的依赖关系（因为 Gradle 会读取 build.gradle 文件的相对路径）。

或者，您也可以按如下方式指定各个文件：

implementation files('libs/foo.jar', 'libs/bar.jar')



远程二进制文件依赖项  //aar  jar + AndroidManifest、res、R、public.txt和可能其他

implementation 'com.example.android:app-magic:12.3'



这实际上是以下代码的简写形式：

implementation group: 'com.example.android', name: 'app-magic', version: '12.3'




这声明了对“com.example.android”命名空间组内的 12.3 版“app-magic”库的依赖关系。

![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/0004.png)



----------


    allprojects {
    repositories {
        google() //Google Maven repository
        jcenter() //Bintray’s JCenter Maven repository
        mavenCentral() //central Maven repository
        maven { url "https://maven.aliyun.com/repository/google" }  //Google阿里镜像
        maven { url "https://maven.aliyun.com/repository/jcenter" }  //JCenter阿里镜像

        mavenLocal() //
    }
}


Google 的 Maven 代码库中提供了以下 Android 库的最新版本：

Android 支持库
架构组件库
约束布局库
AndroidX 测试
数据绑定库
Android 免安装应用库
Wear OS
Google Play 服务
Google Play 结算库
Firebase


![](https://github.com/WhileCrow/AndroidKnowledge/blob/main/res/0005.png)
