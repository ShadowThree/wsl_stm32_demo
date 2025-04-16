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
usbipd attach --wsl --busid 2-9     # attach the JLink to wsl
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