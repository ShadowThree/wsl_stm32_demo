## 说明
1. 通过`stm32cubeMx`生成`cmake`工程；
2. 通过`wsl(ubuntu)`编译并下载固件；

## 使用方式
1. 双击`*.ioc`通过`STM32CubeMX`打开，正常配置工程，注意`Toolchain/IDE`需要设置为`cmake`;然后生成代码；
2. 打开`wsl ubuntu`，通过`cd /mnt/path_to_project`进入工程路径；
3. 在`wsl ubuntu`中通过`code .`打开工程；
4. 在`vscode`终端执行`cmake .`生成`Makafile`文件；
5. 在`vscode`终端执行`make`编译工程；
6. 在`vscode`中按快捷键`Ctrl Shift B`触发`.vscode/tasks.json`中定义的任务，进行固件下载；
```bash
# or download by this command
openocd -f interface/jlink.cfg -c "transport select swd" -f target/stm32f1x.cfg -c "program f103rc_blink.elf verify reset exit"
```

## 注意
1. `wsl`中不能直接使用主机的`USB`设备，需要通过[USBIPD-win](https://github.com/dorssel/usbipd-win/releases)共享`USB`设备;
```bash
# windows powershell
usbipd list     # get the usb devices
usbipd bind --force --busid <busid>     # 参考注意4
usbipd attach --wsl --busid <busid>     # attach the JLink to wsl
usbipd detach --busid <busid>       # detach the JLink from wsl

# wsl ubuntu
lsusb       # check if the jlink is shared succssful
```
2. `wsl`编译下载过程中使用到了各种库，需要提前安装：
```bash
# wsl ubuntu
sudo apt install build-essential git make cmake
sudo apt install gcc-arm-none-eabi
sudo apt install openocd    # fw download

sudo apt install usbutils   # lsusb

# install usbip tool
sudo apt install linux-tools-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*/usbip 20
```
3. `JLink`共享到`WSL`后，主机不能正常使用了
```
进入设备管理器，可以看到设备名不再是`JLink`，而是`USBIP shared device`，右击卸载设备并选择删除驱动，然后重新拔插`JLink`即可；
```
4. `usbipd: warning: USB filter 'USBPcap' is known to be incompatible with this software; 'bind --force' will be required.`
```
这个警告表明系统中安装了 USBPcap（一个 USB 数据包捕获工具），它与 USBIPD 存在兼容性问题。USBPcap 是一个用于捕获 USB 通信数据的工具，通常与 Wireshark 一起使用。
方案一：卸载 USBPcap;
方案二：attach 前先 usbipd bind --force --busid <busid>
```

## 添加其他的库源码
1. 参考 [CMakeLists.txt](./CMakeLists.txt) 。注意：头文件只需要写到文件夹，源文件不支持通配符`*.c`；
2. 修改 [CMakeLists.txt](./CMakeLists.txt) 后需要重新执行 `cmake .` 。

## 重定向日志
1. 如果通过`linux`编译`cmake`工程，并且需要打印浮点值的情况下，需要使能`_printf_float`选项：
```CMakeLists.txt
# 在 CMakeLists.txt 中添加如下配置
target_link_options(${CMAKE_PROJECT_NAME} PRIVATE -u _printf_float)
```
2. 如果通过`linux`下通过`JLinkRTTLogger`一直收不到日志，有可能是`JLink`驱动太高了，卸载后重新安装即可：
```bash
# https://www.segger.com/downloads/jlink/
sudo apt purge jlink    # uninstall
sudo dpkg -i JLink_Linux_V766d_x86_64.deb   # install
JLinkRTTLogger      # recv RTT log
```