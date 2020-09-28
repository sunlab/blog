### Activity 基础面试题

#### 1、Activity和Fragment生命周期有哪些？

![](https://devyk.oss-cn-qingdao.aliyuncs.com/blog/20200310210007.png)



#### 2、横竖屏切换时候Activity的生命周期

不设置Activity的android:configChanges时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。 设置Activity的android:configChanges="orientation"时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次 设置Activity的android:configChanges="orientation |keyboardHidden"时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法

>适配 Android 10 横竖屏切换配置：
>
><!-- 锁定布局在发生以下改变时，不重置状态 -->
><item name="android:configChanges">orientation|keyboardHidden|screenSize</item>
>
>



#### 3. activity的startActivity和context的startActivity区别？

(1)、从Activity中启动新的Activity时可以直接mContext.startActivity(intent)就好

(2)、如果从其他Context中启动Activity则必须给intent设置Flag:

```
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK) ;
mContext.startActivity(intent);
```

##### 

#### 4.  怎么加速启动Activity？

- onCreate() 中不执行耗时操作 把页面显示的 View 细分一下，放在 AsyncTask 里逐步显示，用 Handler 更好。这样用户的看到的就是有层次有步骤的一个个的 View 的展示，不会是先看到一个黑屏，然后一下显示所有 View。最好做成动画，效果更自然。
- 利用多线程的目的就是尽可能的减少 onCreate() 和 onReume() 的时间，使得用户能尽快看到页面，操作页面。
- 减少主线程阻塞时间。
- 提高 Adapter 和 AdapterView 的效率。
- 优化布局文件。

#### 5. 直接在Activity中创建一个thread跟在service中创建一个thread之间的区别？

在Activity中被创建：该Thread的就是为这个Activity服务的，完成这个特定的Activity交代的任务，主动通知该Activity一些消息和事件，Activity销毁后，该Thread也没有存活的意义了。

在Service中被创建：这是保证最长生命周期的Thread的唯一方式，只要整个Service不退出，Thread就可以一直在后台执行，一般在Service的onCreate()中创建，在onDestroy()中销毁。所以，在Service中创建的Thread，适合长期执行一些独立于APP的后台任务，比较常见的就是：在Service中保持与服务器端的长连接。



#### 6. LaunchMode应用场景

standard，创建一个新的Activity。

singleTop，栈顶不是该类型的Activity，创建一个新的Activity。否则，onNewIntent。

singleTask，回退栈中没有该类型的Activity，创建Activity，否则，onNewIntent+ClearTop。

注意:

设置了"singleTask"启动模式的Activity，它在启动的时候，会先在系统中查找属性值affinity等于它的属性值taskAffinity的Task存在；如果存在这样的Task，它就会在这个Task中启动，否则就会在新的任务栈中启动。因此， 如果我们想要设置了"singleTask"启动模式的Activity在新的任务中启动，就要为它设置一个独立的taskAffinity属性值。

如果设置了"singleTask"启动模式的Activity不是在新的任务中启动时，它会在已有的任务中查看是否已经存在相应的Activity实例， 如果存在，就会把位于这个Activity实例上面的Activity全部结束掉，即最终这个Activity 实例会位于任务的Stack顶端中。

在一个任务栈中只有一个"singleTask"启动模式的Activity存在。他的上面可以有其他的Activity。这点与singleInstance是有区别的。

singleInstance，回退栈中，只有这一个Activity，没有其他Activity。

singleTop适合接收通知启动的内容显示页面。

例如，某个新闻客户端的新闻内容页面，如果收到10个新闻推送，每次都打开一个新闻内容页面是很烦人的。

singleTask适合作为程序入口点。

例如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

singleInstance应用场景：

闹铃的响铃界面。 你以前设置了一个闹铃：上午6点。在上午5点58分，你启动了闹铃设置界面，并按 Home 键回桌面；在上午5点59分时，你在微信和朋友聊天；在6点时，闹铃响了，并且弹出了一个对话框形式的 Activity(名为 AlarmAlertActivity) 提示你到6点了(这个 Activity 就是以 SingleInstance 加载模式打开的)，你按返回键，回到的是微信的聊天界面，这是因为 AlarmAlertActivity 所在的 Task 的栈只有他一个元素， 因此退出之后这个 Task 的栈空了。如果是以 SingleTask 打开 AlarmAlertActivity，那么当闹铃响了的时候，按返回键应该进入闹铃设置界面。

#### 

#### 7. 说说Activity、Intent、Service 是什么关系

他们都是 Android 开发中使用频率最高的类。其中 Activity 和 Service 都是 Android 四大组件之一。他俩都是 Context 类的子类 ContextWrapper 的子类，因此他俩可以算是兄弟关系吧。不过兄弟俩各有各自的本领，Activity 负责用户界面的显示和交互，Service 负责后台任务的处理。Activity 和 Service 之间可以通过 Intent 传递数据，因此 可以把 Intent 看作是通信使者。



#### 8. ActivityA跳转ActivityB然后B按back返回A，各自的生命周期顺序，A与B均不透明。

##### ActivityA跳转到ActivityB：

```
Activity A&#xFF1A;onPause
Activity B&#xFF1A;onCreate
Activity B&#xFF1A;onStart
Activity B&#xFF1A;onResume
Activity A&#xFF1A;onStop
```

##### ActivityB返回ActivityA：

```
Activity B&#xFF1A;onPause
Activity A&#xFF1A;onRestart
Activity A&#xFF1A;onStart
Activity A&#xFF1A;onResume
Activity B&#xFF1A;onStop
Activity B&#xFF1A;onDestroy
```

#### 

#### 9. scheme跳转协议

Android中的scheme是一种页面内跳转协议，通过定义自己的scheme协议，可以跳转到app中的各个页面

服务器可以定制化告诉app跳转哪个页面

App可以通过跳转到另一个App页面

可以通过H5页面跳转页面



#### 10. 如何将一个Activity设置成窗口的样式。

中配置：

```
android:theme="@android:style/Theme.Dialog"
```

另外

```
android:theme="@android:style/Theme.Translucnt"
```

是设置透明。

#### 11. [Activity正常和异常情况下的生命周期](http://blog.csdn.net/geekerhw/article/details/48749935)

####12. 通过Acitivty的xml标签来改变任务栈的默认行为

- 使用`android:launchMode="standard|singleInstance|singleTask|singleTop"`来控制Acivity任务栈。

  **任务栈**是一种后进先出的结构。位于栈顶的Activity处于焦点状态,当按下back按钮的时候,栈内的Activity会一个一个的出栈,并且调用其`onDestory()`方法。如果栈内没有Activity,那么系统就会回收这个栈,每个APP默认只有一个栈,以APP的包名来命名.

  - standard : 标准模式,每次启动Activity都会创建一个新的Activity实例,并且将其压入任务栈栈顶,而不管这个Activity是否已经存在。Activity的启动三回调(*onCreate()->onStart()->onResume()*)都会执行。
  - singleTop : 栈顶复用模式.这种模式下,如果新Activity已经位于任务栈的栈顶,那么此Activity不会被重新创建,所以它的启动三回调就不会执行,同时Activity的`onNewIntent()`方法会被回调.如果Activity已经存在但是不在栈顶,那么作用与*standard模式*一样.
  - singleTask: 栈内复用模式.创建这样的Activity的时候,系统会先确认它所需任务栈已经创建,否则先创建任务栈.然后放入Activity,如果栈中已经有一个Activity实例,那么这个Activity就会被调到栈顶,`onNewIntent()`,并且singleTask会清理在当前Activity上面的所有Activity.(clear top)
  - singleInstance : 加强版的singleTask模式,这种模式的Activity只能单独位于一个任务栈内,由于栈内复用的特性,后续请求均不会创建新的Activity,除非这个独特的任务栈被系统销毁了

Activity的堆栈管理以ActivityRecord为单位,所有的ActivityRecord都放在一个List里面.可以认为一个ActivityRecord就是一个Activity栈



#### 13. Activity 缓存方法

有a、b两个Activity，当从a进入b之后一段时间，可能系统会把a回收，这时候按back，执行的不是a的onRestart而是onCreate方法，a被重新创建一次，这是a中的临时数据和状态可能就丢失了。

可以用Activity中的onSaveInstanceState()回调方法保存临时数据和状态，这个方法一定会在活动被回收之前调用。方法中有一个Bundle参数，putString()、putInt()等方法需要传入两个参数，一个键一个值。数据保存之后会在onCreate中恢复，onCreate也有一个Bundle类型的参数。

示例代码：

```
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //这里，当Acivity第一次被创建的时候为空
        //所以我们需要判断一下
        if( savedInstanceState != null ){
            savedInstanceState.getString("anAnt");
        }
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);

        outState.putString("anAnt","Android");

    }
```

一、onSaveInstanceState (Bundle outState)

当某个activity变得“容易”被系统销毁时，该activity的onSaveInstanceState就会被执行，除非该activity是被用户主动销毁的，例如当用户按BACK键的时候。

注意上面的双引号，何为“容易”？言下之意就是该activity还没有被销毁，而仅仅是一种可能性。这种可能性有哪些？通过重写一个activity的所有生命周期的onXXX方法，包括onSaveInstanceState和onRestoreInstanceState方法，我们可以清楚地知道当某个activity（假定为activity A）显示在当前task的最上层时，其onSaveInstanceState方法会在什么时候被执行，有这么几种情况：

1、当用户按下HOME键时。

这是显而易见的，系统不知道你按下HOME后要运行多少其他的程序，自然也不知道activity A是否会被销毁，故系统会调用onSaveInstanceState，让用户有机会保存某些非永久性的数据。以下几种情况的分析都遵循该原则

2、长按HOME键，选择运行其他的程序时。

3、按下电源按键（关闭屏幕显示）时。

4、从activity A中启动一个新的activity时。

5、屏幕方向切换时，例如从竖屏切换到横屏时。（如果不指定configchange属性） 在屏幕切换之前，系统会销毁activity A，在屏幕切换之后系统又会自动地创建activity A，所以onSaveInstanceState一定会被执行

总而言之，onSaveInstanceState的调用遵循一个重要原则，即当系统“未经你许可”时销毁了你的activity，则onSaveInstanceState会被系统调用，这是系统的责任，因为它必须要提供一个机会让你保存你的数据（当然你不保存那就随便你了）。另外，需要注意的几点：

1.布局中的每一个View默认实现了onSaveInstanceState()方法，这样的话，这个UI的任何改变都会自动地存储和在activity重新创建的时候自动地恢复。但是这种情况只有在你为这个UI提供了唯一的ID之后才起作用，如果没有提供ID，app将不会存储它的状态。

2.由于默认的onSaveInstanceState()方法的实现帮助UI存储它的状态，所以如果你需要覆盖这个方法去存储额外的状态信息，你应该在执行任何代码之前都调用父类的onSaveInstanceState()方法（super.onSaveInstanceState()）。 既然有现成的可用，那么我们到底还要不要自己实现onSaveInstanceState()?这得看情况了，如果你自己的派生类中有变量影响到UI，或你程序的行为，当然就要把这个变量也保存了，那么就需要自己实现，否则就不需要。

3.由于onSaveInstanceState()方法调用的不确定性，你应该只使用这个方法去记录activity的瞬间状态（UI的状态）。不应该用这个方法去存储持久化数据。当用户离开这个activity的时候应该在onPause()方法中存储持久化数据（例如应该被存储到数据库中的数据）。

4.onSaveInstanceState()如果被调用，这个方法会在onStop()前被触发，但系统并不保证是否在onPause()之前或者之后触发。

二、onRestoreInstanceState (Bundle outState)

至于onRestoreInstanceState方法，需要注意的是，onSaveInstanceState方法和onRestoreInstanceState方法“不一定”是成对的被调用的，（本人注：我昨晚调试时就发现原来不一定成对被调用的！）

onRestoreInstanceState被调用的前提是，activity A“确实”被系统销毁了，而如果仅仅是停留在有这种可能性的情况下，则该方法不会被调用，例如，当正在显示activity A的时候，用户按下HOME键回到主界面，然后用户紧接着又返回到activity A，这种情况下activity A一般不会因为内存的原因被系统销毁，故activity A的onRestoreInstanceState方法不会被执行

另外，onRestoreInstanceState的bundle参数也会传递到onCreate方法中，你也可以选择在onCreate方法中做数据还原。 还有onRestoreInstanceState在onstart之后执行。 至于这两个函数的使用，给出示范代码（留意自定义代码在调用super的前或后）：

```
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
        savedInstanceState.putBoolean("MyBoolean", true);
        savedInstanceState.putDouble("myDouble", 1.9);
        savedInstanceState.putInt("MyInt", 1);
        savedInstanceState.putString("MyString", "Welcome back to Android");
        // etc.
        super.onSaveInstanceState(savedInstanceState);
}

@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);

        boolean myBoolean = savedInstanceState.getBoolean("MyBoolean");
        double myDouble = savedInstanceState.getDouble("myDouble");
        int myInt = savedInstanceState.getInt("MyInt");
        String myString = savedInstanceState.getString("MyString");
}
```



#### 14. Activity 生命周期

- 启动Activity: onCreate()—>onStart()—>onResume()，Activity进入运行状态。
- Activity退居后台: 当前Activity转到新的Activity界面或按Home键回到主屏： onPause()—>onStop()，进入停滞状态。
- Activity返回前台: onRestart()—>**onStart()**—>onResume()，再次回到运行状态。
- Activity 退居后台，且系统内存不足， 系统会杀死这个后台状态的Activity（此时这个Activity引用仍然处在任务栈中，只是这个时候引用指向的对象已经为null），若再次回到这个Activity,则会走onCreate()–>onStart()—>onResume()(将重新走一次Activity的初始化生命周期)
- 锁屏：`onPause()->onStop()`
- 解锁：`onStart()->onResume()`



####15. 横竖屏切换时候Activity的生命周期

不设置Activity的android:configChanges时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。 设置Activity的android:configChanges=”orientation”时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次 设置Activity的android:configChanges=”orientation |keyboardHidden”时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法



####16. onRestart 什么时候调用？

【参考】

1、按下 home 键执行，再次打开这个 demo 执行；onRestart()--->onStart()--->onResume()

三个方法。

2、点击界面的 btn，跳转到另一个 Activity1，从 Activity1 返回，会执行如下图 2；onRestart()-

-->onStart()--->onResume()三个方法

3、切换到其他的应用，从其他应用切换回来。onRestart()--->onStart()--->onResume()三个

方法



####17. onSaveInstanceState 和onRestoreInstanceState 调用时机

【参考】

03-09 12:14:32.529 2298-2298/com.example.myapplication I/MY_TEST: onPause

03-09 12:14:32.556 2298-2298/com.example.myapplication I/MY_TEST: onCreate2

03-09 12:14:32.557 2298-2298/com.example.myapplication I/MY_TEST: onStart2

03-09 12:14:32.557 2298-2298/com.example.myapplication I/MY_TEST: onResume2

03-09 12:14:32.981 2298-2298/com.example.myapplication I/MY_TEST: onSaveInstanceState

一个参数

03-09 12:14:32.981 2298-2298/com.example.myapplication I/MY_TEST: onStop

分割线-------------------------------------------------------------------------

\---------------------

03-09 12:15:28.715 2298-2298/com.example.myapplication I/MY_TEST: onPause2

03-09 12:15:28.763 2298-2298/com.example.myapplication I/MY_TEST: onCreate

03-09 12:15:28.764 2298-2298/com.example.myapplication I/MY_TEST: onStart

03-09 12:15:28.764 2298-2298/com.example.myapplication I/MY_TEST: 

onRestoreInstanceState

03-09 12:15:28.767 2298-2298/com.example.myapplication I/MY_TEST: onActivityResult

03-09 12:15:28.767 2298-2298/com.example.myapplication I/MY_TEST: onResume

03-09 12:15:29.141 2298-2298/com.example.myapplication I/MY_TEST: onStop2

03-09 12:15:29.141 2298-2298/com.example.myapplication I/MY_TEST: onDestroy2

注意点：

1.跳转过程中，先执行 1 的 onpause，等等 onstop，等待 2 的 onresume 执行完之后，再执

行 1 的 onstop、ondestory

2.onSaveInstanceState 每次隐藏 activity 都会在 onpause 之后执行（即使 activity 没有销毁也

会执行）

3.onRestoreInstance 在 onstart 之后，onActivityResult 之前执行



#### 18. **Activity 与 Service 通信的四种方式**

【参考】

1、Binder

2、Intent3、接口 Interface

4、Broadcast 广播接收

http://itindex.net/detail/45126-android-service-activity



####19. **Activity 之间的几种通信方式**

【参考】

1、Intent

2、借助类的静态变量

3、借助全局变量/Application

4、借助外部工具

5、 借助 SharedPreference 

6、使用 Android 数据库 SQLite 

7、 赤裸裸的使用 File 

8、Android 剪切板

9、借助 Service

https://blog.csdn.net/cyanchen666/article/details/81982562



####20. AlertDialog，Toast 对 Activity 生命周期的影响

【参考】：无论 Dialog 弹出覆盖页面，对 Activity 生命周期没有影响，只有再启动另外一个

Activity 的时候才会进入 onPause 状态，而不是想象中的被覆盖或者不可见，同时通过

AlertDialog 源 码 或 者 Toast 源 码 我 们 都 可 以 发 现 它 们 实 现 的 原 理 都 是

windowmanager.addView();来添加的， 它们都是一个个 view ,因此不会对 activity 的生命周

期有任何影响。

https://blog.csdn.net/cloud_castle/article/details/56011562



####21. **onCreate 中的 Bundle 有什么用？**

【参考】 Activity 中有一个名称叫 onCreate 的方法。该方法是在 Activity 创建时被系统调

用，是一个 Activity 生命周期的开始。可是有一点容易被忽视，就是 onCreate 方法的参数

saveInsanceState。一般的程序开发中，很少用到这个参数。

onCreate 方法的完整定义如下:

```java
public void onCreate(Bundle saveInsanceState){super.onCreate(saveInsanceState);

}
```

Bundle 类型的数据与 Map 类型的数据相似，都是以 key-value 的形式存储数据的。

从字面上看 saveInsanceState，是保存实例状态的。实际上，saveInsanceState 也就是保存

Activity 的状态的。那么，saveInsanceState 中的状态数据是从何处而来的呢？下面我们介绍

Activity 的另一个方法 saveInsanceState。onsaveInsanceState 方法是用来保存 Activity 的状态的。当一个 Activity 在生命周期结束前，会调用该方法保存状态。

如下所示：

```java
public void onSaveInsanceState(Bundle saveInsanceState){

super.onSaveInsanceState(saveInsanceState);

}
```

在实际应用中，当一个 Activity 结束前，如果需要保存状态，就在 onsaveInsanceState 中，

将状态数据以 key-value 的形式放入到 saveInsanceState 中。这样，当一个 Activity 被创建

时，就能从 onCreate 的参数 saveInsanceState 中获得状态数据。

状态这个参数在实现应用中有很大的用途，比如：一个游戏在退出前，保存一下当前游戏运

行的状态，当下次开启时能接着上次的继续玩下去。再比如：电子书程序，当一本小说被阅

读到第 199 页后退出了（不管是内存不足还是用户自动关闭程序），当下次打开时，读者可

能已忘记了上次已阅读到第几页了，但是，读者想接着上次的读下去。如果采用

saveInstallState 参数，就很容易解决上述问题。

https://blog.csdn.net/liubin8095/article/details/9328563 



####22. onSaveInstanceState() 与 onRestoreIntanceState()

Activity的 onSaveInstanceState() 和 onRestoreInstanceState()并不是生命周期方法，它们不同于 onCreate()、onPause()等生命周期方法，它们并不一定会被触发。当应用遇到意外情况（如：内存不足、用户直接按Home键）由系统销毁一个Activity时，onSaveInstanceState() 会被调用。但是当用户主动去销毁一个Activity时，例如在应用中按返回键，onSaveInstanceState()就不会被调用。因为在这种情况下，用户的行为决定了不需要保存Activity的状态。通常onSaveInstanceState()只适合用于保存一些临时性的状态，而onPause()适合用于数据的持久化保存。 在activity被杀掉之前调用保存每个实例的状态,以保证该状态可以在onCreate(Bundle)或者onRestoreInstanceState(Bundle) (传入的Bundle参数是由onSaveInstanceState封装好的)中恢复。这个方法在一个activity被杀死前调用，当该activity在将来某个时刻回来时可以恢复其先前状态。 例如，如果activity B启用后位于activity A的前端，在某个时刻activity A因为系统回收资源的问题要被杀掉，A通过onSaveInstanceState将有机会保存其用户界面状态，使得将来用户返回到activity A时能通过onCreate(Bundle)或者onRestoreInstanceState(Bundle)恢复界面的状态







