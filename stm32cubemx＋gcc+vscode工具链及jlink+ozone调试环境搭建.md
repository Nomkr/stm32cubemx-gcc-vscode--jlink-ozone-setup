# stm32cubemx＋gcc+vscode工具链及jlink+ozone调试环境搭建

[TOC]

## 1. 安装VScode

如果电脑里没有 VS Code：

- 双击Packages-WinOS文件夹中的vscodesetup程序下载
- 安装完后可以在插件中搜索“Chinese”插件下载

如果电脑里已经有 VS Code：

- 可以不管
- 只要它能正常打开工程即可



## 2.安装MSYS2

双击MSYS2setup程序

下载完后打开MSYS2，输入以下内容：

```bash
pacman -Suy
```

（更新包管理器）

再安装工具：

```bash
pacman -S make mingw-w64-x86_64-toolchain mingw-w64-x86_64-arm-none-eabi-toolchain
```

如果安装过程中问你要安装哪些工具，通常直接回车默认即可。



## 3. 配置系统环境变量 Path

打开：

- Windows 搜索

- 搜索“环境变量”

  <img src="file:///C:\Users\20621\Documents\Tencent Files\2062191603\nt_qq\nt_data\Pic\2026-05\Ori\0404543cf4e8e792200172a628d926a9.png" alt="img" style="zoom:50%;" />

- 打开“编辑系统环境变量”

- 进入“环境变量”

- 找到系统变量 `Path`

  <img src="C:\Users\20621\AppData\Roaming\Typora\typora-user-images\image-20260523091750962.png" alt="image-20260523091750962" style="zoom:50%;" />

- 点“编辑”

加入两项路径（根据你自己的路径来添加）。

例如：

```text
C:\msys64\mingw64\bin
C:\msys64\usr\bin
```



## 4. 安装 STM32CubeMX(省略)

一般都安装过了，这里就不再赘述



## 5. 安装 J-Link Driver

在Packages-WinOS文件夹中下载对应程序即可

装好后可以将jlink插上电脑，打开“设备管理器”，看有没有类似：

- J-Link
- SEGGER J-Link
- 或者一类正常的 USB 调试设备

如果看到“黄色感叹号”或者“未知设备”，通常就是没识别好。



## 6. 安装 Ozone

在Packages-WinOS文件夹中下载对应程序即可



## 7. 在VScode中新建MSYS2终端并编译代码

在vscode里打开项目工程代码

crtl+`   （Esc键下面那个）打开终端，发现没有msys2终端。



按 `Ctrl + Shift + P`

输入：`Preferences: Open Settings (JSON)`

复制进以下代码：

```json
{
    "idf.hasWalkthroughBeenShown": true,
    "workbench.iconTheme": "material-icon-theme",
    "terminal.integrated.profiles.windows": {
        "MSYS2 MINGW64": {
            "path": "msys2/urs/bin/bash.exe所在目录",
            "args": [
                "-login"
            ],
            "env": {
                "MSYSTEM": "MINGW64",
                "CHERE_INVOKING": "1",
                "MSYS2_PATH_TYPE": "inherit"
            },
            "icon": "terminal-bash"
        }
    },

}
```

注意：path那行的路径一定要按照自己的路径来写

可能还要自己进行makefile文件的配置



然后再打开终端下拉按键，看看是否有出现msys2终端

<img src="file:///C:\Users\20621\Documents\Tencent Files\2062191603\nt_qq\nt_data\Pic\2026-05\Ori\912f86cecc70b4813266081b30207900.png" alt="img" style="zoom:80%;" />

若没有，关闭vscode或按 `Ctrl + Shift + P`，输入reload windows，再看看是否成功



打开msys2终端，输入以下代码，进入项目目录：

```
cd "(工程的makefile文件所在的相对路径) "
```





成功后，终端应该变到makefile所在文件夹下



然后输入：

`make`



即可编译，若出现绿色的`build success！`



则恭喜你，成功在vscode里完成编译了！



## 8. 在ozone中打开工程并烧录代码

首先你需要一个jlink，用jlink连接好烧录口和电脑





![image-20260523193800054](C:\Users\20621\AppData\Roaming\Typora\typora-user-images\image-20260523193800054.png)

第二张图是选择对应型号的stm32芯片



[STM32F3 Mixed-Signal Microcontrollers (MCU) - STMicroelectronics](https://www.st.com/en/microcontrollers-microprocessors/stm32f3-series.html#cad-resources)   这里面可以找所有stm32芯片的.svd文件

找到对应的.svd文件，并下载

<img src="file:///C:\Users\20621\Documents\Tencent Files\2062191603\nt_qq\nt_data\Pic\2026-05\Ori\ef2dfcb80fb5498f0fb3e8d153e60597.png" alt="img" style="zoom:95%;" />

点击蓝色框的Peripherals，选择你准备好的 .svd`文件，然后继续下一步即可

文件选择 /build 文件夹中的 .elf 文件，然后就可以在ozone中打开工程。

点击ozone左上角的绿色按钮，即可下载代码。



> 关于ozone的其他功能使用和界面的布局，可以自行上网查找相关教程，这里省略
>
> 关于代码下载和检查jlink驱动的部分有点忘记了，如果遇到问题可以问ai，在配置过程中如果遇到一些其他报错，也可以向ai求助，ai的回答往往十分高效



## 9. 安装stm32cubemx

这不是这次记录的重点，不再赘述



