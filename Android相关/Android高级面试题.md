# Android高级面试题 （⭐⭐⭐）


## 一、性能优化

### 1、做过哪些性能优化？是怎么评测和具体优化的？

#### 一、App启动速度优化

![image](https://github.com/JsonChao/Awesome-Android-Interview/blob/master/screenshot/App%E5%90%AF%E5%8A%A8%E9%80%9F%E5%BA%A6%E4%BC%98%E5%8C%96.png?raw=true)

##### [开放问题：如果提高启动速度，设计一个延迟加载框架或者sdk的方法和注意的问题](https://blog.csdn.net/dd864140130/article/details/53558011)


#### 二、App绘制优化

![image](https://github.com/JsonChao/Awesome-Android-Interview/blob/master/screenshot/App%E7%BB%98%E5%88%B6%E4%BC%98%E5%8C%96.png?raw=true)


#### 三、App内存优化

![image](https://github.com/JsonChao/Awesome-Android-Interview/blob/master/screenshot/App%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96.png?raw=true)

#### 四、App瘦身

![image](https://github.com/JsonChao/Awesome-Android-Interview/blob/master/screenshot/App%E7%98%A6%E8%BA%AB.png?raw=true)


#### 五、App电量优化

![image](https://github.com/JsonChao/Awesome-Android-Interview/blob/master/screenshot/App%E7%94%B5%E9%87%8F%E4%BC%98%E5%8C%96.png?raw=true)


#### 六、网络优化
    
##### 移动端获取网络数据优化的几个点

- 1、连接复用：节省连接建立时间，如开启 keep-alive。于Android来说默认情况下HttpURLConnection和HttpClient都开启了keep-alive。只是2.2之前HttpURLConnection存在影响连接池的Bug。
    
- 2、请求合并：即将多个请求合并为一个进行请求，比较常见的就是网页中的CSS Image Sprites。如果某个页面内请求过多，也可以考虑做一定的请求合并。
    
- 3、减少请求数据的大小：对于post请求，body可以做gzip压缩的，header也可以做数据压缩(不过只支持http 2.0)。
返回数据的body也可以做gzip压缩，body数据体积可以缩小到原来的30%左右（也可以考虑压缩返回的json数据的key数据的体积，尤其是针对返回数据格式变化不大的情况，支付宝聊天返回的数据用到了）。

- 4、根据用户的当前的网络质量来判断下载什么质量的图片（电商用的比较多）。

##### [客户端网络安全实现](http://mrpeak.cn/blog/encrypt/)


#### 七、安卓的安全优化

##### 提高app安全性的方法？


##### 安卓的app加固如何做？


##### 安卓的混淆原理是什么？


##### 谈谈你对安卓签名的理解。


### 2、为什么WebView加载会慢呢？

这是因为在客户端中，加载H5页面之前，需要先初始化WebView，在WebView完全初始化完成之前，后续的界面加载过程都是被阻塞的。

优化手段围绕着以下两个点进行：

- 预加载WebView。
- 加载WebView的同时，请求H5页面数据。

因此常见的方法是：

- 全局WebView。
- 客户端代理页面请求。WebView初始化完成后向客户端请求数据。
- asset存放离线包。

除此之外还有一些其他的优化手段：

- 脚本执行慢，可以让脚本最后运行，不阻塞页面解析。
- DNS链接慢，可以让客户端复用使用的域名与链接。
- React框架代码执行慢，可以将这部分代码拆分出来，提前进行解析。


### 3、如何优化自定义View

为了加速你的view，对于频繁调用的方法，需要尽量减少不必要的代码。先从onDraw开始，需要特别注意不应该在这里做内存分配的事情，因为它会导致GC，从而导致卡顿。在初始化或者动画间隙期间做分配内存的动作。不要在动画正在执行的时候做内存分配的事情。

你还需要尽可能的减少onDraw被调用的次数，大多数时候导致onDraw都是因为调用了invalidate().因此请尽量减少调用invaildate()的次数。如果可能的话，尽量调用含有4个参数的invalidate()方法而不是没有参数的invalidate()。没有参数的invalidate会强制重绘整个view。

另外一个非常耗时的操作是请求layout。任何时候执行requestLayout()，会使得Android UI系统去遍历整个View的层级来计算出每一个view的大小。如果找到有冲突的值，它会需要重新计算好几次。另外需要尽量保持View的层级是扁平化的，这样对提高效率很有帮助。

如果你有一个复杂的UI，你应该考虑写一个自定义的ViewGroup来执行他的layout操作。与内置的view不同，自定义的view可以使得程序仅仅测量这一部分，这避免了遍历整个view的层级结构来计算大小。


### 4、FC(Force Close)什么时候会出现？

Error、OOM，StackOverFlowError、Runtime,比如说空指针异常

解决的办法：

- 注意内存的使用和管理
- 使用Thread.UncaughtExceptionHandler接口


### 5、[Java多线程引发的性能问题，怎么解决](https://blog.csdn.net/luofenghan/article/details/78596950)？


## 二、Android Framework相关

### 1、Android系统架构

![image](https://developer.android.com/guide/platform/images/android-stack_2x.png?hl=zh-cn)

Android 是一种基于 Linux 的开放源代码软件栈，为广泛的设备和机型而创建。下图所示为 Android 平台的五大组件：
    
1.应用程序 

Android 随附一套用于电子邮件、短信、日历、互联网浏览和联系人等的核心应用。平台随附的应用与用户可以选择安装的应用一样，没有特殊状态。因此第三方应用可成为用户的默认网络浏览器、短信 Messenger 甚至默认键盘（有一些例外，例如系统的“设置”应用）。

系统应用可用作用户的应用，以及提供开发者可从其自己的应用访问的主要功能。例如，如果您的应用要发短信，您无需自己构建该功能，可以改为调用已安装的短信应用向您指定的接收者发送消息。
      
2、Java API 框架  

您可通过以 Java 语言编写的 API 使用 Android OS 的整个功能集。这些 API 形成创建 Android 应用所需的构建块，它们可简化核心模块化系统组件和服务的重复使用，包括以下组件和服务：

- 丰富、可扩展的视图系统，可用以构建应用的 UI，包括列表、网格、文本框、按钮甚至可嵌入的网络浏览器
- 资源管理器，用于访问非代码资源，例如本地化的字符串、图形和布局文件
- 通知管理器，可让所有应用在状态栏中显示自定义提醒
- Activity 管理器，用于管理应用的生命周期，提供常见的导航返回栈
- 内容提供程序，可让应用访问其他应用（例如“联系人”应用）中的数据或者共享其自己的数据

开发者可以完全访问 Android 系统应用使用的框架 API。
      
3、系统运行库 

1)原生 C/C++ 库 

许多核心 Android 系统组件和服务（例如 ART 和 HAL）构建自原生代码，需要以 C 和 C++ 编写的原生库。Android 平台提供 Java 框架 API 以向应用显示其中部分原生库的功能。例如，您可以通过 Android 框架的 Java OpenGL API 访问 OpenGL ES，以支持在应用中绘制和操作 2D 和 3D 图形。如果开发的是需要 C 或 C++ 代码的应用，可以使用 Android NDK 直接从原生代码访问某些原生平台库。

2)Android Runtime

对于运行 Android 5.0（API 级别 21）或更高版本的设备，每个应用都在其自己的进程中运行，并且有其自己的 Android Runtime (ART) 实例。ART 编写为通过执行 DEX 文件在低内存设备上运行多个虚拟机，DEX 文件是一种专为 Android 设计的字节码格式，经过优化，使用的内存很少。编译工具链（例如 Jack）将 Java 源代码编译为 DEX 字节码，使其可在 Android 平台上运行。

ART 的部分主要功能包括：

- 预先 (AOT) 和即时 (JIT) 编译
- 优化的垃圾回收 (GC)
- 更好的调试支持，包括专用采样分析器、详细的诊断异常和崩溃报告，并且能够设置监视点以监控特定字段

在 Android 版本 5.0（API 级别 21）之前，Dalvik 是 Android Runtime。如果您的应用在 ART 上运行效果很好，那么它应该也可在 Dalvik 上运行，但反过来不一定。

Android 还包含一套核心运行时库，可提供 Java API 框架使用的 Java 编程语言大部分功能，包括一些 Java 8 语言功能。
      
4、硬件抽象层 (HAL) 

硬件抽象层 (HAL) 提供标准界面，向更高级别的 Java API 框架显示设备硬件功能。HAL 包含多个库模块，其中每个模块都为特定类型的硬件组件实现一个界面，例如相机或蓝牙模块。当框架 API 要求访问设备硬件时，Android 系统将为该硬件组件加载库模块。
      
5、Linux 内核 

Android 平台的基础是 Linux 内核。例如，Android Runtime (ART) 依靠 Linux 内核来执行底层功能，例如线程和低层内存管理。使用 Linux 内核可让 Android 利用主要安全功能，并且允许设备制造商为著名的内核开发硬件驱动程序。

#### 对于Android应用开发来说，最好能手绘下面的系统架构图：
    
![image](https://raw.githubusercontent.com/BeesAndroid/BeesAndroid/master/art/android_system_structure.png)


### 2、View的事件分发机制？滑动冲突怎么解决？

[更全面的了解请移步到此处](https://jsonchao.github.io/2018/10/17/Android%E8%A7%A6%E6%91%B8%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6/)

1).Android事件分发机制的本质是要解决：点击事件由个对象发出，经过哪些对象，最终达到哪个对象并最终到处理。这里的对象是指Activity、ViewGroup、View.

2).Android中事件分发顺序：Activity（Window） ->ViewGroup -> View.

3).事件分发过程由dispatchTouchEvent()、onInterceptTouchEvent()和onTouchEvent()三个方协助完成

设置Button按钮来响应点击事件事件传递情况：（如下图）

布局如下:

![image](https://user-gold-cdn.xitu.io/2017/11/21/15fdc4cfaed36b0e?imageslim)
    
最外层：Activiy A，包含两个子View：ViewGroupB、View C

中间层：ViewGroup B，包含一个子View：View C

最内层：View C

假设用户首先触摸到屏幕上ViewC上的某个点（如图中黄色区域），那么Action_DOWN事就在该点产生，然后用户移动手指并最后离开屏幕。

按钮点击事件:

DOWN事件被传递给C的onTouchEvent方法，该方法返回tre，表示处理这个事件;

因为C正在处理这个事件，那么DOWN事件将不再往上传给B和A的onTouchEvent()；

该事件列的其他事件（Move、Up）也将传递给C的onToucEvent();

![image](https://user-gold-cdn.xitu.io/2017/11/21/15fdc4cfd00ab478?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![image](https://upload-images.jianshu.io/upload_images/2911038-5349d6ebb32372da)

(记住这个图的传递顺序,面试的时候能够画出来,就很详细了)

#### ACTION_CANCEL什么时候触发，触摸button然后滑动到外部抬起会触发点击事件吗，在+ + 滑动回去抬起会么？


##### 点击事件被拦截，但是想传到下面的View，如何操作？


#### 如何解决ScrollView嵌套ListView和GridView产生的冲突？

https://blog.csdn.net/btt2013/article/details/53447649


### 3、View的绘制流程？

1.ViewRootImpl会调用performTraversals(),其内部会用performMeasure()、performLayout、performDraw()。

2.performMeasure()会调用最外层的ViewGroup的measur()-->onMeasure(),ViewGroup的onMeasure()是抽象方，但其提供了measureChildren()，这之中会遍历子Vie然后循环调用measureChild()这之中会用getChildMeasueSpec()+父View的MeasureSpec+子View的LayoutParam起获取本View的MeasureSpec，然后调用子View的measur()到View的onMeasure()-->setMeasureDimension(getDeaultSize(),getDefaultSize()),getDefaultSize()默认返回measureSpec的测量数值，所以继承View进行自定义的wrap_content需要重写。

3.performLayout()会调用最外层的ViewGroup的layout(,t,r,b),本View在其中使用setFrame()设置本View的四顶点位置。在onLayout(抽象方法)中确定子View的位置如LinearLayout会遍历子View，循环调用setChildFrame-->子View.layout()。

4.performDraw()会调用最外层ViewGroup的draw():其会先后调用background.draw()(绘制背景)、onDraw()(制自己)、dispatchDraw()(绘制子View)、onDrawScrollars()(绘制装饰)。

5.MeasureSpec由2位SpecMode(UNSPECIFIED、EXACTLY(应精确值和match_parent)、AT_MOST(对应warp_content)和30位SpecSize组成一个int,DecorView的MeasureSpe由窗口大小和其LayoutParams决定，其他View由父ViewMeasureSpec和本View的LayoutParams决定。ViewGroup有getChildMeasureSpec()来获取子View的MeasureSpec。

6.三种方式获取measure()后的宽高：

- Activity#onWindowFocusChange()中调用获取
- view.post(Runnable)将获取的代码投递到消息队列尾部。
- ViewTreeObservable.

[全面的了解请点击此处](https://jsonchao.github.io/2018/10/28/Android%20View%E7%9A%84%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B/)

#### View刷新机制

##### Requestlayout，onlayout，onDraw，DrawChild区别与联系

requestLayout()方法 ：会导致调用measure()过程 和 layout()过程 。 将会根据标志位判断是否需要ondraw

onLayout()方法(如果该View是ViewGroup对象，需要实现该方法，对每个子视图进行布局)

调用onDraw()方法绘制视图本身 (每个View都需要重载该方法，ViewGroup不需要实现该方法)

drawChild()去重新回调每个子视图的draw()方法

##### invalidate和postInvalidate的区别及使用


### 4、跨进程通信。

#### [AIDL使用](http://blog.csdn.net/singwhatiwanna/article/details/17041691)

AIDL (Android Interface Definition Language) 是一种AIDL 语言，用于生成可以在Android设备上两个进程之间进行进程间通信(interprocess communication, IPC)的代码。如果在一个进程中（例如Activity）要调用另一个进程中（例如Service）对象的操作，就可以使用AIDL生成可序列化的参数。 AIDL IPC机制是面向接口的，像COM或Corba一样，但是更加轻量级。它是使用代理类在客户端和实现端传递数据。如果要使用AIDL, 需要完成2件事情: 1. 引入AIDL的相关类.; 2. 调用aidl产生的class.

理论上, 参数可以传递基本数据类型和String, 还有就是Bundle的派生类, 不过在Eclipse中,目前的ADT不支持Bundle做为参数，具体实现步骤如下:

- 1、创建AIDL文件, 在这个文件里面定义接口, 该接口定义了可供客户端访问的方法和属性。
- 2、编译AIDL文件, 用Ant的话, 可能需要手动, 使用Eclipse plugin的话,可以根据adil文件自动生产java文件并编译, 不需要人为介入.
- 3、在Java文件中, 实现AIDL中定义的接口. 编译器会根据AIDL接口, 产生一个JAVA接口。这个接口有一个名为Stub的内部抽象类，它继承扩展了接口并实现了远程调用需要的几个方法。接下来就需要自己去实现自定义的几个接口了.
- 4、向客户端提供接口ITaskBinder, 如果写的是service，扩展该Service并重载onBind ()方法来返回一个实现上述接口的类的实例。
- 5、在服务器端回调客户端的函数. 前提是当客户端获取的IBinder接口的时候,要去注册回调函数, 只有这样, 服务器端才知道该调用那些函数
    
AIDL语法很简单,可以用来声明一个带一个或多个方法的接口，也可以传递参数和返回值。 由于远程调用的需要, 这些参数和返回值并不是任何类型.下面是些AIDL支持的数据类型:
    
- 不需要import声明的简单Java编程语言类型(int,boolean等)
- String, CharSequence不需要特殊声明
- List, Map和Parcelables类型, 这些类型内所包含的数据成员也只能是简单数据类型, String等其他比支持的类型.
    
实现接口时有几个原则:

- 1.抛出的异常不要返回给调用者. 跨进程抛异常处理是不可取的.
- 2.IPC调用是同步的。如果你知道一个IPC服务需要超过几毫秒的时间才能完成地话，你应该避免在Activity的主线程中调用。也就是IPC调用会挂起应用程序导致界面失去响应. 这种情况应该考虑单起一个线程来处理.
- 3.不能在AIDL接口中声明静态属性。

IPC的调用步骤:

    声明一个接口类型的变量，该接口类型在.aidl文件中定义。
    实现ServiceConnection。
    调用ApplicationContext.bindService(),并在ServiceConnection实现中进行传递.
    在ServiceConnection.onServiceConnected()实现中，你会接收一个IBinder实例(被调用的Service). 调用
    YourInterfaceName.Stub.asInterface((IBinder)service)将参数转换为YourInterface类型。
    调用接口中定义的方法。你总要检测到DeadObjectException异常，该异常在连接断开时被抛出。它只会被远程方法抛出。
    断开连接，调用接口实例中的ApplicationContext.unbindService()
    aidl主要就是帮助我们完成了包装数据和解包的过程，并调用了transact过程，而用来传递的数据包我们就称为parcel
    AIDL: xxx.aidl->xxx.java,注册service
    用aidl定义需要被调用方法接口
    实现这些方法
    调用这些方法


#### Binder机制

IPC:

![image](https://user-gold-cdn.xitu.io/2017/10/10/a1cd0604f7807e215047053498e6daad?imageslim)

通信，利用进程间可共享的内核内存空间来完成底层通信工作的，Client 端与 Server 端进程往往采用 ioctl 等方法跟内核空间的驱动进行交互。

Binder 原理:

Binder 通信采用 C/S 架构，包含 Client、Server、ServiceManager 以及 binder 驱动，其中 ServiceManager 用于管理系统中的各种服务。
    
架构图如下所示：

![image](https://raw.githubusercontent.com/wangkuiwu/android_applets/master/os/pic/binder/binder_frame.jpg)

Binder 四个角色：

- Client 进程：使用服务的进程
- Server 进程：提供服务的进程
- ServiceManager 进程：将字符型是的 Binder 名字转为 Client 中对该 Binder 的引用，使得 Client 能够通过 Binder 名字获取到 Server 中 Binder 实体的引用。
- Binder 驱动：进程间 Binder 通信的建立，Binder 在进程间的传递，Binder 引用计数管理，数据包在进程间传递和交互等一系列底层支持。

Binder 运行机制：

- 注册服务：Server 在 ServiceManager 注册服务
- 获取服务：Client 从 ServiceManager 获取相应的 Service
- 使用服务：Client 根据得到的 Service 信息建立与 Service 所在的 Server 进程通信的通路可与 Server 进行交互。
    
Android Binder是用来做进程通信的，Android的各个应用以及系统服务都运行在独立的进程中，它们的通信都依赖于Binder。

为什么选用Binder，在讨论这个问题之前，我们知道Android也是基于Linux内核，Linux现有的进程通信手段有以下几种：

管道：在创建时分配一个page大小的内存，缓存区大小比较有限；

消息队列：信息复制两次，额外的CPU消耗；不合适频繁或信息量大的通信；

共享内存：无须复制，共享缓冲区直接付附加到进程虚拟地址空间，速度快；但进程间的同步问题操作系统无法实现，必须各进程利用同步工具解决；

套接字：作为更通用的接口，传输效率低，主要用于不通机器或跨网络的通信；

信号量：常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。6. 信号: 不适用于信息交换，更适用于进程中断控制，比如非法内存访问，杀死某个进程等；

既然有现有的IPC方式，为什么重新设计一套Binder机制呢。主要是出于以上三个方面的考量：

高性能：从数据拷贝次数来看Binder只需要进行一次内存拷贝，而管道、消息队列、Socket都需要两次，共享内存不需要拷贝，Binder的性能仅次于共享内存。

稳定性：上面说到共享内存的性能优于Binder，那为什么不适用共享内存呢，因为共享内存需要处理并发同步问题，控制负责，容易出现死锁和资源竞争，稳定性较差。而Binder基于C/S架构，客户端与服务端彼此独立，稳定性较好。

安全性：我们知道Android为每个应用分配了UID，用来作为鉴别进程的重要标志，Android内部也依赖这个UID进行权限管理，包括6.0以前的固定权限和6.0以后的动态权限，传荣IPC只能由用户在数据包里填入UID/PID，这个标记完全 是在用户空间控制的，没有放在内核空间，因此有被恶意篡改的可能，因此Binder的安全性更高。

1).首先，Binder分为Client和Server两个进程。


    注意，Client和Server是相对的。谁发消息，谁就是Client，谁接收消息，谁就是Server。
    
    
    举个例子，两个进程A和B之间使用Binder通信，进程A发消息给进程B，那么这时候A是Binder Client，B是Binder Server；进程B发消息给进程A，那么这时候B是Binder Client，A是Binder Server——其实这么说虽然简单了，但还是不太严谨，我们先这么理解着。


2).其次，我们看下面这个图（摘自田维术的博客），基本说明白了Binder的组成解构：

![image](https://user-gold-cdn.xitu.io/2017/11/21/15fdc4cfd565735a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

图中的IPC就是进程间通信的意思。

图中的ServiceManager，负责把Binder Server注册到一个容器中。

有人把ServiceManager比喻成电话局，存储着每个住宅的座机电话，还是很恰当的。张三给李四打电话，拨打电话号码，会先转接到电话局，电话局的接线员查到这个电话号码的地址，因为李四的电话号码之前在电话局注册过，所以就能拨通；没注册，就会提示该号码不存在。
    
对照着Android Binder机制，对着上面这图，张三就是Binder Client，李四就是Binder Server，电话局就是ServiceManager，电话局的接线员在这个过程中做了很多事情，对应着图中的Binder驱动.

3).接下来我们看Binder通信的过程，还是摘自田维术博客的一张图：

![image](https://user-gold-cdn.xitu.io/2017/11/21/15fdc4cfd6cf0739?imageslim)

注：图中的SM也就是ServiceManager。

我们看到，Client想要直接调用Server的add方法，是不可以的，因为它们在不同的进程中，这时候就需要Binder来帮忙了。
    
首先是Server在SM这个容器中注册。
    
其次，Client想要调用Server的add方法，就需要先获取Server对象， 但是SM不会把真正的Server对象返回给Client，而是把Server的一个代理对象返回给Client，也就是Proxy。
    
然后，Client调用Proxy的add方法，SM会帮他去调用Server的add方法，并把结果返回给Client。
    
以上这3步，Binder驱动出了很多力，但我们不需要知道Binder驱动的底层实现，涉及到C++的代码了——把有限的时间去做更有意义的事情。

2.为什么android选用Binder来实现进程间通信？

1).可靠性。在移动设备上，通常采用基于Client-Server的通信方式来实现互联网与设备间的内部通信。目前linux支持IPC包括传统的管道，System V IPC，即消息队列/共享内存/信号量，以及socket中只有socket支持Client-Server的通信方式。Android系统为开发者提供了丰富进程间通信的功能接口，媒体播放，传感器，无线传输。
    
这些功能都由不同的server来管理。开发都只关心将自己应用程序的client与server的通信建立起来便可以使用这个服务。毫无疑问，如若在底层架设一套协议来实现Client-Server通信，增加了系统的复杂性。在资源有限的手机 上来实现这种复杂的环境，可靠性难以保证。
    
2).传输性能。socket主要用于跨网络的进程间通信和本机上进程间的通信，但传输效率低，开销大。消息队列和管道采用存储-转发方式，即数据先从发送方缓存区拷贝到内核开辟的一块缓存区中，然后从内核缓存区拷贝到接收方缓存区，其过程至少有两次拷贝。虽然共享内存无需拷贝，但控制复杂。比较各种IPC方式的数据拷贝次数。共享内存：0次。Binder：1次。Socket/管道/消息队列：2次。
    
3).安全性。Android是一个开放式的平台，所以确保应用程序安全是很重要的。Android对每一个安装应用都分配了UID/PID,其中进程的UID是可用来鉴别进程身份。传统的只能由用户在数据包里填写UID/PID，这样不可靠，容易被恶意程序利用。而我们要求由内核来添加可靠的UID。
    
所以，出于可靠性、传输性、安全性。android建立了一套新的进程间通信方式。

请按顺序仔细阅读下列文章提升对Binder机制的理解程度：

[Android跨进程通信：图文详解 Binder机制 原理](https://blog.csdn.net/carson_ho/article/details/73560642)

[Binder学习指南](http://weishu.me/2016/01/12/binder-index-for-newer/)

[Binder设计与实现](https://blog.csdn.net/universus/article/details/6211589)

[老罗Binder机制分析系列或Android系统源代码情景分析Binder章节](https://blog.csdn.net/luoshengyang/article/details/6618363)


### 5、Android系统启动流程是什么？（提示：Zygote进程 –> SystemServer进程 –> 各种系统服务 –> 应用进程）



### 6、启动一个程序，可以主界面点击图标进入，也可以从一个程序中跳转过去，二者有什么区别？

是因为启动程序（主界面也是一个app），发现了在这个程序中存在一个设置为<category android:name="android.intent.category.LAUNCHER" />的activity,
所以这个launcher会把icon提出来，放在主界面上。当用户点击icon的时候，发出一个Intent：
    
    Intent intent = mActivity.getPackageManager().getLaunchIntentForPackage(packageName);
    mActivity.startActivity(intent);   
    
跳过去可以跳到任意允许的页面，如一个程序可以下载，那么真正下载的页面可能不是首页（也有可能是首页），这时还是构造一个Intent，startActivity。这个intent中的action可能有多种view，download都有可能。系统会根据第三方程序向系统注册的功能，为你的Intent选择可以打开的程序或者页面。所以唯一的一点
不同的是从icon的点击启动的intent的action是相对单一的，从程序中跳转或者启动可能样式更多一些。本质是相同的。


### 7、AMS家族重要术语解释。

1.ActivityManagerServices，简称AMS，服务端对象，负责系统中所有Activity的生命周期。

2.ActivityThread，App的真正入口。当开启App之后，调用main()开始运行，开启消息循环队列，这就是传说的UI线程或者叫主线程。与ActivityManagerService一起完成Activity的管理工作。

3.ApplicationThread，用来实现ActivityManagerServie与ActivityThread之间的交互。在ActivityManagerSevice需要管理相关Application中的Activity的生命周期时，通过ApplicationThread的代理对象与ActivityThread通信。

4.ApplicationThreadProxy，是ApplicationThread在服务器端的代理，负责和客户端的ApplicationThread通信。AMS就是通过该代理与ActivityThread进行通信的。

5.Instrumentation，每一个应用程序只有一个Instrumetation对象，每个Activity内都有一个对该对象的引用，Instrumentation可以理解为应用进程的管家，ActivityThread要创建或暂停某个Activity时，都需要通过Instrumentation来进行具体的操作。

6.ActivityStack，Activity在AMS的栈管理，用来记录经启动的Activity的先后关系，状态信息等。通过ActivtyStack决定是否需要启动新的进程。

7.ActivityRecord，ActivityStack的管理对象，每个Acivity在AMS对应一个ActivityRecord，来记录Activity状态以及其他的管理信息。其实就是服务器端的Activit对象的映像。

8.TaskRecord，AMS抽象出来的一个“任务”的概念，是记录ActivityRecord的栈，一个“Task”包含若干个ActivityRecord。AMS用TaskRecord确保Activity启动和退出的顺序。如果你清楚Activity的4种launchMode，那么对这概念应该不陌生。


### 8、App启动流程（Activity的冷启动流程）。

整个应用程序的启动过程要执行很多步骤，但是整体来看，主要分为以下五个阶段：

一.Launcher通过Binder进程间通信机制通知ActityManagerService，它要启动一个Activity；

二.ActivityManagerService通过Binder进程间机制通知Launcher进入Paused状态；

三.Launcher通过Binder进程间通信机制通知ActityManagerService，它已经准备就绪进入Paused状态，于是ActivityManagerService就创建一个新的进程，用来启动一个ActivityThread实例，即将要启动的Activity就是在这个ActivityThread实例中运行；
    
四.ActivityThread通过Binder进程间通信机制将一个ApplicationThread类型的Binder对象传递给ActivityManagerService，以便以后ActivityManagerService能够通过这个Binder对象和它进行通信；
    
五 ：ActivityManagerService通过Binder进程间通信机制通知ActivityThread，现在一切准备就绪，它可以真正执行Activity的启动操作了。

点击应用图标后会去启动应用的LauncherActivity，如果LancerActivity所在的进程没有创建，还会创建新进程，整体的流程就是一个Activity的启动流程。

Activity的启动流程图（放大可查看）如下所示：

![image](https://github.com/guoxiaoxing/android-open-source-project-analysis/raw/master/art/app/component/activity_start_flow.png)

整个流程涉及的主要角色有：

Instrumentation: 监控应用与系统相关的交互行为。

AMS：组件管理调度中心，什么都不干，但是什么都管。

ActivityStarter：Activity启动的控制器，处理Intent与Flag对Activity启动的影响，具体说来有：1 寻找符合启动条件的Activity，如果有多个，让用户选择；2 校验启动参数的合法性；3 返回int参数，代表Activity是否启动成功。

ActivityStackSupervisior：这个类的作用你从它的名字就可以看出来，它用来管理任务栈。

ActivityStack：用来管理任务栈里的Activity。

ActivityThread：最终干活的人，Activity、Service、BroadcastReceiver的启动、切换、调度等各种操作都在这个类里完成。

注：这里单独提一下ActivityStackSupervisior，这是高版本才有的类，它用来管理多个ActivityStack，早期的版本只有一个ActivityStack对应着手机屏幕，后来高版本支持多屏以后，就有了多个ActivityStack，于是就引入了ActivityStackSupervisior用来管理多个ActivityStack。

整个流程主要涉及四个进程：

- 调用者进程，如果是在桌面启动应用就是Launcher应用进程。
- ActivityManagerService等待所在的System Server进程，该进程主要运行着系统服务组件。
- Zygote进程，该进程主要用来fork新进程。
- 新启动的应用进程，该进程就是用来承载应用运行的进程了，它也是应用的主线程（新创建的进程就是主线程），处理组件生命周期、界面绘制等相关事情。

有了以上的理解，整个流程可以概括如下：

- 1、点击桌面应用图标，Launcher进程将启动Activity（MainActivity）的请求以Binder的方式发送给了AMS。
- 2、AMS接收到启动请求后，交付ActivityStarter处理Intent和Flag等信息，然后再交给ActivityStackSupervisior/ActivityStack 处理Activity进栈相关流程。同时以Socket方式请求Zygote进程fork新进程。
- 3、Zygote接收到新进程创建请求后fork出新进程。
- 4、在新进程里创建ActivityThread对象，新创建的进程就是应用的主线程，在主线程里开启Looper消息循环，开始处理创建Activity。
- 5、ActivityThread利用ClassLoader去加载Activity、创建Activity实例，并回调Activity的onCreate()方法，这样便完成了Activity的启动。

最后，再看看另一幅启动流程图来加深理解：

![image](http://img.mp.itc.cn/upload/20170329/ca9567ce3bf04c4abdb4d124cebfee76_th.jpeg)


### 9、ActivityThread工作原理。


### 10、说下四大组件的启动过程，四大组件的启动与销毁的方式。


#### 广播发送和接收的原理了解吗？

- 继承BroadcastReceiver，重写onReceive()方法。
- 通过Binder机制向ActivityManagerService注册广播。
- 通过Binder机制向ActivityMangerService发送广播。
- ActivityManagerService查找符合相应条件的广播（IntentFilter/Permission）的BroadcastReceiver，将广播发送到BroadcastReceiver所在的消息队列中。
- BroadcastReceiver所在消息队列拿到此广播后，回调它的onReceive()方法。



### 11、AMS是如何管理Activity的？


### 12、理解Window和WindowManager。

1.Window用于显示View和接收各种事件，Window有三种型：应用Window(每个Activity对应一个Window)、子Widow(不能单独存在，附属于特定Window)、系统window(toast和状态栏)

2.Window分层级，应用Window在1-99、子Window在1000-999、系统Window在2000-2999.WindowManager提供了增改View的三个功能。

3.Window是个抽象概念：每一个Window对应着一个ViewRootImpl，Window通过ViewRootImpl来和View建立联系，View是Window存在的实体，只能通过WindowManager来访问Window。

4.WindowManager的实现是WindowManagerImpl，其再委托WindowManagerGlobal来对Window进行操作，其中有四种List分别储存对应的View、ViewRootImpl、WindowManger.LayoutParams和正在被删除的View。

5.Window的实体是存在于远端的WindowMangerService，所以增删改Window在本端是修改上面的几个List然后过ViewRootImpl重绘View，通过WindowSession(每个对应一个)在远端修改Window。

6.Activity创建Window：Activity会在attach()中创建Wndow并设置其回调(onAttachedToWindow()、dispatchTouchEvent())，Activity的Window是由Policy类创建PhoneWindow实现的。然后通过Activity#setContentView()调用PhoneWindow的setContentView。


### 13、WMS是如何管理Window的？


### 14、大体说清一个应用程序安装到手机上时发生了什么？

APK的安装流程如下所示：

![image](https://github.com/guoxiaoxing/android-open-source-project-analysis/raw/master/art/app/package/apk_install_structure.png)

复制APK到/data/app目录下，解压并扫描安装包。

资源管理器解析APK里的资源文件。

解析AndroidManifest文件，并在/data/data/目录下创建对应的应用数据目录。

然后对dex文件进行优化，并保存在dalvik-cache目录下。

将AndroidManifest文件解析出的四大组件信息注册到PackageManagerService中。

安装完成后，发送广播。


### 15、Android的打包流程？apk里有哪些东西？签名算法的原理？

#### apk打包流程

Android的包文件APK分为两个部分：代码和资源，所以打包方面也分为资源打包和代码打包两个方面，下面就来分析资源和代码的编译打包原理。

APK整体的的打包流程如下图所示：

![image](https://github.com/guoxiaoxing/android-open-source-project-analysis/raw/master/art/native/vm/apk_package_flow.png)

具体说来：

- 通过AAPT工具进行资源文件（包括AndroidManifest.xml、布局文件、各种xml资源等）的打包，生成R.java文件。
- 通过AIDL工具处理AIDL文件，生成相应的Java文件。
- 通过Java Compiler编译R.java、Java接口文件、Java源文件，生成.class文件。
- 通过dex命令，将.class文件和第三方库中的.class文件处理生成classes.dex，该过程主要完成Java字节码转换成Dalvik字节码，压缩常量池以及清除冗余信息等工作。
- 通过ApkBuilder工具将资源文件、DEX文件打包生成APK文件。
- 通过Jarsigner工具，利用KeyStore对生成的APK文件进行签名。
- 如果是正式版的APK，还会利用ZipAlign工具进行对齐处理，对齐的过程就是将APK文件中所有的资源文件距离文件的起始距位置都偏移4字节的整数倍，这样通过内存映射访问APK文件的速度会更快，并且会减少其在设备上运行时的内存占用。

#### apk组成

- dex：最终生成的Dalvik字节码。
- res：存放资源文件的目录。
- asserts：额外建立的资源文件夹。
- lib：如果存在的话，存放的是ndk编出来的so库。   
- META-INF：存放签名信息
- MANIFEST.MF：其中每一个资源文件都有一个SHA-256-Digest签名，MANIFEST.MF文件的SHA256（SHA1）并base64编码的结果即为CERT.SF中的SHA256-Digest-Manifest值。

CERT.SF：除了开头处定义的SHA256（SHA1）-Digest-Manifest值，后面几项的值是对MANIFEST.MF文件中的每项再次SHA256并base64编码后的值。
CERT.RSA：其中包含了公钥、加密算法等信息。首先对前一步生成的MANIFEST.MF使用了SHA256（SHA1）-RSA算法，用开发者私钥签名，然后在安装时使用公钥解密。最后，将其与未加密的摘要信息（MANIFEST.MF文件）进行对比，如果相符，则表明内容没有被修改。

- androidManifest：程序的全局清单配置文件。
- resources.arsc：编译后的二进制资源文件。

#### [签名算法的原理](https://www.jianshu.com/p/286d2b372334)

##### 为什么要签名？

- 确保Apk来源的真实性。
- 确保Apk没有被第三方篡改。

##### 什么是签名？

在Apk中写入一个“指纹”。指纹写入以后，Apk中有任何修改，都会导致这个指纹无效，Android系统在安装Apk进行签名校验时就会不通过，从而保证了安全性。

##### 数字摘要

对一个任意长度的数据，通过一个Hash算法计算后，都可以得到一个固定长度的二进制数据，这个数据就称为“摘要”。

补充：
- 散列算法的基础原理：将数据（如一段文字）运算变为另一固定长度值。
- SHA-1：在密码学中，SHA-1（安全散列算法1）是一种加密散列函数，它接受输入并产生一个160 位（20 字节）散列值，称为消息摘要 。
- MD5：MD5消息摘要算法（英语：MD5 Message-Digest Algorithm），一种被广泛使用的密码散列函数，可以产生出一个128位（16字节）的散列值（hash value），用于确保信息传输完整一致。
- SHA-2：名称来自于安全散列算法2（英语：Secure Hash Algorithm 2）的缩写，一种密码散列函数算法标准，其下又可再分为六个不同的算法标准，包括了：SHA-224、SHA-256、SHA-384、SHA-512、SHA-512/224、SHA-512/256。
- 

特征：

- 唯一性
- 固定长度：比较常用的Hash算法有MD5和SHA1，MD5的长度是128拉，SHA1的长度是160位。
- 不可逆性

##### 签名和校验的主要过程

签名就是在摘要的基础上再进行一次加密，对摘要加密后的数据就可以当作数字签名。

##### 签名过程：

- 1、计算摘要：通过Hash算法提取出原始数据的摘要。
- 2、计算签名：再通过基于密钥（私钥）的非对称加密算法对提取出的摘要进行加密，加密后的数据就是签名信息。
- 3、写入签名：将签名信息写入原始数据的签名区块内。

##### 校验过程：
。，首先用同样的Hash算法从接收到的数据中提取出摘要。
- 2、解密签名：使用发送方的公钥对数字签名进行解密，解密出原始摘要。
- 3、比较摘要：如果解密后的数据和提取的摘要一致，则校验通过；如果数据被第三方篡改过，解密后的数据和摘要将会不一致，校验则不会通过。

##### 数字证书

如何保证公钥的可靠性呢？答案是数字证书，数字证书是身份认证机构（Certificate Authority）颁发的，包含了以下信息：

- 证书颁发机构
- 证书颁发机构签名
- 证书绑定的服务器域名
- 证书版本、有效期
- 签名使用的加密算法（非对称算法，如RSA）
- 公钥等

接收方收到消息后，先向CA验证证书的合法性，再进行签名校验。

注意：Apk的证书通常的自签名的，也就是由开发者自己制作，没有向CA机构申请。Android在安装Apk时并没有校验证书本身的合法性，只是从证书中提取公钥和加密算法，这也正是对第三方Apk重新签名后，还能够继续在没有安装这个Apk的系统中继续安装的原因。

##### keystore和证书格式

keystore文件中包含了私钥、公钥和数字证书。根据编码不同，keystore文件分为很多种，Android使用的是Java标准keystore格式JKS(Java Key Storage)，所以通过Android Studio导出的keystore文件是以.jks结尾的。

keystore使用的证书标准是X.509，X.509标准也有多种编码格式，常用的有两种：pem（Privacy Enhanced Mail）和der（Distinguished Encoding Rules）。jks使用的是der格式，Android也支持直接使用pem格式的证书进行签名。

两种证书编码格式的区别：

- DER（Distinguished Encoding Rules）

二进制格式，所有类型的证书和私钥都可以存储为der格式。

- PEM（Privacy Enhanced Mail）

base64编码，内容以-----BEGIN xxx----- 开头，以-----END xxx----- 结尾。

##### jarsigner和apksigner的区别

Android提供了两种对Apk的签名方式，一种是基于JAR的签名方式，另一种是基于Apk的签名方式，它们的主要区别在于使用的签名文件不一样：jarsigner使用keystore文件进行签名；apksigner除了支持使用keystore文件进行签名外，还支持直接指定pem证书文件和私钥进行签名。

##### 在签名时，除了要指定keystore文件和密码外，也要指定alias和key的密码，这是为什么呢？

keystore是一个密钥库，也就是说它可以存储多对密钥和证书，keystore的密码是用于保护keystore本身的，一对密钥和证书是通过alias来区分的。所以jarsigner是支持使用多个证书对Apk进行签名的，apksigner也同样支持。




### 16、描述清点击 Android Studio 的 build 按钮后发生了什么？


### 17、[说下安卓虚拟机和java虚拟机的原理和不同点](https://blog.csdn.net/jason0539/article/details/50440669)?（JVM、Davilk、ART三者的原理和区别） 

#### JVM 和Dalvik虚拟机的区别

JVM:.java -> javac -> .class -> jar -> .jar
    
架构: 堆和栈的架构.

DVM:.java -> javac -> .class -> dx.bat -> .dex

架构: 寄存器(cpu上的一块高速缓存)

#### Android2个虚拟机的区别（一个5.0之前，一个5.0之后）

art上应用启动快，运行快，但是耗费更多存储空间，安装时间长，总的来说ART的功效就是”空间换时间”。

- ART: Ahead of Time
- Dalvik: Just in Time

什么是Dalvik：Dalvik是Google公司自己设计用于Android平台的Java虚拟机。Dalvik虚拟机是Google等厂商合作开发的Android移动设备平台的核心组成部分之一，它可以支持已转换为.dex(即Dalvik Executable)格式的Java应用程序的运行，.dex格式是专为Dalvik应用设计的一种压缩格式，适合内存和处理器速度有限的系统。Dalvik经过优化，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个Dalvik应用作为独立的Linux进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。

什么是ART:Android操作系统已经成熟，Google的Android团队开始将注意力转向一些底层组件，其中之一是负责应用程序运行的Dalvik运行时。Google开发者已经花了两年时间开发更快执行效率更高更省电的替代ART运行时。ART代表Android Runtime,其处理应用程序执行的方式完全不同于Dalvik，Dalvik是依靠一个Just-In-Time(JIT)编译器去解释字节码。开发者编译后的应用代码需要通过一个解释器在用户的设备上运行，这一机制并不高效，但让应用能更容易在不同硬件和架构上运行。ART则完全改变了这套做法，在应用安装的时候就预编译字节码为机器语言，这一机制叫Ahead-Of-Time(AOT)编译。在移除解释代码这一过程后，应用程序执行将更有效率，启动更快。

ART优点：

- 系统性能的显著提升。
- 应用启动更快、运行更快、体验更流畅、触感反馈更及时。
- 更长的电池续航能力。
- 支持更低的硬件。

ART缺点：

- 更大的存储空间占用，可能会增加10%-20%。
- 更长的应用安装时间。


### 18、安卓采用自动垃圾回收机制，请说下安卓内存管理的原理？

### 19、Android中App是如何沙箱化的,为何要这么做？

### 20、[一个图片在app中调用R.id后是如何找到的](https://my.oschina.net/u/255456/blog/608229)？

### 21、JNI

#### Java调用C++

- 在Java中声明Native方法（即需要调用的本地方法）
- 编译上述 Java源文件javac（得到 .class文件） 3。 通过 javah 命令导出JNI的头文件（.h文件）
- 使用 Java需要交互的本地代码 实现在 Java中声明的Native方法
- 编译.so库文件
- 通过Java命令执行 Java程序，最终实现Java调用本地代码

#### C++调用Java

- 从classpath路径下搜索ClassMethod这个类，并返回该类的Class对象。
- 获取类的默认构造方法ID。
- 查找实例方法的ID。
- 创建该类的实例。
- 调用对象的实例方法。

    JNIEXPORT void JNICALL Java_com_study_jnilearn_AccessMethod_callJavaInstaceMethod  
    (JNIEnv *env, jclass cls)  
    {  
        jclass clazz = NULL;  
        jobject jobj = NULL;  
        jmethodID mid_construct = NULL;  
        jmethodID mid_instance = NULL;  
        jstring str_arg = NULL;  
        // 1、从classpath路径下搜索ClassMethod这个类，并返回该类的Class对象  
        clazz = (*env)->FindClass(env, "com/study/jnilearn/ClassMethod");  
        if (clazz == NULL) {  
            printf("找不到'com.study.jnilearn.ClassMethod'这个类");  
            return;  
        }  
    
        // 2、获取类的默认构造方法ID  
        mid_construct = (*env)->GetMethodID(env,clazz, "<init>","()V");  
        if (mid_construct == NULL) {  
            printf("找不到默认的构造方法");  
            return;  
        }  
    
        // 3、查找实例方法的ID  
        mid_instance = (*env)->GetMethodID(env, clazz, "callInstanceMethod", "(Ljava/lang/String;I)V");  
        if (mid_instance == NULL) {  
    
            return;  
        }  
    
        // 4、创建该类的实例  
        jobj = (*env)->NewObject(env,clazz,mid_construct);  
        if (jobj == NULL) {  
            printf("在com.study.jnilearn.ClassMethod类中找不到callInstanceMethod方法");  
            return;  
        }  
    
        // 5、调用对象的实例方法  
        str_arg = (*env)->NewStringUTF(env,"我是实例方法");  
        (*env)->CallVoidMethod(env,jobj,mid_instance,str_arg,200);  
    
        // 删除局部引用  
        (*env)->DeleteLocalRef(env,clazz);  
        (*env)->DeleteLocalRef(env,jobj);  
        (*env)->DeleteLocalRef(env,str_arg);  
    }  
    
    
#### 如何在jni中注册native函数，有几种注册方式？

    
### 22、请介绍一下NDK？


## 三、Android优秀三方库源码

### 1、你项目中用到哪些开源库？说说其实现原理？

#### 一、[网络底层框架：OkHttp实现原理](https://jsonchao.github.io/2018/12/01/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%B8%80%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3OKHttp%E6%BA%90%E7%A0%81%EF%BC%89/)

##### 手写拦截器？

##### 网络请求缓存处理，okhttp如何处理网络缓存的？

##### HttpUrlConnection 和 okhttp关系？

##### Volley与OkHttp的对比：

Volley：支持HTTPS。缓存、异步请求，不支持同步请求。协议类型是Http/1.0, Http/1.1，网络传输使用的是 HttpUrlConnection/HttpClient，数据读写使用的IO。
OkHttp：支持HTTPS。缓存、异步请求、同步请求。协议类型是Http/1.0, Http/1.1, SPDY, Http/2.0, WebSocket，网络传输使用的是封装的Socket，数据读写使用的NIO（Okio）。
SPDY协议类似于HTTP，但旨在缩短网页的加载时间和提高安全性。SPDY协议通过压缩、多路复用和优先级来缩短加载时间。

Okhttp的子系统层级结构图如下所示：

![image](https://github.com/guoxiaoxing/android-open-framwork-analysis/raw/master/art/okhttp/okhttp_structure.png)

网络配置层：利用Builder模式配置各种参数，例如：超时时间、拦截器等，这些参数都会由Okhttp分发给各个需要的子系统。
重定向层：负责重定向。
Header拼接层：负责把用户构造的请求转换为发送给服务器的请求，把服务器返回的响应转换为对用户友好的响应。
HTTP缓存层：负责读取缓存以及更新缓存。
连接层：连接层是一个比较复杂的层级，它实现了网络协议、内部的拦截器、安全性认证，连接与连接池等功能，但这一层还没有发起真正的连接，它只是做了连接器一些参数的处理。
数据响应层：负责从服务器读取响应的数据。
在整个Okhttp的系统中，我们还要理解以下几个关键角色：

OkHttpClient：通信的客户端，用来统一管理发起请求与解析响应。
Call：Call是一个接口，它是HTTP请求的抽象描述，具体实现类是RealCall，它由CallFactory创建。
Request：请求，封装请求的具体信息，例如：url、header等。
RequestBody：请求体，用来提交流、表单等请求信息。
Response：HTTP请求的响应，获取响应信息，例如：响应header等。
ResponseBody：HTTP请求的响应体，被读取一次以后就会关闭，所以我们重复调用responseBody.string()获取请求结果是会报错的。
Interceptor：Interceptor是请求拦截器，负责拦截并处理请求，它将网络请求、缓存、透明压缩等功能都统一起来，每个功能都是一个Interceptor，所有的Interceptor最 终连接成一个Interceptor.Chain。典型的责任链模式实现。
StreamAllocation：用来控制Connections与Streas的资源分配与释放。
RouteSelector：选择路线与自动重连。
RouteDatabase：记录连接失败的Route黑名单。

##### 自己去设计网络请求框架，怎么做？

##### 从网络加载一个10M的图片，说下注意事项？

##### http怎么知道文件过大是否传输完毕的响应？

##### 谈谈你对WebSocket的理解？

##### WebSocket与socket的区别？


#### 二、[网络封装框架：Retrofit实现原理](https://jsonchao.github.io/2018/12/09/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%BA%8C%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Retrofit%E6%BA%90%E7%A0%81%EF%BC%89/)


##### Android：主流网络请求开源库的对比（Android-Async-Http、Volley、OkHttp、Retrofit）

https://www.jianshu.com/p/050c6db5af5a


#### 三、[响应式编程框架：RxJava实现原理](https://jsonchao.github.io/2019/01/01/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%BA%94%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3RxJava%E6%BA%90%E7%A0%81%EF%BC%89/)

##### [RxJava中map和flatmap操作符的区别及底层实现](https://www.jianshu.com/p/af13a8278a05)

##### 手写rxjava遍历数组。

##### 你认为Rxjava的线程池与你们自己实现任务管理框架有什么区别？


#### 四、[图片加载框架：Glide实现原理](https://jsonchao.github.io/2018/12/16/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%B8%89%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Glide%E6%BA%90%E7%A0%81%EF%BC%89/)

##### Glide如何确定图片加载完毕？


##### Glide使用什么缓存？


##### Glide内存缓存如何控制大小？


##### 怎样计算一张图片的大小，加载bitmap过程（怎样保证不产生内存溢出），二级缓存，LRUCache算法。

    计算一张图片的大小
    
    
    图片占用内存的计算公式：图片高度 * 图片宽度 * 一个像素占用的内存大小.所以，计算图片占用内存大小的时候，要考虑图片所在的目录跟设备密度，这两个因素其实影响的是图片的高宽，android会对图片进行拉升跟压缩。
    
    
    加载bitmap过程（怎样保证不产生内存溢出）
    
    
    由于Android对图片使用内存有限制，若是加载几兆的大图片便内存溢出。Bitmap会将图片的所有像素（即长x宽）加载到内存中，如果图片分辨率过大，会直接导致内存OOM，只有在BitmapFactory加载图片时使用BitmapFactory.Options对相关参数进行配置来减少加载的像素。
    
    
    BitmapFactory.Options相关参数详解
    
    
    (1).Options.inPreferredConfig值来降低内存消耗。
    
    比如：默认值ARGB_8888改为RGB_565,节约一半内存。
    
    
    (2).设置Options.inSampleSize 缩放比例，对大图片进行压缩 。
    
    
    (3).设置Options.inPurgeable和inInputShareable：让系统能及时回 收内存。
    
    A：inPurgeable：设置为True时，表示系统内存不足时可以被回 收，设置为False时，表示不能被回收。
    
    B：inInputShareable：设置是否深拷贝，与inPurgeable结合使用，inPurgeable为false时，该参数无意义。
    
    
    (4).使用decodeStream代替其他方法。
    
    decodeResource,setImageResource,setImageBitmap等方法
    


### Android中软引用与弱引用的应用场景。

Java 引用类型分类：

![image](https://user-gold-cdn.xitu.io/2017/10/10/29c884389e96babb2759b95014628aae?imageslim)

    在 Android 应用的开发中，为了防止内存溢出，在处理一些占用内存大而且声明周期较长的对象时候，可以尽量应用软引用和弱引用技术。
    软/弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收器回收，Java 虚拟机就会把这个软引用加入到与之关联的引用队列中。利用这个队列可以得知被回收的软/弱引用的对象列表，从而为缓冲器清除已失效的软 / 弱引用。
    如果只是想避免 OOM 异常的发生，则可以使用软引用。如果对于应用的性能更在意，想尽快回收一些占用内存比较大的对象，则可以使用弱引用。
    可以根据对象是否经常使用来判断选择软引用还是弱引用。如果该对象可能会经常使用的，就尽量用软引用。如果该对象不被使用的可能性更大些，就可以用弱引用。

##### Android里的内存缓存和磁盘缓存是怎么实现的。

内存缓存基于LruCache实现，磁盘缓存基于DiskLruCache实现。这两个类都基于Lru算法和LinkedHashMap来实现。

LRU算法可以用一句话来描述，如下所示：

    LRU是Least Recently Used的缩写，最近最久未使用算法，从它的名字就可以看出，它的核心原则是如果一个数据在最近一段时间没有使用到，那么它在将来被 访问到的可能性也很小，则这类数据项会被优先淘汰掉。

LruCache的原理是利用LinkedHashMap持有对象的强引用，按照Lru算法进行对象淘汰。具体说来假设我们从表尾访问数据，在表头删除数据，当访问的数据项在链表中存在时，则将该数据项移动到表尾，否则在表尾新建一个数据项。当链表容量超过一定阈值，则移除表头的数据。

为什么会选择LinkedHashMap呢？

这跟LinkedHashMap的特性有关，LinkedHashMap的构造函数里有个布尔参数accessOrder，当它为true时，LinkedHashMap会以访问顺序为序排列元素，否则以插入顺序为序排序元素。

DiskLruCache与LruCache原理相似，只是多了一个journal文件来做磁盘文件的管理和迎神，如下所示：

    libcore.io.DiskLruCache
    1
    1
    1
    
    DIRTY 1517126350519
    CLEAN 1517126350519 5325928
    REMOVE 1517126350519
注：这里的缓存目录是应用的缓存目录/data/data/pckagename/cache，未root的手机可以通过以下命令进入到该目录中或者将该目录整体拷贝出来：

    //进入/data/data/pckagename/cache目录
    adb shell
    run-as com.your.packagename 
    cp /data/data/com.your.packagename/
    
    //将/data/data/pckagename目录拷贝出来
    adb backup -noapk com.your.packagename
我们来分析下这个文件的内容：

第一行：libcore.io.DiskLruCache，固定字符串。
第二行：1，DiskLruCache源码版本号。
第三行：1，App的版本号，通过open()方法传入进去的。
第四行：1，每个key对应几个文件，一般为1.
第五行：空行
第六行及后续行：缓存操作记录。
第六行及后续行表示缓存操作记录，关于操作记录，我们需要了解以下三点：

DIRTY 表示一个entry正在被写入。写入分两种情况，如果成功会紧接着写入一行CLEAN的记录；如果失败，会增加一行REMOVE记录。注意单独只有DIRTY状态的记录是非法的。
当手动调用remove(key)方法的时候也会写入一条REMOVE记录。
READ就是说明有一次读取的记录。
CLEAN的后面还记录了文件的长度，注意可能会一个key对应多个文件，那么就会有多个数字。

##### LRUCache算法是怎样实现的。

    内部存在一个LinkedHashMap和maxSize，把最近使用的对象用强引用存储在 LinkedHashMap中，给出来put和get方法，每次put图片时计算缓存中所有图片总大小，跟maxSize进行比较，大于maxSize，就将最久添加的图片移除；反之小于maxSize就添加进来。
    
    
    之前，我们会使用内存缓存技术实现，也就是软引用或弱引用，在Android 2.3（APILevel 9）开始，垃圾回收器会更倾向于回收持有软引用或弱引用的对象，这让软引用和弱引用变得不再可靠。

##### Bitmap 压缩策略

    加载 Bitmap 的方式：
    BitmapFactory 四类方法：
    decodeFile( 文件系统 )
    decodeResourece( 资源 )
    decodeStream( 输入流 ) 
    decodeByteArray( 字节数 )
    BitmapFactory.options 参数
    inSampleSize 采样率，对图片高和宽进行缩放，以最小比进行缩放（一般取值为 2 的指数）。通常是根据图片宽高实际的大小/需要的宽高大小，分别计算出宽和高的缩放比。但应该取其中最小的缩放比，避免缩放图片太小，到达指定控件中不能铺满，需要拉伸从而导致模糊。
    inJustDecodeBounds 获取图片的宽高信息，交给  inSampleSize 参数选择缩放比。通过 inJustDecodeBounds = true，然后加载图片就可以实现只解析图片的宽高信息，并不会真正的加载图片，所以这个操作是轻量级的。当获取了宽高信息，计算出缩放比后，然后在将 inJustDecodeBounds = false,再重新加载图片，就可以加载缩放后的图片。
    高效加载 Bitmap 的流程
    将 BitmapFactory.Options 的 inJustDecodeBounds 参数设为 true 并加载图片
    从 BitmapFactory.Options 中取出图片原始的宽高信息，对应于 outWidth 和 outHeight 参数
    根据采样率规则并结合目标 view 的大小计算出采样率 inSampleSize
    将 BitmapFactory.Options 的 inJustDecodeBounds 设置为 false 重新加载图片

##### Bitmap的处理：

当使用ImageView的时候，可能图片的像素大于ImageView，此时就可以通过BitmapFactory.Option来对图片进行压缩，inSampleSize表示缩小2^(inSampleSize-1)倍。

BitMap的缓存：

1.使用LruCache进行内存缓存。

2.使用DiskLruCache进行硬盘缓存。

3.实现一个ImageLoader的流程：同步异步加载、图片压缩、内存硬盘缓存、网络拉取

    1.同步加载只创建一个线程然后按照顺序进行图片加载
    2.异步加载使用线程池，让存在的加载任务都处于不同线程
    3.为了不开启过多的异步任务，只在列表静止的时候开启图片加载
    
##### Bitmap在decode的时候申请的内存如何复用，释放时机
    
##### 图片库对比

http://stackoverflow.com/questions/29363321/picasso-v-s-imageloader-v-s-fresco-vs-glide

http://www.trinea.cn/android/android-image-cache-compare/

##### Fresco与Glide的对比：

Glide：相对轻量级，用法简单优雅，支持Gif动态图，适合用在那些对图片依赖不大的App中。
Fresco：采用匿名共享内存来保存图片，也就是Native堆，有效的的避免了OOM，功能强大，但是库体积过大，适合用在对图片依赖比较大的App中。

Fresco的整体架构如下图所示：

![image](https://github.com/guoxiaoxing/android-open-framwork-analysis/raw/master/art/fresco/fresco_structure.png)

DraweeView：继承于ImageView，只是简单的读取xml文件的一些属性值和做一些初始化的工作，图层管理交由Hierarchy负责，图层数据获取交由负责。
DraweeHierarchy：由多层Drawable组成，每层Drawable提供某种功能（例如：缩放、圆角）。
DraweeController：控制数据的获取与图片加载，向pipeline发出请求，并接收相应事件，并根据不同事件控制Hierarchy，从DraweeView接收用户的事件，然后执行取消网络请求、回收资源等操作。
DraweeHolder：统筹管理Hierarchy与DraweeHolder。
ImagePipeline：Fresco的核心模块，用来以各种方式（内存、磁盘、网络等）获取图像。
Producer/Consumer：Producer也有很多种，它用来完成网络数据获取，缓存数据获取、图片解码等多种工作，它产生的结果由Consumer进行消费。
IO/Data：这一层便是数据层了，负责实现内存缓存、磁盘缓存、网络缓存和其他IO相关的功能。
纵观整个Fresco的架构，DraweeView是门面，和用户进行交互，DraweeHierarchy是视图层级，管理图层，DraweeController是控制器，管理数据。它们构成了整个Fresco框架的三驾马车。当然还有我们 幕后英雄Producer，所有的脏活累活都是它干的，最佳劳模👍

理解了Fresco整体的架构，我们还有了解在这套矿建里发挥重要作用的几个关键角色，如下所示：

Supplier：提供一种特定类型的对象，Fresco里有很多以Supplier结尾的类都实现了这个接口。
SimpleDraweeView：这个我们就很熟悉了，它接收一个URL，然后调用Controller去加载图片。该类继承于GenericDraweeView，GenericDraweeView又继承于DraweeView，DraweeView是Fresco的顶层View类。
PipelineDraweeController：负责图片数据的获取与加载，它继承于AbstractDraweeController，由PipelineDraweeControllerBuilder构建而来。AbstractDraweeController实现了DraweeController接口，DraweeController 是Fresco的数据大管家，所以的图片数据的处理都是由它来完成的。
GenericDraweeHierarchy：负责SimpleDraweeView上的图层管理，由多层Drawable组成，每层Drawable提供某种功能（例如：缩放、圆角），该类由GenericDraweeHierarchyBuilder进行构建，该构建器 将placeholderImage、retryImage、failureImage、progressBarImage、background、overlays与pressedStateOverlay等 xml文件或者Java代码里设置的属性信息都传入GenericDraweeHierarchy中，由GenericDraweeHierarchy进行处理。
DraweeHolder：该类是一个Holder类，和SimpleDraweeView关联在一起，DraweeView是通过DraweeHolder来统一管理的。而DraweeHolder又是用来统一管理相关的Hierarchy与Controller
DataSource：类似于Java里的Futures，代表数据的来源，和Futures不同，它可以有多个result。
DataSubscriber：接收DataSource返回的结果。
ImagePipeline：用来调取获取图片的接口。
Producer：加载与处理图片，它有多种实现，例如：NetworkFetcherProducer，LocalAssetFetcherProducer，LocalFileFetchProducer。从这些类的名字我们就可以知道它们是干什么的。 Producer由ProducerFactory这个工厂类构建的，而且所有的Producer都是像Java的IO流那样，可以一层嵌套一层，最终只得到一个结果，这是一个很精巧的设计👍
Consumer：用来接收Producer产生的结果，它与Producer组成了生产者与消费者模式。
注：Fresco源码里的类的名字都比较长，但是都是按照一定的命令规律来的，例如：以Supplier结尾的类都实现了Supplier接口，它可以提供某一个类型的对象（factory, generator, builder, closure等）。 以Builder结尾的当然就是以构造者模式创建对象的类。

##### Bitmap如何处理大图，如一张30M的大图，如何预防OOM?

http://blog.csdn.net/guolin_blog/article/details/9316683

https://blog.csdn.net/lmj623565791/article/details/493009890

使用BitmapRegionDecoder动态加载图片的显示区域。


##### Bitmap对象的理解

##### 自己去实现图片库，怎么做？（对扩展开发，对修改封闭，同时又保持独立性，参考Android源码设计模式解析实战的图片加载库案例即可）

##### 写个图片浏览器，说出你的思路？


#### 五、[事件总线框架：EventBus实现原理](https://jsonchao.github.io/2019/01/28/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%B9%9D%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3EventBus%E6%BA%90%E7%A0%81%EF%BC%89/)


#### 六、[内存泄漏检测框架：LeakCanary实现原理](https://jsonchao.github.io/2019/01/06/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E5%85%AD%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Leakcanary%E6%BA%90%E7%A0%81%EF%BC%89/)


#### 七、[依赖注入框架：ButterKnife实现原理](https://jsonchao.github.io/2019/01/13/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%B8%83%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ButterKnife%E6%BA%90%E7%A0%81%EF%BC%89/)

ButterKnife对性能的影响很小，因为没有使用使用反射，而是使用的Annotation Processing Tool(APT)，注解处理器，javac中用于编译时扫描和解析Java注解的工具。在编译阶段执行的，它的原理就是读入Java源代码，解析注解，然后生成新的Java代码。新生成的Java代码最后被编译成Java字节码，注解解析器不能改变读入的Java类，比如不能加入或删除Java方法。

#### AOP IOC 的好处以及在 Android 开发中的应用



#### 八、[依赖全局管理框架：Dagger2实现原理](https://jsonchao.github.io/2019/01/20/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E5%85%AB%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Dagger2%E6%BA%90%E7%A0%81%EF%BC%89/)


#### 九、[数据库框架：GreenDao实现原理](https://jsonchao.github.io/2018/12/22/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E5%9B%9B%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3GreenDao%E6%BA%90%E7%A0%81%EF%BC%89/)

##### 数据库框架对比？

##### 数据库的优化

##### 数据库数据迁移问题


## 四、热修复、插件化、模块化、组件化、Gradle

### 1、热修复和插件化

#### 热修补技术是怎样实现的，和插件化有什么区别？

http://www.liuguangli.win/archives/366

http://www.liuguangli.win/archives/387

http://www.liuguangli.win/archives/452


插件化：动态加载主要解决3个技术问题：

- 1、使用ClassLoader加载类。
- 2、资源访问。
- 3、生命周期管理。

插件化是体现在功能拆分方面的，它将某个功能独立提取出来，独立开发，独立测试，再插入到主应用中。依次来较少主应用的规模。

热修复：

原因：因为一个dvm中存储方法id用的是short类型，导致dex中方法不能超过65536个。

原理：将编译好的class文件拆分打包成两个dex，绕过dex方法数量的限制以及安装时的检查，在运行时再动态加载第二个dex文件中。使用Dexclassloader：热修复是体现在bug修复方面的，它实现的是不需要重新发版和重新安装，就可以去修复已知的bug。利用PathClassLoader和DexClassLoader去加载与bug类同名的类，替换掉bug类，进而达到修复bug的目的，原理是在app打包的时候阻止类打上CLASS_ISPREVERIFIED标志，然后在热修复的时候动态改变BaseDexClassLoader对象间接引用的dexElements，替换掉旧的类。

目前热修复框架主要分为两大类：

- Sophix：修改方法指针。
- Tinker：修改dex数组元素。

相同点:
    
都使用ClassLoader来实现加载的新的功能类，都可以使用PathClassLoader与DexClassLoader。
    
不同点：
    
热修复因为是为了修复Bug的，所以要将新的同名类替代同名的Bug类，要抢先加载新的类而不是Bug类，所以多做两件事：在原先的app打包的时候，阻止相关类去打上CLASS_ISPREVERIFIED标志，还有在热修复时动态改变BaseDexClassLoader对象间接引用的dexElements，这样才能抢先代替Bug类，完成系统不加载旧的Bug类.
    
而插件化只是增加新的功能类或者是资源文件，所以不涉及抢先加载旧的类这样的使命，就避过了阻止相关类去打上CLASS_ISPREVERIFIED标志和还有在热修复时动态改变BaseDexClassLoader对象间接引用的dexElements.
    
所以插件化比热修复简单，热修复是在插件化的基础上在进行替旧的Bug类
    

#### 为什么选用插件化，插件化框架的比较，梳理插件化的架构？


#### 插件化原理分析


### 2、模块化和组件化


#### 模块化的好处

https://www.jianshu.com/p/376ea8a19a17


#### 组件化原理，组件化中路由（ARouter）的实现



#### Android 组件化的原理，还有一些组件化平时使用的问题。


#### 项目组件化的理解


#### 多模块开发的时候不同的负责人可能会引入重复资源，相同的字符串，相同的icon等但是文件名并不一样，怎样去重？



### 3、gradle

#### gradle熟悉么，自动打包知道么？

#### 如何加快 Gradle 的编译速度？

#### Gradle的Flavor能否配置sourceset？

#### Gradle生命周期


## 五、设计模式与架构设计


### 1、设计模式


#### 谈谈你对Android设计模式的理解


#### 项目中常用的设计模式


#### 手写生产者/消费者模式


#### 适配器模式，装饰者模式，外观模式的异同？


### 2、架构设计

#### [MVC MVP MVVM原理和区别？](http://www.tianmaying.com/tutorial/AndroidMVC)

##### 架构设计的目的

通过设计是模块程序化，从而做到高内聚低耦合，让开发者能更专注于功能实现本身，提供程序开发效率、更容易进行测试、维护和定位问题等等。而且，不同的规模的项目应该选用不同的架构设计。

##### MVC

MVC是模型(model)－视图(view)－控制器(controller)的缩写，其中M层处理数据，业务逻辑等；V层处理界面的显示结果；C层起到桥梁的作用，来控制V层和M层通信以此来达到分离视图显示和业务逻辑层。在Android中的MVC划分是这样的：

- 视图层(View)：一般采用XML文件进行界面的描述，也可以在界面中使用动态布局的方式。
- 控制层(Controller)：由Activity承担。
- 模型层(Model)：数据库的操作、对网络等的操作，复杂业务计算等等。

##### MVC缺点

在Android开发中，Activity并不是一个标准的MVC模式中的Controller，它的首要职责是加载应用的布局和初始化用户界面，并接受和处理来自用户的操作请求，进而作出响应。随着界面及其逻辑的复杂度不断提升，Activity类的职责不断增加，以致变得庞大臃肿。

##### MVP

MVP框架由3部分组成：View负责显示，Presenter负责逻辑处理，Model提供数据。

- View:负责绘制UI元素、与用户进行交互(在Android中体现为Activity)。
- Model:负责存储、检索、操纵数据(有时也实现一个Model interface用来降低耦合)。
- Presenter:作为View与Model交互的中间纽带，处理与用户交互的逻辑。
- View interface:需要View实现的接口，View通过View interface与Presenter进行交互，降低耦合，方便使用MOCK对Presenter进行单元测试。

MVP的Presenter是框架的控制者，承担了大量的逻辑操作，而MVC的Controller更多时候承担一种转发的作用。因此在App中引入MVP的原因，是为了将此前在Activty中包含的大量逻辑操作放到控制层中，避免Activity的臃肿。

##### MVP与MVC的主要区别:

- 1、（最主要区别）View与Model并不直接交互，而是通过与Presenter交互来与Model间接交互。而在MVC中View可以与Model直接交互。
- 2、Presenter与View的交互是通过接口来进行的，更有利于添加单元测试。

##### MVP的优点

- 1、模型与视图完全分离，我们可以修改视图而不影响模型。
- 2、可以更高效地使用模型，因为所有的交互都发生在一个地方——Presenter内部。
- 3、我们可以将一个Presenter用于多个视图，而不需要改变Presenter的逻辑。这个特性非常的有用，因为视图的变化总是比模型的变化频繁。
- 4、如果我们把逻辑放在Presenter中，那么我们就可以脱离用户接口来测试这些逻辑（单元测试）。

UI层一般包括Activity，Fragment，Adapter等直接和UI相关的类，UI层的Activity在启动之后实例化相应的Presenter，App的控制权后移，由UI转移到Presenter，两者之间的通信通过BroadCast、Handler、事件总线机制或者接口完成，只传递事件和结果。

MVP的执行流程：首先V层通知P层用户发起了一个网络请求，P层会决定使用负责网络相关的M层去发起请求网络，最后，P层将完成的结果更新到V层。

##### MVP的变种：Passive View

View直接依赖Presenter，但是Presenter间接依赖View，它直接依赖的是View实现的接口。相对于View的被动，那Presenter就是主动的一方。对于Presenter的主动，有如下的理解：

- Presenter是整个MVP体系的控制中心，而不是单纯的处理View请求的人。
- View仅仅是用户交互请求的汇报者，对于响应用户交互相关的逻辑和流程，View不参与决策，真正的决策者是Presenter。
- View向Presenter发送用户交互请求应该采用这样的口吻：“我现在将用户交互请求发送给你，你看着办，需要我的时候我会协助你”。
- 对于绑定到View上的数据，不应该是View从Presenter上“拉”回来的，应该是Presenter主动“推”给View的。（这里借鉴了IOC做法）
- View尽可能不维护数据状态，因为其本身仅仅实现单纯的、独立的UI操作；Presenter才是整个体系的协调者，它根据处理用于交互的逻辑给View和Model安排工作。

##### MVP架构存在的问题与解决办法

- 1、加入模板方法

将逻辑操作从V层转移到P层后，可能有一些Activity还是比较膨胀，此时，可以通过继承BaseActivity的方式加入模板方法。注意，最好不要超过3层继承。

- 2、Model内部分层

模型层（Model）中的整体代码量是最大的，此时可以进行模块的划分和接口隔离。

- 3、使用中介者和代理

在UI层和Presenter之间设置中介者Mediator，将例如数据校验、组装在内的轻量级逻辑操作放在Mediator中；在Presenter和Model之间使用代理Proxy；通过上述两者分担一部分Presenter的逻辑操作，但整体框架的控制权还是在Presenter手中。

##### MVVM 

MVVM可以算是MVP的升级版，其中的VM是ViewModel的缩写，ViewModel可以理解成是View的数据模型和Presenter的合体，ViewModel和View之间的交互通过Data Binding完成，而Data Binding可以实现双向的交互，这就使得视图和控制层之间的耦合程度进一步降低，关注点分离更为彻底，同时减轻了Activity的压力。

##### MVC->MVP->MVVM演进过程

MVC -> MVP -> MVVM 这几个软件设计模式是一步步演化发展的，MVVM 是从 MVP 的进一步发展与规范，MVP 隔离了MVC中的 M 与 V 的直接联系后，靠 Presenter 来中转，所以使用 MVP 时 P 是直接调用 View 的接口来实现对视图的操作的，这个 View 接口的东西一般来说是 showData、showLoading等等。M 与 V已经隔离了，方便测试了，但代码还不够优雅简洁，所以 MVVM 就弥补了这些缺陷。在 MVVM 中就出现的 Data Binding 这个概念，意思就是 View 接口的 showData 这些实现方法可以不写了，通过 Binding 来实现。

##### 三种模式的相同点

M层和V层的实现是一样的。

##### 三种模式的不同点

三者的差异在于如何粘合View和Model，实现用户的交互操作以及变更通知。

- Controller：接收View的命令，对Model进行操作，一个Controller可以对应多个View。
- Presenter：Presenter与Controller一样，接收View的命令，对Model进行操作；与Controller不同的是Presenter会反作用于View，Model的变更通知首先被Presenter获得，然后Presenter再去更新View。通常一个Presenter只对应于一个View。据Presenter和View对逻辑代码分担的程度不同，这种模式又有两种情况：普通的MVP模式和Passive View模式。
- ViewModel：注意这里的“Model”指的是View的Model，跟MVVM中的一个Model不是一回事。所谓View的Model就是包含View的一些数据属性和操作的这么一个东东，这种模式的关键技术就是数据绑定（data binding），View的变化会直接影响ViewModel，ViewModel的变化或者内容也会直接体现在View上。这种模式实际上是框架替应用开发者做了一些工作，开发者只需要较少的代码就能实现比较复杂的交互。

##### 补充：基于AOP的架构设计

AOP(Aspect-Oriented Programming, 面向切面编程)，诞生于上个世纪90年代，是对OOP(Object-Oriented Programming, 面向对象编程)的补充和完善。OOP引入封装、继承和多态性等概念来建立一种从上道下的对象层次结构，用以模拟公共行为的一个集合。当我们需要为分散的对象引入公共行为的时候，即定义从左到右的关系时，OOP则显得无能为力。例如日志功能。日志代码往往水平地散布在所有对象层次中，而与它所散布到的对象的核心功能毫无关系。对于其他类型的代码，如安全性、异常处理和透明的持续性也是如此。这种散布在各处的无关的代码被称为横切（Cross-Cutting）代码，在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。而AOP技术则恰恰相反，它利用一种称为“横切”的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其名为“Aspect”，即方面。所谓“方面”，简单地说，就是将那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性。

在Android App中的横切关注点有Http, SharedPreferences, Log, Json, Xml, File, Device, System, 格式转换等。Android App的需求差别很大，不同的需求横切关注点必然是不一样的。一般的App工程中应该有一个Util Package来存放相关的切面操作，在项目多了之后可以将其中使用较多的Util封装为一个Jar包/aar文件/远程依赖的方式供工程调用。

在使用MVP和AOP对App进行纵向和横向的切割之后，能够使得App整体的结构更清晰合理，避免局部的代码臃肿，方便开发、测试以及后续的维护。这样纵，横两次对于App代码的分割已经能使得程序不会过多堆积在一个Java文件里，但靠一次开发过程就写出高质量的代码是很困难的，趁着项目的间歇期，对代码进行重构很有必要。

##### 最后的建议

如果“从零开始”，用什么设计架构的问题属于想得太多做得太少的问题。
从零开始意味着一个项目的主要技术难点是基本功能实现。当每一个功能都需要考虑如何做到的时候，我觉得一般人都没办法考虑如何做好。
因为，所有的优化都是站在最上层进行统筹规划。在这之前，你必须对下层的每一个模块都非常熟悉，进而提炼可复用的代码、规划逻辑流程。


#### MVC的情况下怎么把Activity的C和V抽离？


#### MVP 架构中 Presenter 定义为接口有什么好处；


#### MVP如何管理Presenter的生命周期，何时取消网络请求？


#### aop思想


#### Fragment如果在Adapter中使用应该如何解耦？


#### 项目框架里有没有Base类，BaseActivity和BaseFragment这种封装导致的问题，以及解决方法？


#### 设计一个音乐播放界面，你会如何实现，用到那些类，如何设计，如何定义接口，如何与后台交互，如何缓存与下载，如何优化(15分钟时间)


#### 从0设计一款App整体架构，如何去做？


#### 说一款你认为当前比较火的应用并设计(比如：直播APP，P2P金融，小视频等)


#### 实现一个库，完成日志的实时上报和延迟上报两种功能，该从哪些方面考虑？



## 六、其它高频面试题

### 1、[如何保证一个后台服务不被杀死？（相同问题：如何保证service在后台不被kill？）比较省电的方式是什么？](https://www.jianshu.com/p/b5371df6d7cb)

#### 保活方案

1、AIDL方式单进程、双进程方式保活Service。（基于onStartCommand() return START_STICKY）

START_STICKY 在运行onStartCommand后service进程被kill后，那将保留在开始状态，但是不保留那些传入的intent。不久后service就会再次尝试重新创建，因为保留在开始状态，在创建 service后将保证调用onstartCommand。如果没有传递任何开始命令给service，那将获取到null的intent。

除了华为此方案无效以及未更改底层的厂商不起作用外（START_STICKY字段就可以保持Service不被杀）。此方案可以与其他方案混合使用

2、降低oom_adj的值（提升service进程优先级）：

Android中的进程是托管的，当系统进程空间紧张的时候，会依照优先级自动进行进程的回收。Android将进程分为6个等级,它们按优先级顺序由高到低依次是:

- 1.前台进程 (Foreground process)
- 2.可见进程 (Visible process)
- 3.服务进程 (Service process)
- 4.后台进程 (Background process)
- 5.空进程 (Empty process)

当service运行在低内存的环境时，将会kill掉一些存在的进程。因此进程的优先级将会很重要，可以使用startForeground 将service放到前台状态。这样在低内存时被kill的几率会低一些。

- 常驻通知栏（可通过启动另外一个服务关闭Notification，不对oom_adj值有影响）。

- 使用”1像素“的Activity覆盖在getWindow()的view上。

此方案无效果

- 循环播放无声音频（黑科技，7.0下杀不掉）。

成功对华为手机保活。小米8下也成功突破20分钟

- 3、监听锁屏广播：使Activity始终保持前台。
- 4、使用自定义锁屏界面：覆盖了系统锁屏界面。
- 5、通过android:process属性来为Service创建一个进程。
- 6、跳转到系统白名单界面让用户自己添加app进入白名单。

#### 复活方案

1、onDestroy方法里重启service

service + broadcast 方式，就是当service走onDestory的时候，发送一个自定义的广播，当收到广播的时候，重新启动service。

2、JobScheduler：原理类似定时器，5.0,5.1,6.0作用很大，7.0时候有一定影响（可以在电源管理中给APP授权）。

只对5.0，5.1、6.0起作用。

3、推送互相唤醒复活：极光、友盟、以及各大厂商的推送。

4、同派系APP广播互相唤醒：比如今日头条系、阿里系。

此外还可以监听系统广播判断Service状态，通过系统的一些广播，比如：手机重启、界面唤醒、应用状态改变等等监听并捕获到，然后判断我们的Service是否还存活。

#### 结论：高版本情况下可以使用弹出通知栏、双进程、无声音乐提高后台服务的保活概率。


### 2、Android动画框架实现原理。

Animation 框架定义了透明度，旋转，缩放和位移几种常见的动画，而且控制的是整个View。实现原理：

每次绘制视图时，View 所在的 ViewGroup 中的 drawChild 函数获取该View 的 Animation 的 Transformation 值，然后调用canvas.concat(transformToApply.getMatrix())，通过矩阵运算完成动画帧，如果动画没有完成，继续调用 invalidate() 函数，启动下次绘制来驱动动画，动画过程中的帧之间间隙时间是绘制函数所消耗的时间，可能会导致动画消耗比较多的CPU资源，最重要的是，动画改变的只是显示，并不能响应事件。


### 3、Activity-Window-View三者的差别？

 Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图） LayoutInflater像剪刀，Xml配置像窗花图纸。
    
在Activity中调用attach，创建了一个Window，
创建的window是其子类PhoneWindow，在attach中创建PhoneWindow。
在Activity中调用setContentView(R.layout.xxx)，
其中实际上是调用的getWindow().setContentView()，
内部调用了PhoneWindow中的setContentView方法。

创建ParentView：

作为ViewGroup的子类，实际是创建的DecorView(作为FramLayout的子类），
将指定的R.layout.xxx进行填充，
通过布局填充器进行填充【其中的parent指的就是DecorView】，
调用ViewGroup的removeAllView()，先将所有的view移除掉，添加新的view：addView()。
    
[深入解析](https://mp.weixin.qq.com/s/oFVBrIAUwD0wnlSfm-95bQ)


### 4、低版本SDK如何实现高版本api？

- 1、在使用了高版本API的方法前面加一个 @TargetApi(API号)。
- 2、在代码上用版本判断来控制不同版本使用不同的代码。


### 5、说说你对Context的理解？


### 6、Android的生命周期和启动模式



#### 由A启动B Activity，A为栈内复用模式，B为标准模式，然后再次启动A或者杀死B，说说A，B的生命周期变化，为什么？

#### Activity的启动模式有哪些？栈里是A-B-C，先想直接到A，BC都清理掉，有几种方法可以做到？这几种方法产生的结果是有几个A的实例？


### 7、ListView和RecyclerView系列

#### RecyclerView和ListView有什么区别？局部刷新？前者使用时多重type场景下怎么避免滑动卡顿。懒加载怎么实现，怎么优化滑动体验。

#### [RecyclerView与ListView缓存机制的不同](https://segmentfault.com/a/1190000007331249)

#### 想改变listview的高度，怎么做？

#### listview跟recyclerview上拉加载的时候分别应该如何处理？

#### 如何自己实现RecyclerView的侧滑删除？

#### RecyclerView的ItemTouchHelper的实现原理



### 8、如何实现一个推送，消息推送原理？推送到达率的问题？


### 9、动态权限系列。

#### 动态权限适配方案，权限组的概念

#### Runtime permission，如何把一个预置的app默认给它权限？不要授权。


### 10、自定义View系列。

#### Canvas的底层机制，绘制框架，硬件加速是什么原理，canvas lock的缓冲区是怎么回事？

#### 双指缩放拖动大图

#### TabLayout中如何让当前标签永远位于屏幕中间

#### TabLayout如何设置指示器的宽度包裹内容？

#### 自定义View如何考虑机型适配


### 11、对谷歌新推出的Room架构。


### 12、没有给权限如何定位，特定机型定位失败，如何解决?


### 13、Debug跟Release的APK的区别？


### 14、android文件存储，各版本存储位置的权限控制的演进，外部存储，内部存储


### 15、[有什么提高编译速度的方法？](https://juejin.im/post/5947b55eda2f600067894275)


### 16、Scroller原理。

Scroller执行流程里面的三个核心方法
    
    mScroller.startScroll()；
    mScroller.computeScrollOffset()；
    view.computeScroll()；
    
1、在mScroller.startScroll()中为滑动做了一些初始化准备，比如：起始坐标，滑动的距离和方向以及持续时间(有默认值)，动画开始时间等。
    
2、mScroller.computeScrollOffset()方法主要是根据当前已经消逝的时间来计算当前的坐标点。因为在mScroller.startScroll()中设置了动画时间，那么在computeScrollOffset()方法中依据已经消逝的时间就很容易得到当前时刻应该所处的位置并将其保存在变量mCurrX和mCurrY中。除此之外该方法还可判断动画是否已经结束。


### 17、Hybrid系列。

#### webwiew了解？怎么实现和javascript的通信？相互双方的通信。@JavascriptInterface在？版本有bug，除了这个还有其他调用android方法的方案吗？

##### Android中Java和JavaScript交互

    webView.addJavaScriptInterface(new Object(){xxx}, "xxx");
    1
    答案：可以使用WebView控件执行JavaScript脚本，并且可以在JavaScript中执行Java代码。要想让WebView控件执行JavaScript，需要调用WebSettings.setJavaScriptEnabled方法，代码如下：
    
    WebView webView = (WebView)findViewById(R.id.webview);
    WebSettings webSettings = webView.getSettings();
    //设置WebView支持JavaScript
    webSettings.setJavaScriptEnabled(true);
    webView.setWebChromeClient(new WebChromeClient());
    
    JavaScript调用Java方法需要使用WebView.addJavascriptInterface方法设置JavaScript调用的Java方法，代码如下：
    
    webView.addJavascriptInterface(new Object()
    {
        //JavaScript调用的方法
        public String process(String value)
        {
            //处理代码
            return result;
        }
    }, "demo");       //demo是Java对象映射到JavaScript中的对象名
    
    可以使用下面的JavaScript代码调用process方法，代码如下：
    
    <script language="javascript">
        function search()
        {
            //调用searchWord方法
            result.innerHTML = "<font color='red'>" + window.demo.process('data') + "</font>";
        }


### 18、如果在当前线程内使用Handler postdelayed 两个消息，一个延迟5s，一个延迟10s，然后使当前线程sleep 5秒，以上消息的执行时间会如何变化？

答：照常执行

扩展：sleep时间<=5 对两个消息无影响，5< sleep时间 <=10 对第一个消息有影响，第一个消息会延迟到sleep后执行，sleep时间>10 对两个时间都有影响，都会延迟到sleep后执行。


### 19、Android中进程内存的分配，能不能自己分配定额内存？


### 20、下拉状态栏是不是影响activity的生命周期，如果在onStop的时候做了网络请求，onResume的时候怎么恢复


### 21、Android长连接，怎么处理心跳机制。

长连接：长连接是建立连接之后, 不主动断开. 双方互相发送数据, 发完了也不主动断开连接, 之后有需要发送的数据就继续通过这个连接发送.

心跳包：其实主要是为了防止NAT超时，客户端隔一段时间就主动发一个数据，探测连接是否断开。

服务器处理心跳包：假如客户端心跳间隔是固定的, 那么服务器在连接闲置超过这个时间还没收到心跳时, 可以认为对方掉线, 关闭连接. 如果客户端心跳会动态改变, 应当设置一个最大值, 超过这个最大值才认为对方掉线. 还有一种情况就是服务器通过TCP连接主动给客户端发消息出现写超时, 可以直接认为对方掉线.


### 22、CrashHandler实现原理？

    获取app crash的信息保存在本地然后在下一次打开app的时候发送到服务器。
   
   
### 23、SurfaceView和View的最本质的区别？

SurfaceView是在一个新起的单独线程中可以重新绘制画面，而view必须在UI的主线程中更新画面。
    
在UI的主线程中更新画面可能会引发问题，比如你更新的时间过长，那么你的主UI线程就会被你正在画的函数阻塞。那么将无法响应按键、触屏等消息。当使用SurfaceView由于是在新的线程中更新画面所以不会阻塞你的UI主线程。但这也带来了另外一个问题，就是事件同步。比如你触屏了一下，你需要在SurfaceView中的thread处理，一般就需要有一个event queue的设计来保存touchevent，这会稍稍复杂一点，因为涉及到线程安全。


### 24、Android程序运行时权限与文件系统权限

1、Linux 文件系统权限。不同的用户对文件有不同的读写执行权限。在android系统中，system和应用程序是分开的，system里的数据是不可更改的。

2、Android中有3种权限，进程权限UserID，签名，应用申明权限。每次安装时，系统根据包名为应用分配唯一的userID，不同的userID运行在不同的进程里，进程间的内存是独立的，不可以相互访问，除非通过特定的Binder机制。

Android提供了如下的一种机制，可以使两个apk打破前面讲的这种壁垒。

在AndroidManifest.xml中利用sharedUserId属性给不同的package分配相同的userID，通过这样做，两个package可以被当做同一个程序，系统会分配给两个程序相同的UserID。当然，基于安全考虑，两个package需要有相同的签名，否则没有验证也就没有意义了。


### 25、曲面屏的适配。


### 26、TextView调用setText方法的内部执行流程。


### 27、怎么控制另外一个进程的View显示（RemoteView）？


### 28、如何实现右滑finish activity？


### 29、如何在整个系统层面实现界面的圆角效果。（即所有的APP打开界面都会是圆角）


### 30、非UI线程可以更新UI吗?

可以，当访问UI时，ViewRootImpl会调用checkThread方法去检查当前访问UI的线程是哪个，如果不是UI线程则会抛出异常。执行onCreate方法的那个时候ViewRootImpl还没创建，无法去检查当前线程.ViewRootImpl的创建在onResume方法回调之后。

    void checkThread() {
        if (mThread != Thread.currentThread()) {
            throw new CalledFromWrongThreadException(
                    "Only the original thread that created a view hierarchy can touch its views.");
        }
    }
    
非UI线程是可以刷新UI的，前提是它要拥有自己的ViewRoot,即更新UI的线程和创建ViewRoot的线程是同一个，或者在执行checkThread()前更新UI。


### 31、如何解决git冲突？


### 32、单元测试有没有做过，说说熟悉的单元测试框架？


### 33、实现一个Json解析器(可以通过正则提高速度)。


### 34、Jenkins持续集成。


### 35、为什么 Google 会推出Fragment ，有什么好处和用途？ 直接用 View 代替不行么？


### 36、工作中有没有用过或者写过什么工具？脚本，插件等等；比如：多人协同开发可能对一些相同资源都各自放了一份，有没有方法自动检测这种重复之类的。




