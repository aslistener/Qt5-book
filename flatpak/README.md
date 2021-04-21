# 什么是flatpak
flatpak 是一套集成了编译、分发、安装、运行的软件管理套件。
它能够解决linux发行版过多，导致开发者适配的工作量较大的问题，另外能够复用已经编译好的工程（当然，这个能力大部分其它构建系统也有）。它还能够让应用在自定义的沙箱中运行。
它的缺点是交叉编译能力较弱。

# 用户角度
- 对于用户，主要的行为是安装、运行软件；在使用flatpak时，他们看到的是一个中心化的软件仓库
- 平台无关，flatpak 支持在不同版本的linux中运行同一套UI应用程序，这些不同版本的linux桌面环境、软件运行时可能都有很大差异。用户在跨越linux版本时，传统的方式有比较大的学习成本。
- 有图形化工具
    >> Gnome 提供了一个 Flatpak 插件，安装它就可以使用**软件商店**来安装 Flatpak app 了。插件的安装命令为：

        sudo apt install gnome-software-plugin-flatpak

- 软件运行在沙箱里，对系统资源有严格的限制
    >> 例如，在metadata中把应用程序的 `filesystems=host;`(表示使用本机的文件系统)去掉，应用程序就会无法启动，并提示各种无法加载库、无法读取文件的错误。 

## 安装和运行
- 添加一个软件仓库

    flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

这个命令会添加一个名为flathub的软件仓库。

>> 其中调试时，可以使用本地仓库。本地仓库的添加，见 [分发](#分发) 一节。


- 查找/下载/卸载命令
>> 有两种安装方式，system模式安装在/var/run/flatpak/app, user模式安装在 .local/share/flatpak/app

    flatpak search gimp
    flatpak install flathub org.gimp.GIMP 
    flatpak uninstall org.gimp.GIMP

- 运行

    flatpak run org.gimp.GIMP

## 对于包提供者(developer)
- 下载 Software Development Kits

    flatpak install flathub  org.freedesktop.Platform//18.08 org.freedesktop.Sdk//18.08

- 第一步执行以下命令，初始化一个工作路径

    flatpak build-init browser-build com.browser360.Browser360 org.freedesktop.Sdk org.freedesktop.Platform

这个步骤会在browser-build目录下生成三个路径：/files 里面放需要安装的文件，export 存放可以被其他应用看到的文件，比如图标、desktop文件等。/var 路径链向 flatpak的“系统路径”.  一个 metadata, 用于填写软件的运行环境的大部分信息 。<br/>

需要注意的是， 执行上面那个命令，flatpak会提醒你没有指定 runtime(org.freedesktop.Platform)、sdk（org.freedesktop.Sdk）版本，你需要按照提示把它们替换为相应的版本。

- 编译
>> 这些步骤并不都是必须的，一般来说，应用厂商不会提供源代码，所以不会在flatpak里面执行gcc进行编译。<br/>
>> 安装 flatpak-builder


    flatpak build browser-build gcc main.c -o /app/bin/main
    flatpak build-finish browser-build
    flatpak build-export my-repo browser-build

- 分发
>> 执行完  build-export 以后，本地会有一个名字为  my-repo 的本地仓库，这个是应用程序的repo；需要把它放到本地仓库或者远端仓库上。<br/>
>> 例如，本地仓库名为 test-repo，则执行命令：

     flatpak remote-add --user --no-gpg-verify test-repo my-repo
    
    




#### 编译
- 编译的输入是一个 json 文件以及一个目录，目录里面是源代码、打包资源； json文件里面填写的是编译相关的命令。以下为一个简单的例子。其中有几个关键字段需要介绍下。
    - app-id: 表示应用程序名称，一般以域名倒序命名。
    - runtime, sdk,runtime-version: 表示应用程序的编译环境、运行环境，以及环境版本号。
    - branch: 表示操作的代码分支，默认是master
    - command: 应用程序实际运行的命令
    - finish-args: 这里面是一些应用程序所需权限
    - modules: 这里面是对已经打包好的库的引用
    - sources: 这里面是依赖文件，它可以是压缩包、目录、文件；可以为它指定编译命令； 有一些构建系统，flatpak已经经过封装，所以使用起来很方便。比如autoconf， 如果没有指定编译命令，默认情况下flatpak 会运行 `./configure --prefix=/${app-id}`

            {
            "app-id": "com.browser360.Browser360",
            "runtime": "org.gnome.Platform",
            "runtime-version": "3.24",
            "sdk": "org.gnome.Sdk",
            "branch": "stable",
            "command": "browser360-cn",
            "finish-args": [
                "--socket=x11",
                "--share=network",
                "--filesystem=host",
                "--share=ipc",
                "--allow=bluetooth",
                "--device=dri",
                "--socket=pulseaudio",
                "--socket=fallback-x11",
                "--socket=session-bus",
                "--socket=system-bus"
            ],
            "modules": [
                {
                "shared-modules/udev/udev-175.json",
                    {
                    "name": "libudev1",
                    "buildsystem": "simple",
                    "build-commands": [
                        "ln -s /app/lib/libudev.so.0 /app/lib/libudev.so.1"
                    ]          
                    }
                }
            }

### 分发


- translation文件、debug文件在extension包里

- [github仓库](https://github.com/flathub) 有很多可借鉴的例子

- 如果不执行 flatpak-builder, 则不需要 json 文件







## 参考文档
- [一个完整的应用程序概念](https://github.com/flathub/flathub/wiki/App-Requirements)，它讲述了对于一个应用程序来说，需要什么组件。当然根据自己的需要，很多是可选的。
- [一个完整的例子](https://www.booleanworld.com/building-cross-distribution-linux-applications-flatpak/)，这里讲述了flatpak 从编译到发布的完整例子。
- [reference](https://docs.flatpak.org/en/latest/flatpak-command-reference.html)
- [官网](https://docs.flatpak.org/en/latest/)
- [镜像管理](https://ostree.readthedocs.io/en/latest/manual/repository-management/)
- [Maintaining a flatpak repository]https://blogs.gnome.org/alexl/2017/02/10/maintaining-a-flatpak-repository/
