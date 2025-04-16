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
usbipd attach --wsl --busid 2-9     # attach the JLink to wsl
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