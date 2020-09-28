## 一、谈谈你对 JNI 和 NDK 的理解

#### JNI：

JNI 是 `Java Native Interface` 的缩写，即 Java 的本地接口。

目的是使得 Java 与本地其他语言（如 C/C++）进行交互。

JNI 是属于 Java 的，与 Android 无直接关系。

#### NDK：

NDK 是 `Native Development Kit` 的缩写，是 Android 的工具开发包。

作用是更方便和快速开发 C/C++ 的动态库，并自动将动态库与应用一起打包到 apk。

NDK 是属于 Android 的，与 Java 无直接关系。

#### 总结：

JNI 是实现的目的，NDK 是 Android 中实现 JNI 的手段。

## 二、谈谈你对 JNIEnv 和 JavaVM 理解

#### JavaVM

JavaVM 是虚拟机在 JNI 层的代表。

一个进程只有一个 JavaVM。（重要！）

所有的线程共用一个 JavaVM。（重要！）

#### JNIEnv

JNIEnv 表示 Java 调用 native 语言的环境，封装了几乎全部 JNI 方法的指针。

JNIEnv 只在创建它的线程生效，不能跨线程传递，不同线程的 JNIEnv 彼此独立。（重要！）

#### 注意：

在 native 环境下创建的线程，要想和 java 通信，即需要获取一个 JNIEnv 对象。我们通过 `AttachCurrentThread` 和 `DetachCurrentThread` 方法将 native 的线程与 JavaVM 关联和解除关联。

## 三、解释一下 JNI 中全局引用和局部引用的区别和使用

#### 全局引用

通过 `NewGlobalRef` 和 `DeleteGlobalRef` 方法创建和释放一个全局引用。

全局引用能在多个线程中被使用，且不会被 GC 回收，只能手动释放。

#### 局部引用

通过 `NewLocalRef` 和 `DeleteLocalRef` 方法创建和释放一个局部引用。

局部引用只在创建它的 native 方法中有效，包括其调用的其它函数中有效。因此我们不能寄望于将一个局部引用直接保存在全局变量中下次使用（请使用全局引用实现该需求）。

我们可以不用删除局部引用，它们会在 native 方法返回时全部自动释放，但是建议对于不再使用的局部引用手动释放，避免内存过度使用。

#### 扩展：弱全局引用

通过 `NewWeakGlobalRef` 和 `DeleteWeakGlobalRef` 创建和释放一个弱全局引用。

弱全局引用类似于全局引用，唯一的区别是它不会阻止被 GC 回收。

## 四、JNI 线程间数据怎么互相访问

考察点和上体类似，线程本来就是共享内存区域的，因此我们需要使用 `全局引用`。

## 五、怎么定位 NDK 中的问题和错误

一般在开发阶段的话，我们可以通过 log 来定位和分析问题。

如果是上线状态（即关闭了基本的 log），我们可以借助 NDK 提供的 `addr2line` 工具和 `objdump` 工具来定位错误。详情：

[so 动态库崩溃问题定位（addr2line与objdump）](https://blog.csdn.net/afei__/article/details/81181827)

其它还可以使用 C/C++ 的一些分析工具。

## 六、静态注册和动态注册

#### 静态注册：

通过 `JNIEXPORT` 和 `JNICALL` 两个宏定义声明，`Java + 包名 + 类名 + 方法名` 形式的函数名。不好的地方就是方法名太长了。

#### 动态注册：

通常在 `JNI_OnLoad` 方法中通过 `RegisterNatives` 方法注册，可以不再遵从固定的命名写法（当然为了代码容易理解，名称还是尽量和 Java 中保持一致）。

## 七、API

有的变态题目还是会考验你一些 API 的运用，比如怎么在 JNI 里面调用 Java 的方法，怎么在 JNI 里面抛异常等等。所以一些 API 还是要熟悉一下的，大致都是什么功能，名字大致是啥呀，这个太多了，看链接介绍吧：

[JNI 方法大全及使用示例](https://blog.csdn.net/afei__/article/details/81016413)
