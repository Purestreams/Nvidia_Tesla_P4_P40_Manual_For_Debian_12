# Nvidia_Tesla_P4_P40_Manual_For_Debian_12
适用于 Linux Debian 12 安装Tesla P4 P40等GPU的显卡Cuda和驱动
Suitable for Linux Debian 12 to install graphics card Cuda and driver for Tesla P4 P40 and other GPUs

在pve 8.2中使用q35（不可使用i440fx)机型创建的虚拟机中安装Nvidia gpu驱动的教程。
Tutorial for installing Nvidia gpu drivers in a virtual machine created with the q35 (not available with i440fx) model in pve 8.2.

![image](https://github.com/user-attachments/assets/ea928233-8f01-45ac-a395-739a5f84c02b)
![image](https://github.com/user-attachments/assets/9e5a8337-09ce-49d4-bc44-727cfb887154)



## 检查显卡存在
通常可以使用 lspci 命令来识别已安装显卡的 NVIDIA 图形处理单元 (GPU) 系列/代号。例如：
You can usually use the lspci command to identify the NVIDIA graphics processing unit (GPU) family/codename of an installed graphics card. For example:
```bash
$ lspci | grep NVIDIA
```

## 为apt允许非自由软件源
Allow non-free software sources for apt

```bash
for non-Chinese users:
vim.tiny /etc/apt/sources.list

Add "contrib", "non-free" and "non-free-firmware" components to /etc/apt/sources.list, for example:

# Debian Bookworm
deb http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware

```
```bash
# 对于中国用户更换清华tuna源：
vim.tiny /etc/apt/sources.list

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware


```



### 更新apt
### Update apt
```bash
apt update -y
```

## 安装显卡驱动
```bash
apt install nvidia-driver firmware-misc-nonfree
```
重启电脑

### 检查 Debian 12 上是否安装了显卡驱动
```bash
nvidia-smi

+---------------------------------------------------------------------------------------+
| NVIDIA-SMI                                                                            |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  Tesla P4                       On  | 00000000:03:00.0 Off |                  Off |
| N/A   38C    P8               6W /  75W |      0MiB /  8192MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```

显卡驱动已安装完毕



## 安装Cuda
install Cuda

```bash
apt install nvidia-cuda-dev nvidia-cuda-toolkit
```

### 检查 Debian 12 上是否安装了 NVIDIA CUDA
```bash
nvcc --version
```


## 安装 NVIDIA cuDNN
install cuDNN

```bash
apt install nvidia-cudnn
```
看到窗口后，按 <Enter> 键同意。
Once you see the window, press <Enter> to agree the license.


NVIDIA cuDNN 库需要从 NVIDIA 官方网站下载。需要一段时间。
NVIDIA cuDNN libraries are being downloaded from the official website of NVIDIA. It takes a while to complete.


## 关闭ECC
Turn off ECC

通过```nvidia-smi | grep Tesla```查看前面GPU编号
```bash
d@d:/fuck$ nvidia-smi | grep Tesla
|   0  Tesla P40                      On  | 00000000:03:00.0 Off |                  Off |
-----------------------------------------------------------------------------------------
nvidia-smi -i n -e 0/1 可关闭(0)/开启(1) , n是GPU的编号。
```

执行关闭ECC```sudo nvidia-smi -i 0 -e 0```重启后该设置生效。




