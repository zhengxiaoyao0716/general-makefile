# General makefile
## 通用的C|C++编译规则

***
### 最近又需要用到Linux C 开发了

之前的Makefile 比较简单，只能在Windows上用，这次花了点时间写个更通用的。

***
### 本身逻辑很简单，但功能很强大

项目结构：
``` sh
${Project}/
    lib/  #工程依赖
    out/  #构建目录
        ${Project}.exe  # 构建完成的可执行文件，Linux下无后缀
    src/  #工程源码
        main/  #源文件目录，.cpp
        head/  #头文件目录，.h
    Makefile
    ...
```

简单说来，源代码放在`src`目录下，其中`src/main`是主程，`.cpp`格式，`src/head`是头文件，`.h`格式。依赖放在`lib`下面。执行make后（Windows上用mingw的make），在`out`目录下可以看到构建成功的可执行文件。

***
### 比起网上那些恶心难用的Makefile

我的Makefile的特点就是简单易用模块清晰。
1. 同时支持Linux和Windows(MinGW)，自动识别
2. 自动搜索源代码、头文件，各常用路径可配置
3. 自动决定输出文件名，当然你也可以手动指定
4. 生成文件都放在一个目录下，不污染原项目结构
5. ~~正的作品，良心出品~~ 有什么Bug尽管提

***
### 整体思路，对底层原理感兴趣的就看一看：

寻找`src`目录下后缀为`.cpp`的所有源代码，（伪遍历，只搜索6层），编译出`.o`，放到`out`目录下相应目录结构的位置去。接着，为每一个`.o`构建一个`.d`，临时Makefile，放在相应位置。最后组装成一个可执行文件。

***
最后，一个示例用法：[VSCodeCppDemo](https://github.com/zhengxiaoyao0716/VscodeCppDemo)