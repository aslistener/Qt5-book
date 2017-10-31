## Qt Document

### Qdoc 介绍

Qdoc是开发者用于在软件工程中生成文档的一个工具。它从工程的源文件中提取qdoc类型注释，并以html页面或者DITA XML文档的形式格式化到文件中。Qdoc在.cpp和.qdoc文件中查找注释，而不会在.h文件中查找。一条qdoc注释往往以一个前置声明符号（!）开始，例如：

```
\class QObject  

\brief The QObject class is the base class of all Qt objects.  

\ingroup objectmodel  

\reentrant  

QObject is the heart of the Qt \l{Object Model}. The

central feature in this model is a very powerful mechanism

for seamless object communication called \l{signals and slots}.

You can connect a signal to a slot with connect\(\)

and destroy the connection with disconnect\(\). To avoid

never ending notification loops you can temporarily block

signals with blockSignals\(\). The protected functions

connectNotify\(\) and disconnectNotify\(\) make it possible to

track connections.  

QObjects organize themselves in \l {Object Trees &

Ownership} {object trees}. When you create a QObject with

another object as parent, the object will automatically

add itself to the parent's \c children\(\) list. The parent

takes ownership of the object. It will automatically

delete its children in its destructor. You can look for an

object by name and optionally type using findChild\(\) or

findChildren\(\).  

Every object has an objectName\(\) and its class name can be

found via the corresponding metaObject\(\) \(see

QMetaObject::className\(\)\). You can determine whether the

object's class inherits another class in the QObject

inheritance hierarchy by using the \c inherits\(\) function.

....
```

根据以上的qdoc注释， qdoc生成html页面格式的qobject类型定义。

本节说明了怎么使用qdoc命令读取源代码文件中的qdoc注释并生成文档，以及怎么生成 QDoc configure file（qdocconf文件）——当在命令行下使用qdoc工具时， 需要把qdocconf文件传给QDoc。

\#\# 运行 QDoc

QDoc的可执行文档名为qdoc，如果要在命令行下运行，需要传入.qdocconf文件。

```
$ ../../bin/qdoc ./config.qdocconf
```

Qdoc把以.qdocconf结尾的参数理解为QDoc configure file，configure file是使用者用以告诉qdoc何处存放源代码文件、头文件以及.qdoc文件，同时它也指定了以何种格式（HTML、DITA XML....）输出结果，以及输出文件的位置。Configure file也包含了一些其它信息，详见下文。

### 使用single mode运行qdoc

从Qt5.5开始，一个使用QDoc的新的方法产生了，它可以节省生成Qt5文档所需要的时间多达90%，这个新的方法就是single mode。目前在Qt5 编译系统（qmake）中仍在使用standard mode，single mode还不可用。只有在单独使用QDoc时才能使用single mode，而这往往出现在当你想为你自己的模块制作文档并且把它和其它模块集成起来的时候。

使用QDoc的single mode时，需要添加-single-exec参数，并传入一个master qdocconf文件，这个文件只是简单包括了所有Qt5模块的qdocconf文件路径。例如：

```
/Users/me/qt5/qtbase/bin/qdoc -outputdir /Users/me/qt5/qtbase/doc -installdir /Users/me/qt5/qtbase/doc /Users/me/qt5/master.qdocconf -single-exec
```

传入的master.qdocconf文件，需要列举Qt5模块的所有qdocconf文件：

```
/Users/me/qt5/qtbase/src/corelib/doc/qtcore.qdocconf

/Users/me/qt5/qtbase/src/network/doc/qtnetwork.qdocconf

/Users/me/qt5/qtbase/src/sql/doc/qtsql.qdocconf

/Users/me/qt5/qtbase/src/xml/doc/qtxml.qdocconf

/Users/me/qt5/qtbase/src/testlib/doc/qttestlib.qdocconf

/Users/me/qt5/qtbase/src/concurrent/doc/qtconcurrent.qdocconf

/Users/me/qt5/qtbase/src/gui/doc/qtgui.qdocconf

/Users/me/qt5/qtbase/src/platformheaders/doc/qtplatformheaders.qdocconf

/Users/me/qt5/qtbase/src/widgets/doc/qtwidgets.qdocconf

/Users/me/qt5/qtbase/src/opengl/doc/qtopengl.qdocconf

/Users/me/qt5/qtbase/src/printsupport/doc/qtprintsupport.qdocconf

/Users/me/qt5/qtbase/src/tools/qdoc/doc/config/qdoc.qdocconf

/Users/me/qt5/qtbase/qmake/doc/qmake.qdocconf

/Users/me/qt5/qtsvg/src/svg/doc/qtsvg.qdocconf

/Users/me/qt5/qtxmlpatterns/src/xmlpatterns/doc/qtxmlpatterns.qdocconf

/Users/me/qt5/qtdeclarative/src/qml/doc/qtqml.qdocconf

/Users/me/qt5/qtdeclarative/src/quick/doc/qtquick.qdocconf

/Users/me/qt5/qtquickcontrols/src/controls/doc/qtquickcontrols.qdocconf

/Users/me/qt5/qtquickcontrols/src/layouts/doc/qtquicklayouts.qdocconf

/Users/me/qt5/qtquickcontrols/src/dialogs/doc/qtquickdialogs.qdocconf

/Users/me/qt5/qtmultimedia/src/multimedia/doc/qtmultimedia.qdocconf

/Users/me/qt5/qtmultimedia/src/multimediawidgets/doc/qtmultimediawidgets.qdocconf

/Users/me/qt5/qtactiveqt/src/activeqt/doc/activeqt.qdocconf

/Users/me/qt5/qtsensors/src/sensors/doc/qtsensors.qdocconf

/Users/me/qt5/qtwebkit/Source/qtwebkit.qdocconf

/Users/me/qt5/qttools/src/assistant/help/doc/qthelp.qdocconf

/Users/me/qt5/qttools/src/assistant/assistant/doc/qtassistant.qdocconf

/Users/me/qt5/qttools/src/designer/src/uitools/doc/qtuitools.qdocconf

/Users/me/qt5/qttools/src/designer/src/designer/doc/qtdesigner.qdocconf

/Users/me/qt5/qttools/src/linguist/linguist/doc/qtlinguist.qdocconf

/Users/me/qt5/qtwebkit-examples/doc/qtwebkitexamples.qdocconf

/Users/me/qt5/qtimageformats/src/imageformats/doc/qtimageformats.qdocconf

/Users/me/qt5/qtgraphicaleffects/src/effects/doc/qtgraphicaleffects.qdocconf

/Users/me/qt5/qtscript/src/script/doc/qtscript.qdocconf

/Users/me/qt5/qtscript/src/scripttools/doc/qtscripttools.qdocconf

/Users/me/qt5/qtserialport/src/serialport/doc/qtserialport.qdocconf

/Users/me/qt5/qtdoc/doc/config/qtdoc.qdocconf
```

### 为什么standard mode比较慢

目前，Qt5 building system并没有使用single mode生成qdoc文档，而是使用了standard mode。当下在转换Qt4文档以识别Qt5模块化这方面，Standard mode是最简单的方法了。在Qt4中，QDoc运行一次，遍历所有的Qt4源代码文件，生成html格式的qdoc文档。在生成qdoc文档的同时，qt4 QDoc还生成了一个索引文件（index file）。这个文件用于在生成依赖于Qt的其它类库、软件产品的html文档时，序列化QDoc的操作，还可以用于把其它类库、软件产品的qdoc文档链接到qt4文档中。

在Qt5中，Qt被模块化，之后，越来越多的模块被加入到Qt5中，在5.5.时，已经有40多个独立模块了。这些模块都有着自己的文档，而不同模块的文档又存在着相互的链接和依赖。

Standard mode中，QDoc为每个模块运行了两次。第一次QDoc在某个指定的模块中运行，遍历所有的源代码文件，并根据获得的一些信息生成索引文件。这个阶段被称为“准备阶段”（prepare phase），因为它生成了索引文件，为下一步的进行做准备。第二次运行依旧遍历所有源代码文件，然后生成qdoc文档，这个阶段被称作“生成阶段”（generate mode），因为它生成了该模块的qdoc文档。

某个模块的qdoc文档可能会包含一个或者多个其它模块的html链接，例如，大部分模块的qdoc文档都有指向QtCore的链接。当一个qdoc文档包含指向其它模块的链接时，认为这个模块依赖于后者。因此当QDoc在该模块下执行生成阶段时，它必须加载所依赖模块的索引文件，以创建这些链接。

\*由此可知\*， 当Qt5 Building

System在生成Qt文档时，它先在每个模块中运行一次QDoc生成索引文件（准备阶段），然后又在每个模块中运行一次QDoc生成qdoc文档（生成阶段）；在第二个阶段，QDoc使用那些相互依赖（链接）的索引文件生成qdoc文档，并把相互之间的链接包含进去。每一次执行QDoc，都会遍历一次源代码文件，在生成阶段还会为依赖关系遍历索引文件。两次QDoc的执行期间没有缓存信息。

### 为什么single mode比较快

顾名思义，single mode执行一次QDoc就能生成所有的qdoc文档。其实这个QDoc过程仍然会在每个模块中执行一个“准备阶段”\(prepare phase\)，一个“生成阶段”\(generate phase\)，但是有所不同。它从读取master qdocconf文件开始，然后读取master

qdocconf里面的qdocconf文件列表，并为每个文件所在的模块执行“准备阶段”，QDoc会遍历这个模块的所有源代码文件，以生成一个语法树，然后是这个模块的索引文件——尽管这个索引文件不会在第二个阶段中被读取。这里和standard mode不同的是，QDoc会在生成索引文件以后保留语法树；所以当所有模块的准备阶段结束后，QDoc仍保留着它建立的语法树。

接着QDoc执行生成阶段，但是这里QDoc已经不用再遍历模块下的所有源代码文件，也不用再读取索引文件，因为内存中保留着该模块的语法树。

这样，QDoc仅遍历一次源代码文件，并且不用读取索引文件，这也是single mode比standard mode快得多的原因。当前的趋势是Qt Building System中会最终使用single mode。

### QDoc工作过程

QDoc从读取由命令行传入的qdocconf文件开始，它把从该文件中读取到的变量保存起来，并在以后使用。例如它使用的一个较早传入的变量 outputformats，它告诉QDoc使用哪个输出生成器。默认是HTML，所以如果你不设置outputformats，QDoc会生成html文档。这通常能够满足用户的要求，但是有时候，用户会需要DITA XML类型的输出，这时需要赋值 DITAXML。

然后QDoc读取headers或者headerdirs变量，并遍历工程里的所有头文件。QDoc不会读取头文件里的注释，而是使用头文件生成一个 master tree，它包含了所有应该在文档中被包括的节点。

接下来，QDoc读取sourcedirs和sources目录，遍历所有的cpp文件和qdoc文件，并读取文件里的qdoc注释。注意qdoc注释是以!开头的（在注释内部）

