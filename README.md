# 交叉编译 笔记

> Android NDK 版本：r22b (22.1.7171670)
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