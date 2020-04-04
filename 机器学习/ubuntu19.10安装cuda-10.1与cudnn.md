# ubuntu19.10安装CUDA-10.1与cuDNN

[toc]

## cuda安装

### 1.安装N卡驱动：

打开ubuntu的软件和更新，设置N卡驱动

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5jYW5neWUubWUvMjAyMC8wMi8yMy91YnVudHUlRTYlOTglQkUlRTUlOEQlQTElRTklQTklQjElRTUlOEElQTglRTglQUUlQkUlRTclQkQlQUUucG5n?x-oss-process=image/format,png)

### 2.查看ubuntu显卡驱动

```
nvidia-smi
```

显示：

```bash
Sun Feb 23 06:41:41 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 430.50       Driver Version: 430.50       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce 940M        Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   44C    P0    N/A /  N/A |    306MiB /  2004MiB |     28%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0       982      G   /usr/lib/xorg/Xorg                            23MiB |
|    0      1390      G   /usr/lib/xorg/Xorg                           102MiB |
|    0      1613      G   /usr/bin/gnome-shell                          95MiB |
|    0      2036      G   ...AAAAAAAAAAAAAAgAAAAAAAAA --shared-files    34MiB |
+-----------------------------------------------------------------------------+

```

可以看到

```bash
CUDA Version: 10.1
```

### 3.我们下载[CUDA Version: 10.1](https://developer.nvidia.com/cuda-10.1-download-archive-base?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1810&target_type=deblocal)

下载后，在安装包目录打开终端输入：

```
sudo dpkg -i cuda-repo-ubuntu1810-10-1-local-10.1.105-418.39_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-10-1-local-10.1.105-418.39/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```

### 4.至此安装包安装完成，接下来将cuda加入环境变量：

以上`deb`方式的安装，安装目录一般在`/usr/local/cuda-10.1`

如果不确定的话，可以查询一下目录在哪里。

`ubuntu`终端输入：

```bash
whereis cuda
```

输出：

```bash
/usr/local/cuda-10.1
```

这就知道`cuda`在哪个目录了

接下来继续我们的操作

```bash
sudo gedit .bashrc
```
```bash
export PATH=/usr/local/cuda-10.1/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64:$LD_LIBRARY_PATH
```
```bash
source .bashrc
```

如果你用的是`zsh`终端。那么，文件是`.zshrc`而不是`.bashrc`，切记！

`zsh`只要把上面的`.bashrc`替换成`.zshrc`即可

### 5.打开安装目录/ usr / local / cuda- 10.1

输入`nvcc -V`,显示cuda版本:

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Fri_Feb__8_19:08:17_PST_2019
Cuda compilation tools, release 10.1, V10.1.105
```

### 6.在编译样例目录/usr/local/cuda-10.1/samples输入`make`发现出错，出错显示：

```bash
make[1]: 进入目录“/usr/local/cuda-10.1/samples/0_Simple/simpleSeparateCompilation”
/usr/local/cuda-10.1/bin/nvcc -ccbin g++ -I../../common/inc  -m64    -dc -gencode arch=compute_30,code=sm_30 -gencode arch=compute_35,code=sm_35 -gencode arch=compute_37,code=sm_37 -gencode arch=compute_50,code=sm_50 -gencode arch=compute_52,code=sm_52 -gencode arch=compute_60,code=sm_60 -gencode arch=compute_61,code=sm_61 -gencode arch=compute_70,code=sm_70 -gencode arch=compute_75,code=sm_75 -gencode arch=compute_75,code=compute_75 -o simpleDeviceLibrary.o -c simpleDeviceLibrary.cu
In file included from /usr/local/cuda-10.1/bin/../targets/x86_64-linux/include/cuda_runtime.h:83,
                 from <command-line>:
/usr/local/cuda-10.1/bin/../targets/x86_64-linux/include/crt/host_config.h:129:2: error: #error -- unsupported GNU version! gcc versions later than 8 are not supported!
  129 | #error -- unsupported GNU version! gcc versions later than 8 are not supported!
      |  ^~~~~
make[1]: *** [Makefile:290：simpleDeviceLibrary.o] 错误 1
make[1]: 离开目录“/usr/local/cuda-10.1/samples/0_Simple/simpleSeparateCompilation”
make: *** [Makefile:51：0_Simple/simpleSeparateCompilation/Makefile.ph_build] 错误 2
```

重点在：

```bash
error -- unsupported GNU version! gcc versions later than 8 are not supported!
```

### 7.原因是ubuntu 19.10 gcc版本过高，所以我们需要使用8.0的gcc g++

为了解决这个问题，我们可以下载gcc-8 g++-8来结合 update-alternatives 对样例进行编译

首先安装gcc-8 g++-8

```bash
sudo apt-get install gcc-8 g++-8
```

安装完成后我们开始使用update-alternatives进行版本控制，语法如下：

```bash
update-alternatives --install  <链接> <名称> <路径> <优先级>
```

对于gcc有，命令行末尾的数字表优先级，数字越大优先级越高

```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 20
```

版本设置：

```bash
sudo update-alternatives --config gcc
```

显示：

```bash
有 2 个候选项可用于替换 gcc (提供 /usr/bin/gcc)。

  选择       路径          优先级  状态
------------------------------------------------------------
* 0            /usr/bin/gcc-9   20        自动模式
  1            /usr/bin/gcc-8   10        手动模式
  2            /usr/bin/gcc-9   20        手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：1（我选的1）
update-alternatives: 使用 /usr/bin/gcc-8 来在手动模式中提供 /usr/bin/gcc (gcc)
```

对于g++

```bash
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 20
```

版本设置：

```bash
sudo update-alternatives --config g++
```

显示：

```bash
有 2 个候选项可用于替换 g++ (提供 /usr/bin/g++)。

  选择       路径          优先级  状态
------------------------------------------------------------
* 0            /usr/bin/g++-9   20        自动模式
  1            /usr/bin/g++-8   10        手动模式
  2            /usr/bin/g++-9   20        手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：1（对的我摁的1,就是我）
update-alternatives: 使用 /usr/bin/g++-8 来在手动模式中提供 /usr/bin/g++ (g++)
```

### 8.编译样例

将`gcc`与`g++`都切换成gcc-8 g++-8后就可以在样例目录下输入`make`编译

编译完成后，生成的bin文件夹里找到deviceQuery 那一级目录，输入

```bash
./deviceQuery
```

看到类似的即为成功安装：

```bash
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "GeForce 940M"
  CUDA Driver Version / Runtime Version          10.1 / 10.1
  CUDA Capability Major/Minor version number:    5.0
  Total amount of global memory:                 2004 MBytes (2101870592 bytes)
  ( 3) Multiprocessors, (128) CUDA Cores/MP:     384 CUDA Cores
  GPU Max Clock rate:                            1176 MHz (1.18 GHz)
  Memory Clock rate:                             1001 Mhz
  Memory Bus Width:                              64-bit
  L2 Cache Size:                                 1048576 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(65536), 2D=(65536, 65536), 3D=(4096, 4096, 4096)
  Maximum Layered 1D Texture Size, (num) layers  1D=(16384), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(16384, 16384), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 1 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Compute Preemption:            No
  Supports Cooperative Kernel Launch:            No
  Supports MultiDevice Co-op Kernel Launch:      No
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 10.1, CUDA Runtime Version = 10.1, NumDevs = 1
Result = PASS
```

## cuDNN

下面是开始阅读[官方文档](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/)时间

### 1.下载

> ### [2.2. Downloading cuDNN For Linux](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/#download)
>
> In order to download cuDNN, ensure you are registered for the [NVIDIA Developer Program](https://developer.nvidia.com/accelerated-computing-developer).
>
> 1. Go to: [NVIDIA cuDNN home page](https://developer.nvidia.com/cudnn).
> 2. Click **Download**.
> 3. Complete the short survey and click **Submit**.
> 4. Accept the Terms and Conditions. A list of available download versions of cuDNN displays.
> 5. Select the cuDNN version you want to install. A list of available resources displays.

这个就是`cuDNN`的下载。

[cuDNN](https://developer.nvidia.com/rdp/cudnn-download)与CUDA是存在对应关系的。我们安装的是CUDA10.1版本，对应的是cuDNN v7.6.5，对应关系在下载页面会给出,在CUDA已经安装好的情况下，我们下载[cuDNN Library for Linux](https://developer.nvidia.com/compute/machine-learning/cudnn/secure/7.6.5.32/Production/10.1_20191031/cudnn-10.1-linux-x64-v7.6.5.32.tgz)的版本即可。

### 2.安装注意事项

>### [2.3. Installing cuDNN On Linux](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/#installlinux)
>
>The following steps describe how to build a cuDNN dependent program. Choose the installation method that meets your environment needs. For example, the tar file installation applies to all Linux platforms, and the debian installation package applies to Ubuntu 14.04,16.04, and 18.04.
>
>In the following sections:
>
>- your CUDA directory path is referred to as /usr/local/cuda/
>- your cuDNN download path is referred to as <cudnnpath>

上面提示我们，`CUDA`的安装位置在`/usr/local/cuda/`的话，我们`cuDNN`的安装位置应该也在那里

### 3.开始安装

> ### [2.3.1. Installing From A Tar File](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/#installlinux-tar)
>
> 
>
> 1. Navigate to your <cudnnpath> directory containing the cuDNN Tar file.
>
> 2. Unzip the cuDNN package.
>
>    ```bash
>    $ tar -xzvf cudnn-10.2-linux-x64-v7.6.5.32.tgz
>    ```
>
> 3. Copy the following files into the CUDA Toolkit directory, and change the file permissions.
>
>    ```bash
>    $ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
>    $ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
>    $ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
>    ```

我们安装的是`tar`包

首先将这个包放到`cuda`文件夹中，接着我们开始解压，如果权限不够那就前面加`sudo`

```bash
tar -xzvf cudnn-10.2-linux-x64-v7.6.5.32.tgz
```

解压后这个文件夹里会出现一个名为`cuda`的文件夹。

我们打开进入这个文件夹。

进入后，注意此时的路径是:`/usr/local/cuda/cuda`。`cuda`里面的`cuda`文件夹就是`cuDNN`解压出来的文件夹

我们开始复制文件

```bash
$ sudo cp include/cudnn.h /usr/local/cuda/include
$ sudo cp lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

这样就安装成功了

