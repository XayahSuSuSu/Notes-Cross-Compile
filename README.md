# 交叉编译 笔记

> Android NDK 版本：r22b (22.1.7171670) **--已上传 Releases**
>
> 目标平台：AArch64

## 环境准备

1. 下载 [Android NDK](https://developer.android.google.cn/ndk/downloads/)

2. 解压

## **CMake** 交叉编译 (以 [Brotli](https://github.com/google/brotli) 为例)

1. 更新Cmake版本 (最低 **3.19** )

   以 **cmake-3.21.0-rc1** 为例：
   
   ```
   wget https://cmake.org/files/v3.21/cmake-3.21.0-rc1.tar.gz

   tar -xvf cmake-3.21.0-rc1.tar.gz

   cd cmake-3.21.0-rc1

   ./configure

   sudo make

   sudo make install

   cmake --version         # 检查版本
   ```
   

2. 克隆 Brotli
   ```
   git clone https://github.com/google/brotli
   ```

3. 交叉编译
   ```
   cd brotli

   mkdir out && cd out

   export NDK=/home/xayah/Compile/NDK             # NDK根目录绝对路径

   export ABI=arm64-v8a                           # ABI配置(arm64-v8a 即为 AArch64)

   export MINSDKVERSION=24                        # 最小目标SDK版本配置(24 即为 Android 7.0)

   cmake \
      -DCMAKE_TOOLCHAIN_FILE=$NDK/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=$ABI \
      -DANDROID_NATIVE_API_LEVEL=$MINSDKVERSION \
      -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./installed ..

   cmake --build . --config Release
   ```

4. 编译产物生成于 **out** 文件夹中

## **Autoconf Makefile** 交叉编译 (以 [Make-4.3](http://ftp.gnu.org/gnu/make/make-4.3.tar.gz) 为例)

1. 下载 **make-4.3.tar.gz** 并解压

2. 交叉编译
   ```
   cd make-4.3

   export NDK=/home/xayah/Compile/NDK                              # NDK根目录绝对路径

   export TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/linux-x86_64     # 交叉编译链路径

   export TARGET=aarch64-linux-android                             # 交叉编译目标

   export API=24                                                   # 最小目标SDK版本配置(24 即为 Android 7.0)

   export AR=$TOOLCHAIN/bin/llvm-ar

   export CC=$TOOLCHAIN/bin/$TARGET$API-clang

   export AS=$CC

   export CXX=$TOOLCHAIN/bin/$TARGET$API-clang++

   export LD=$TOOLCHAIN/bin/ld

   export RANLIB=$TOOLCHAIN/bin/llvm-ranlib

   export STRIP=$TOOLCHAIN/bin/llvm-strip

   ./configure --enable-checking=release --enable-languages=c,c++ --disable-multilib --host $TARGET

   make
   ```

   注：其中的 **AR** 、 **CC** 、 **AS** 、 **CXX** 、 **LD** 、 **RANLIB** 、 **STRIP** 等决定于Makefile，我这里图方便就直接复制了 [NDK文档](https://developer.android.google.cn/ndk/guides/other_build_systems#autoconf) 中的变量，恰好已经覆盖完全，不会报错。当交叉编译报错时，请自行添加相应变量。

3. 编译产物生成于当前文件夹中

## **非 Autoconf Makefile** 交叉编译 (以 [Dtc](https://github.com/dgibson/dtc) 为例)

1. 克隆 **dtc**
   ```
   git clone https://github.com/dgibson/dtc
   ```

2. 交叉编译
   ```
   cd dtc

   export NDK=/home/xayah/Compile/NDK                              # NDK根目录绝对路径

   export TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/linux-x86_64     # 交叉编译链路径

   export TARGET=aarch64-linux-android                             # 交叉编译目标

   export API=24                                                   # 最小目标SDK版本配置(24 即为 Android 7.0)

   export AR=$TOOLCHAIN/bin/llvm-ar

   export CC=$TOOLCHAIN/bin/$TARGET$API-clang

   export AS=$CC

   export CXX=$TOOLCHAIN/bin/$TARGET$API-clang++

   export LD=$TOOLCHAIN/bin/ld

   export RANLIB=$TOOLCHAIN/bin/llvm-ranlib

   export STRIP=$TOOLCHAIN/bin/llvm-strip

   make
   ```

   注：其中的 **AR** 、 **CC** 、 **AS** 、 **CXX** 、 **LD** 、 **RANLIB** 、 **STRIP** 等决定于Makefile，我这里图方便就直接复制了 [NDK文档](https://developer.android.google.cn/ndk/guides/other_build_systems#autoconf) 中的变量，恰好已经覆盖完全，不会报错。当交叉编译报错时，请自行添加相应变量。

3. 编译产物生成于当前文件夹中