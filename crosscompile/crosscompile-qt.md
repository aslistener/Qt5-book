# cross compile Qt



## compiling script

    #! /bin/bash

    export PATH=/home/maqidi/project/mytest/qt/arm64/usr/aarch64-linux-gnu/bin:/home/maqidi/project/mytest/qt/arm64/usr/bin:$PATH
    export LD_LIBRARY_PATH=/home/maqidi/project/mytest/qt/arm64/usr/lib/x86_64-linux-gnu/:$LD_LIBRARY_PATH
    export cc=aarch64-linux-gnu-gcc
    export cxx=aarch64-linux-gnu-g++
    export as=aarch64-linux-gnu-as
    export CFALGS="--sysroot /home/maqidi/project/mytest/qt/arm64/usr"
    export CPPFLAGS="--sysroot /home/maqidi/project/mytest/qt/arm64/usr"
    export CXXFLAGS="--sysroot /home/maqidi/project/mytest/qt/arm64/usr"

    export custom_make="make CC=aarch64-linux-gnu-gcc CXX=aarch64-linux-gnu-g++ CPP=aarch64-linux-gnu-cpp LD=aarch64-linux-gnu-ld" 

    SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
    if [ "$1" == "0" ]; then
        make clean
        ./configure -no-opengl -opensource -confirm-license -xplatform linux-aarch64-gnu-g++ \
    -I /home/maqidi/project/mytest/qt/arm64/usr/include -I /home/maqidi/project/mytest/qt/arm64/usr/include/aarch64-linux-gnu
    -device-option CROSS_COMPILE=linux-aarch64-gnu- --sysroot /home/maqidi/project/mytest/qt/install 
    #\
    #-make tools -no-opengl
        #-datadir /home/maqidi/project/mytest/qt/arm64/usr/lib/aarch64-linux-gnu/qt5 \
        #-hostdatadir /home/maqidi/project/mytest/qt/arm64/usr/lib/aarch64-linux-gnu/qt5 \
        #-pkg-config=/home/maqidi/project/mytest/qt/arm64/usr/lib/aarch64-linux-gnu/pkgconfig 

    elif [ "$1" == "1" ]; then
        ${custom_make}
        exit 0
    fi

    ${custom_make} -j60 && make install

## explanations


## questions



## links

https://doc.qt.io/qt-5/configure-linux-device.html
https://doc.qt.io/qt-5/qt-conf.html
https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18842110/Qt+Qwt+Build+Instructions+Qt+5.4.2+Qwt+6.1.2
