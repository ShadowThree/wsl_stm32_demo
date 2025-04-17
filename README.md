## ˵��
1. ͨ��`stm32cubeMx`����`cmake`���̣�
2. ͨ��`wsl(ubuntu)`���벢���ع̼���

## ʹ�÷�ʽ
1. ˫��`*.ioc`ͨ��`STM32CubeMX`�򿪣��������ù��̣�ע��`Toolchain/IDE`��Ҫ����Ϊ`cmake`;Ȼ�����ɴ��룻
2. ��`wsl ubuntu`��ͨ��`cd /mnt/path_to_project`���빤��·����
3. ��`wsl ubuntu`��ͨ��`code .`�򿪹��̣�
4. ��`vscode`�ն�ִ��`cmake .`����`Makafile`�ļ���
5. ��`vscode`�ն�ִ��`make`���빤�̣�
6. ��`vscode`�а���ݼ�`Ctrl Shift B`����`.vscode/tasks.json`�ж�������񣬽��й̼����أ�
```bash
# or download by this command
openocd -f interface/jlink.cfg -c "transport select swd" -f target/stm32f1x.cfg -c "program f103rc_blink.elf verify reset exit"
```

## ע��
1. `wsl`�в���ֱ��ʹ��������`USB`�豸����Ҫͨ��[USBIPD-win](https://github.com/dorssel/usbipd-win/releases)����`USB`�豸;
```bash
# windows powershell
usbipd list     # get the usb devices
usbipd bind --force --busid <busid>     # �ο�ע��4
usbipd attach --wsl --busid <busid>     # attach the JLink to wsl
usbipd detach --busid <busid>       # detach the JLink from wsl

# wsl ubuntu
lsusb       # check if the jlink is shared succssful
```
2. `wsl`�������ع�����ʹ�õ��˸��ֿ⣬��Ҫ��ǰ��װ��
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
3. `JLink`����`WSL`��������������ʹ����
```
�����豸�����������Կ����豸��������`JLink`������`USBIP shared device`���һ�ж���豸��ѡ��ɾ��������Ȼ�����°β�`JLink`���ɣ�
```
4. `usbipd: warning: USB filter 'USBPcap' is known to be incompatible with this software; 'bind --force' will be required.`
```
����������ϵͳ�а�װ�� USBPcap��һ�� USB ���ݰ����񹤾ߣ������� USBIPD ���ڼ��������⡣USBPcap ��һ�����ڲ��� USB ͨ�����ݵĹ��ߣ�ͨ���� Wireshark һ��ʹ�á�
����һ��ж�� USBPcap;
��������attach ǰ�� usbipd bind --force --busid <busid>
```

## ��������Ŀ�Դ��
1. �ο� [CMakeLists.txt](./CMakeLists.txt) ��ע�⣺ͷ�ļ�ֻ��Ҫд���ļ��У�Դ�ļ���֧��ͨ���`*.c`��
2. �޸� [CMakeLists.txt](./CMakeLists.txt) ����Ҫ����ִ�� `cmake .` ��

## �ض�����־
1. ���ͨ��`linux`����`cmake`���̣�������Ҫ��ӡ����ֵ������£���Ҫʹ��`_printf_float`ѡ�
```CMakeLists.txt
# �� CMakeLists.txt �������������
target_link_options(${CMAKE_PROJECT_NAME} PRIVATE -u _printf_float)
```
2. ���ͨ��`linux`��ͨ��`JLinkRTTLogger`һֱ�ղ�����־���п�����`JLink`����̫���ˣ�ж�غ����°�װ���ɣ�
```bash
# https://www.segger.com/downloads/jlink/
sudo apt purge jlink    # uninstall
sudo dpkg -i JLink_Linux_V766d_x86_64.deb   # install
JLinkRTTLogger      # recv RTT log
```