# Qt笔记

## Qt官方代码的BM字符串匹配算法

```cpp
inline void bm_init_skiptable(const uchar *cc, int len, uchar *skiptable)
{
    int l = qMin(len, 255);
    memset(skiptable, l, 256*sizeof(uchar));
    cc += len - l;
    while (l--)
        skiptable[*cc++] = l;
}
inline int bm_find(const uchar *cc, int l, int index, const uchar *puc, uint pl,
const uchar *skiptable)
{
    if (pl == 0)
        return index > l ? -1 : index;
    const uint pl_minus_one = pl - 1;
    const uchar *current = cc + index + pl_minus_one;
    const uchar *end = cc + l;
    while (current < end) {
        uint skip = skiptable[*current];
        if (!skip) {
            // possible match
            while (skip < pl) {
                if (*(current - skip) != puc[pl_minus_one - skip])
                    break;
                skip++;
            }
            if (skip > pl_minus_one) // we have a match
                return (current - cc) - skip + 1;
            // in case we don't have a match we are a bit inefficient as we only skip by one
            // when we have the non matching char in the string.
            if (skiptable[*(current - skip)] == pl)
                skip = pl - skip;
            else
                skip = 1;
        }
        if (current > end - skip)
            break;
        current += skip;
    }
    return -1; // not found
}

int qFindByteArrayBoyerMoore(
    const char *haystack, int haystackLen, int haystackOffset,
    const char *needle, int needleLen)
{
    uchar skiptable[256];
    bm_init_skiptable((const uchar *)needle, needleLen, skiptable);
    if (haystackOffset < 0)
        haystackOffset = 0;
    return bm_find((const uchar *)haystack, haystackLen, haystackOffset,
                   (const uchar *)needle, needleLen, skiptable);
}

```

+ **haystack:** 原字符串
+ **haystackLen:** 原字符串长度
+ **haystackOffset:** 偏移量
+ **needle:** 目标字符串
+ **needleLen:** 目标字符串长度

可以自己封装一个外部函数，把`Len`相关的参数去掉，根据传进去的字符串进行计算。

## Qt delegate

`QItemEditorFactory`创建item delegate工厂类，`QItemEditorCreatorBase`是editor接口，通过`QItemEditorCreator`创建实例。

```cpp
QItemEditorFactory *factory = new QItemEditorFactor();
QItemEditorCreatorBase *editor = new QItemEditorCreator<QComboBox>();//这里QComboBox是显示的widget，可以自定义为其他，对于QComboBox来说意义不大，因为需要在初始数据，最好是继承相关功能的类，添加更多功能
factory->registerEditor(editor);
QItemEditorFactory::setDefaultFactory(factory);
```

## Qt Event

自定义`Event`，可以通过继承`QEvent`，在自定义Event内部添加功能。

在使用`QCoreApplication::postEvent()`时，事件必须创建在堆中，因为`postEvent()`会将**事件对象**的拥有权转移到事件队列，事件在被`post`之后将被delete，不可以去再使用该**事件对象**。

```cpp
MyWidget w;
CustomEvent *event = new Custome((QEvent::Type)1001);
QCoreApplication::postEvent(&w, event);

event->doWork();// 若在被post之后调用，则错误
```

**用processEvents()** 处理耗时任务时，它虽然不会阻塞，但比一般情况下运行的速度要慢，而且可能比线程去处理也要慢。

## Qt翻译
翻译的加载必须放在窗口对象初始化之前，否则无效。

`QTranslator` 不应该被声明为局部变量（除了static），因为在生命周期结束之后即使调用`installTranslator()`，该翻译内容也会被清除，也就是说翻译内容存在于`QTranslator`对象之中。



## Qt Test

最近了解了TDD极限编程方法后，打算在实际开发中进行实践运用，所以首先要先确定好测试库。

目前C++比较有名且开源的测试库有`googletest`、`Catch2`等，而Qt自带了自动测试工具。

### Qt TestLib

测试库可以自己通过创建工程的方式创建，也可以在pro文件进行手动创建，我个人比较推崇第二种，因为这样的方式能直接在项目代码中进行直接使用。

在pro文件中添加`QT += testlib`，然后新建一个测试类，这个类必须继承自`QObject`。写一个测试用例这方面我就不写了，毕竟不重要，重要的在于如何写测试用例的内容。

先说说测试中常用到的宏

+ QCOMPARE(condition1, condition2) :比较值是否相等，相等亮绿灯，否则红灯
+ QVERIFY(condition1 == condition2) :同样以==方式比较值相等
+ QTEST(actual, testElement) :该测试宏适用于已存在一个测试变量，不过也可以声明临时变量
+ QBENCHMARK{} :该宏用于测试性能，可在`{}`里添加测试代码
+ QSKIP(description) : 直接跳出测试
+ QTEST_MAIN :创建测试用例相关的main
+ QFETCH(classname, instanceName) :声明一个测试实例

有时候我们需要多个数据进行测试，对相应的测试用例，新增一个命名上目标测试用例名+_data即可，例如`case_test_data`。

```cpp
void case_test()
{
    QFETCH(MyClass, cls);
    QFETCH(string, result);

    QCOMPARE(cls.testFun(), result);
}

void case_test_data()
{
    QTest::addColumn<MyClass>("cls");//字符串内容为QFETCH声明的变量的名称
    QTest::addColumn<string>("result");

    QTest::newRow("rowname1") << MyClass("arg1") << "result2";
    QTest::newRow("rowname2") << MyClass("arg2") << "result2";
}
```

### 界面GUI测试

**鼠标点击** :模拟鼠标事件测试，有以下几个接口

+ `QTest::mouseClick`
+ `QTest::mouseDClick`
+ `QTest::mousePress`
+ `QTest::mouseRelease`
+ `QTest::mouseMove`

**键盘按键** :模拟键盘事件测试相关接口

+ `QTest::keyClick`
+ `QTest::keyClicks`
+ `QTest::Press`
+ `QTest::Release`
+ `QTest::keySequence`
+ `QTest::keyEvent`

鼠标和键盘的事件测试同样可以进行多数据测试，可以借助`QTestEventList`添加多个事件，

```cpp
void case_event_test()
{
    QFETCH(QTestEventList, events);
    QFETCH(string, result);
    
    MyWidget w;
    events.simulate(&w);
    QVERIFY(w.text() == result);
}

void case_event_test_data()
{
    QTest::addColumn<QTestEventList>("events");
    QTest::addColumn<string>("result");

    QTestEventList list1;
    list1.addMouseClick(Qt::LeftButton);
    list1.addKeyClick('a');
    QTest::newRow("rowname1") << list1 << "a";

    QTestEventList list2;
    list2.addKeyClick('a');
    list2.addKeyClick(Qt::Key_BackSpace);
    QTest::newRow("rowname2") << list2 << "a";
}
```

### 信号与槽相关 TODO

### 网络测试 TODO

### 多线程测试 TODO

### 测试遇到的麻烦与问题

一般编写的测试用例都是一些类的函数，那么如何做到快速的访问函数及其相关的private变量?
1. 可在对应要测试的类中将测试类标识为`friend`,`firend class TestCls;`
2. 在测试中要使用该类，则直接创建即可。
3. 测试用例中模拟事件，则需要在预处理器定义中包含QT_WIDGETS_LIB、QT_GUI_LIB。
4. 模拟事件，可以使用QTest的接口，也可以直接使用qApp->notify()。
5. 模拟事件时，需要明确哪个控件处理该事件，例QTabWidget的鼠标点击页签，真正的消息处理对象是QTabBar。
6. 测试用例中，不能弹出模态窗体，否则会阻塞后续代码。
7. 用测试多数据必须声明为`Q_DECLARE_METATYPE()`和实现拷贝构造

## googletest记录

### 编译安装

```shell
mkdir build && cd build
cmake -Dgtest_build_tests=on -DCMAKE_INSTALL_PREFIX=. ..
make
make install
cp -r libgtest*.a /你的lib
cp -r libgmock*.a /你的lib
cp -r gtest /你的include
cp -r gmock /你的include
```

```cpp
testing::InitGoogleTest(&argc, argv);
return RUN_ALL_TESTS();
```

## QML Object Declarations

声明一个QML Object的属性顺序

+ id
+ property declarations
+ signal declarations
+ JavaScript functions
+ object properties
+ child objects
+ states
+ transitions
