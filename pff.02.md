# 第2章 Python基础设施  
  
> 建房子时，存在木材的选择问题。木匠的重中之重在于携带能切削良好的工具，并在有空时打磨这些工具。  
> ——宫本武藏（《五轮书》）  
  
对于Python新手来说，Python的部署似乎一点也不简单。对于可以选择安装的丰富库和包来说也是如此。首先，不仅仅只有一个Python。Python有许多不同的变体，比如CPython、Jython、IronPython和PyPy。其次是Python 2.7与3.x世界之间的鸿沟。[^1]  
  
即使你决定了版本，由于以下几个额外原因，部署依然很困难：  
  
- 解释器（标准的CPython安装）仅附带所谓的标准库（例如，涵盖典型的数学函数）  
- 可选的Python包需要单独安装——而这样的包有成百上千个  
- 由于依赖关系和特定操作系统的要求，自行编译/构建这些非标准包可能会很棘手  
- 随着时间的推移，处理这些依赖关系和版本一致性（即维护）往往既繁琐又耗时  
- 某些包的更新和升级可能需要重新编译大量的其他包  
- 更改或替换一个包可能会在（许多）其他地方引发问题  
  
幸运的是，现有的一些工具和策略可以提供帮助。本章涵盖了以下有助于Python部署的技术类型：  
  
**包管理器**  
  
像pip和conda这样的包管理器有助于安装、更新和删除Python包；它们还有助于保持不同包的版本一致性。  
  
**虚拟环境管理器**  
  
像virtualenv或conda这样的虚拟环境管理器允许你并行管理多个Python安装（例如，在单台机器上同时拥有Python 2.7和3.7的安装，或者在没有风险的情况下测试某个精美Python包的最新开发版本）。[^2]  
  
**容器**  
  
Docker容器代表了完整的文件系统，包含了运行特定软件（如代码、运行时或系统工具）所需系统的所有部分。例如，你可以在运行macOS或Windows 10的机器上托管的Docker容器中，运行带有Python 3.7安装及相应Python代码的Ubuntu 18.04操作系统。  
  
**云实例**  
  
为金融应用程序部署Python代码通常需要高可用性、安全性和高性能；这些要求通常只能通过使用专业的计算和存储基础设施来满足，如今这些基础设施以极具吸引力的条件，以从相当小到非常大且强大的云实例形式提供。与长期租赁的专用服务器相比，云实例（即虚拟服务器）的一个好处是，用户通常只需为实际使用的小时数付费；另一个优势是，如果需要，此类云实例几乎可以在一两分钟内准备就绪，这有助于敏捷开发以及可扩展性。  
  
本章的结构如下：  
  
“conda作为包管理器”在第35页  
  
本节介绍conda作为Python的包管理器。  
  
“conda作为虚拟环境管理器”在第41页  
  
本节重点介绍conda作为虚拟环境管理器的功能。  
  
“使用Docker容器”在第45页  
  
本节简要概述了Docker作为一种容器化技术，并重点介绍如何构建带有Python 3.7安装的基于Ubuntu的容器。  
  
“使用云实例”在第50页  
  
本节展示了如何在云端部署Python和Jupyter Notebook——一个强大的、基于浏览器的Python开发工具套件。  
  
本章的目标是在专业的基础设施上建立一个合适的Python安装，包含最重要的工具以及数值计算、数据分析和可视化包。这一组合将成为在后续章节中实现和部署Python代码的支柱，无论是交互式金融分析代码，还是脚本和模块形式的代码。  
  
## conda作为包管理器  
  
虽然conda可以独立安装，但通过Miniconda（一个包含conda作为包管理器和虚拟环境管理器的最小Python发行版）进行安装是一种高效的方式。  
  
### 安装Miniconda  
  
Miniconda提供Windows、macOS和Linux版本。你可以从Miniconda网页下载不同版本。在下文中，假设使用的是Python 3.7 64位版本。本节的主要示例是在基于Ubuntu的Docker容器中的一个会话，该会话通过wget下载Linux 64位安装程序，然后安装Miniconda。此处展示的代码也应该能在任何其他基于Linux或macOS的机器上运行——可能只需进行微小的修改：  
  
```python  
docker run -ti -h py4fi -p 11111:11111 ubuntu:latest /bin/bash  
apt-get update; apt-get upgrade -y  
```  
  
```txt  
...  
```  
  
```python  
apt-get install -y bzip2 gcc wget  
```  
  
```txt  
...  
```  
  
```python  
cd root  
wget \  
https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh \  
-O miniconda.sh  
```  
  
```txt  
...  
HTTP request sent, awaiting response... 200 OK  
Length: 62574861 (60M) [application/x-sh]  
Saving to: 'miniconda.sh'  
  
miniconda.sh             100%[====================>]  59.68M  5.97MB/s   in 11s  
  
2018-09-15 09:44:28 (5.42 MB/s) - 'miniconda.sh' saved [62574861/62574861]  
```  
  
```python  
bash miniconda.sh  
```  
  
```txt  
Welcome to Miniconda3 4.5.11  
  
In order to continue the installation process, please review the license  
agreement.  
Please, press ENTER to continue  
>>>   
```  
  
只需按回车键即可开始安装过程。查看许可协议后，通过回答yes来批准条款：  
  
```txt  
...  
Do you accept the license terms? [yes|no]  
[no] >>> yes  
  
Miniconda3 will now be installed into this location:  
/root/miniconda3  
  
  - Press ENTER to confirm the location  
  - Press CTRL-C to abort the installation  
  - Or specify a different location below  
  
[/root/miniconda3] >>>   
PREFIX=/root/miniconda3  
installing: python-3.7. ...  
...  
installing: requests-2.19.1-py37_0 ...  
installing: conda-4.5.11-py37_0 ...  
installation finished.  
```  
  
同意许可条款并确认安装位置后，你应该允许Miniconda将新的Miniconda安装位置添加到PATH环境变量的最前面，再次回答yes：  
  
```txt  
Do you wish the installer to prepend the Miniconda3 install location  
to PATH in your /root/.bashrc ? [yes|no]  
[no] >>> yes  
  
Appending source /root/miniconda3/bin/activate to /root/.bashrc  
A backup will be made to: /root/.bashrc-miniconda3.bak  
  
For this change to become active, you have to open a new terminal.  
  
Thank you for installing Miniconda3!  
```  
  
之后，你可能希望同时升级conda和Python：[^3]  
  
```python  
export PATH="/root/miniconda3/bin/:$PATH"  
conda update -y conda python  
```  
  
```txt  
...  
```  
  
```python  
echo ". /root/miniconda3/etc/profile.d/conda.sh" >> ~/.bashrc  
bash  
```  
  
经过这个相对简单的安装过程，你将拥有一个基本的Python安装，并且可以使用conda。基本的Python安装自带了一些好用的内置功能，比如SQLite3数据库引擎。在将相关路径附加到相应的环境变量（如前所述）后，你可以尝试在一个新的shell实例中是否可以启动Python：  
  
```python  
python  
```  
  
```txt  
Python 3.7.0 (default, Jun 28 2018, 13:15:42)  
[GCC 7.2.0] :: Anaconda, Inc. on linux  
Type "help", "copyright", "credits" or "license" for more information.  
```  
  
```python  
print('Hello Python for Finance World.')  
```  
  
```txt  
Hello Python for Finance World.  
```  
  
```python  
exit()  
```  
  
### conda的基本操作  
  
除了其他功能外，conda可以有效地用于处理Python包的安装、更新和删除。以下列表概述了其主要功能：  
  
安装Python x.x  
  
```python  
conda install python=x.x  
```  
  
更新Python  
  
```python  
conda update python  
```  
  
安装一个包  
  
```python  
conda install $PACKAGE_NAME  
```  
  
更新一个包  
  
```python  
conda update $PACKAGE_NAME  
```  
  
删除一个包  
  
```python  
conda remove $PACKAGE_NAME  
```  
  
更新conda本身  
  
```python  
conda update conda  
```  
  
搜索包  
  
```python  
conda search $SEARCH_TERM  
```  
  
列出已安装的包  
  
```python  
conda list  
```  
  
有了这些功能，例如安装NumPy——所谓的科学计算栈中最重要的库之一——只需要一条命令。当安装在具有英特尔处理器的机器上时，该程序会自动安装英特尔数学内核库（mkl），这不仅加快了NumPy的数值计算操作，也加快了一些其他科学Python包的运行速度：[^4]  
  
```python  
conda install numpy  
```  
  
```txt  
Solving environment: done  
  
## Package Plan ##  
  
  environment location: /root/miniconda3  
  
  added / updated specs:  
    - numpy  
  
The following packages will be downloaded:  
  
    package                    |            build  
    ---------------------------|-----------------  
    mkl-2019.0                 |              117       204.4 MB  
    intel-openmp-2019.0        |              117         721 KB  
    mkl_random-1.0.1           |   py37h4414c95_1         372 KB  
    libgfortran-ng-7.3.0       |       hdf63c60_0         1.3 MB  
    numpy-1.15.1               |   py37h1d66e8a_0          37 KB  
    numpy-base-1.15.1          |   py37h81de0dd_0         4.2 MB  
    blas-1.0                   |              mkl           6 KB  
    mkl_fft-1.0.4              |   py37h4414c95_1         149 KB  
    ------------------------------------------------------------  
                                           Total:       211.1 MB  
  
The following NEW packages will be INSTALLED:  
  
    blas:             1.0-mkl  
    intel-openmp:     2019.0-117  
    libgfortran-ng:   7.3.0-hdf63c60_0  
    mkl:              2019.0-117  
    mkl_fft:          1.0.4-py37h4414c95_1  
    mkl_random:       1.0.1-py37h4414c95_1  
    numpy:            1.15.1-py37h1d66e8a_0  
    numpy-base:       1.15.1-py37h81de0dd_0  
  
Proceed ([y]/n)? y  
  
Downloading and Extracting Packages  
mkl-2019.0           | 204.4 MB | ####################################### | 100%   
...  
numpy-1.15.1         | 37 KB    | ####################################### | 100%   
numpy-base-1.15.1    | 4.2 MB   | ####################################### | 100%   
...  
```  
  
也可以一次安装多个包。-y标志表示所有（潜在的）问题都将自动回答为yes：  
  
```python  
conda install -y ipython matplotlib pandas pytables scikit-learn scipy  
```  
  
```txt  
...  
pytables-3.4.4       | 1.5 MB   | ####################################### | 100%   
kiwisolver-1.0.1     | 83 KB    | ####################################### | 100%   
icu-58.2             | 22.5 MB  | ####################################### | 100%   
Preparing transaction: done  
Verifying transaction: done  
Executing transaction: done  
```  
  
在执行了相应的安装过程之后，除了标准库之外，还可以使用一些最重要的数据分析金融库。这些包括：  
  
IPython  
  
一个改进的交互式Python shell  
  
matplotlib  
  
Python中的标准绘图库  
  
NumPy  
  
用于高效处理数值数组  
  
pandas  
  
用于管理表格数据，如金融时间序列数据  
  
PyTables  
  
HDF5库的Python封装器  
  
scikit-learn  
  
用于机器学习及相关任务的包  
  
SciPy  
  
科学计算类和函数的集合（作为依赖项安装）  
  
这为一般的数据分析以及金融分析提供了基本工具集。下一个示例使用IPython并通过NumPy绘制一组伪随机数：  
  
```python  
ipython  
```  
  
```txt  
Python 3.7.0 (default, Jun 28 2018, 13:15:42)   
Type 'copyright', 'credits' or 'license' for more information  
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help.  
```  
  
```python  
In [1]: import numpy as np  
  
In [2]: np.random.seed(100)  
  
In [3]: np.random.standard_normal((5, 4))  
```  
  
```txt  
Out[3]:   
array([[-1.74976547,  0.3426804 ,  1.1530358 , -0.25243604],  
       [ 0.98132079,  0.51421884,  0.22117967, -1.07004333],  
       [-0.18949583,  0.25500144, -0.45802699,  0.43516349],  
       [-0.58359505,  0.81684707,  0.67272081, -0.10441114],  
       [-0.53128038,  1.02973269, -0.43813562, -1.11831825]])  
```  
  
```python  
In [4]: exit  
```  
  
执行conda list将显示已安装了哪些包：  
  
```python  
conda list  
```  
  
```txt  
# packages in environment at /root/miniconda3:  
#  
# Name                    Version                   Build  Channel  
asn1crypto                0.24.0                   py37_0    
backcall                  0.1.0                    py37_0    
blas                      1.0                         mkl    
blosc                     1.14.4               hdbcaa40_0    
bzip2                     1.0.6                h14c3975_5    
...  
python                    3.7.0                hc3d631a_0    
...  
wheel                     0.31.1                   py37_0    
xz                        5.2.4                h14c3975_4    
yaml                      0.1.7                had09818_2    
zlib                      1.2.11               ha838bed_2    
```  
  
如果某个包不再需要，可以使用conda remove有效删除它：  
  
```python  
conda remove scikit-learn  
```  
  
```txt  
Solving environment: done  
  
## Package Plan ##  
  
  environment location: /root/miniconda3  
  
  removed specs:  
    - scikit-learn  
  
The following packages will be REMOVED:  
  
    scikit-learn: 0.19.1-py37hedc7406_0  
  
Proceed ([y]/n)? y  
  
Preparing transaction: done  
Verifying transaction: done  
Executing transaction: done  
```  
  
作为包管理器的conda已经非常有用。然而，当将虚拟环境管理加入其中时，其全部威力才得以体现。  
  
**简易包管理**  
  
使用conda作为包管理器使安装、更新和删除Python包成为一种愉快的体验。无需费心自行构建和编译包——考虑到包所指定的依赖项列表以及在不同操作系统上需要考虑的细节，这有时会很棘手。  
  
## conda作为虚拟环境管理器  
  
根据你选择的安装程序版本，Miniconda提供默认的Python 2.7或3.7安装。conda的虚拟环境管理功能允许你（例如）在Python 3.7的默认安装旁边添加一个完全独立的Python 2.7.x安装。为此，conda提供了以下功能：  
  
创建一个虚拟环境  
  
```python  
conda create --name $ENVIRONMENT_NAME  
```  
  
激活一个环境  
  
```python  
conda activate $ENVIRONMENT_NAME  
```  
  
停用一个环境  
  
```python  
conda deactivate $ENVIRONMENT_NAME  
```  
  
删除一个环境  
  
```python  
conda env remove --name $ENVIRONMENT_NAME  
```  
  
导出到一个环境文件  
  
```python  
conda env export > $FILE_NAME  
```  
  
从文件创建一个环境  
  
```python  
conda env create -f $FILE_NAME  
```  
  
列出所有环境  
  
```python  
conda info --envs  
```  
  
作为一个简单的说明，下面这段示例代码创建了一个名为py27的环境，安装了IPython，并执行了一行Python 2.7.x的代码：  
  
```python  
conda create --name py27 python=2.7  
```  
  
```txt  
Solving environment: done  
  
## Package Plan ##  
  
  environment location: /root/miniconda3/envs/py27  
  
  added / updated specs:  
    - python=2.7  
  
The following NEW packages will be INSTALLED:  
  
    ca-certificates: 2018.03.07-0            
...  
    python:          2.7.15-h1571d57_0       
...  
    zlib:            1.2.11-ha838bed_2       
  
Proceed ([y]/n)? y  
  
Preparing transaction: done  
Verifying transaction: done  
Executing transaction: done  
#  
# To activate this environment, use:  
# > conda activate py27  
#  
# To deactivate an active environment, use:  
# > conda deactivate  
#  
```  
  
请注意在激活环境后提示符是如何改变以包含(py27)的：  
  
```python  
conda activate py27  
```  
  
```txt  
(py27) root@py4fi:~#  
```  
  
```python  
conda install ipython  
```  
  
```txt  
Solving environment: done  
...  
Executing transaction: done  
```  
  
最终，这允许你使用Python 2.7语法的IPython：  
  
```python  
ipython  
```  
  
```txt  
Python 2.7.15 |Anaconda, Inc.| (default, May  1 2018, 23:32:55)   
Type "copyright", "credits" or "license" for more information.  
  
IPython 5.8.0 -- An enhanced Interactive Python.  
?         -> Introduction and overview of IPython's features.  
%quickref -> Quick reference.  
help      -> Python's own help system.  
object?   -> Details about 'object', use 'object??' for extra details.  
```  
  
```python  
In [1]: print "Hello Python for Finance World!"  
```  
  
```txt  
Hello Python for Finance World!  
```  
  
```python  
In [2]: exit  
```  
  
如本例所示，使用conda作为虚拟环境管理器允许你将不同的Python版本并排安装。它还允许你安装某些包的不同版本。默认的Python安装不受此类过程的影响，同一机器上可能存在的其他环境也不受影响。通过conda env list可以显示所有可用环境：  
  
```python  
conda env list  
```  
  
```txt  
# conda environments:  
#  
base                  /root/miniconda3  
py27               *  /root/miniconda3/envs/py27  
```  
  
有时需要与他人共享环境信息，或在多台机器上使用环境信息。为此，可以使用conda env export将已安装包列表导出到一个文件。默认情况下，由于在生成的YAML文件中指定了构建版本，这仅在机器使用相同操作系统时才能正常工作，但可以删除它们以仅指定包版本：  
  
```python  
conda env export --no-builds > py27env.yml  
cat py27env.yml  
```  
  
```txt  
name: py27  
channels:  
  - defaults  
dependencies:  
  - backports=1.0  
...  
  - python=2.7.15  
...  
  - zlib=1.2.11  
prefix: /root/miniconda3/envs/py27  
```  
  
通常，创建一个虚拟环境（在技术上只不过是一个特定的（子）文件夹结构）只是为了做一些快速测试。[^5]在这样的情况下，在停用后，很容易通过conda env remove移除环境：  
  
```python  
conda deactivate  
conda env remove -y --name py27  
```  
  
```txt  
Remove all packages in environment /root/miniconda3/envs/py27:  
  
## Package Plan ##  
  
  environment location: /root/miniconda3/envs/py27  
  
The following packages will be REMOVED:  
  
    backports: 1.0-py27_1  
...  
    zlib:      1.2.11-ha838bed_2  
```  
  
这就结束了对conda作为虚拟环境管理器的概述。  
  
**简易环境管理**  
  
conda不仅有助于管理包；它还是Python的虚拟环境管理器。它简化了不同Python环境的创建，允许你在同一台机器上拥有多个版本的Python和可选包，而它们之间不会产生任何影响。conda还允许你导出环境信息，因此你可以轻松地在多台机器上复制它，或与他人分享。  
  
## 使用Docker容器  
  
Docker容器已席卷IT界。尽管这项技术还相对年轻，但它已经成为高效开发和部署几乎任何种类软件应用程序的基准之一。  
  
出于本书的目的，可以简单地将Docker容器视为一个独立的（“容器化的”）文件系统，其中包含操作系统（例如，Ubuntu Server 18.04）、（Python）运行时、额外的系统和开发工具，以及根据需要添加的更多（Python）库和包。这样一个Docker容器可以运行在具有Windows 10的本地机器上，也可以运行在具有Linux操作系统的云实例上等。  
  
本节不深入探讨Docker容器的所有激动人心的细节。它只是简要说明Docker技术在Python部署环境中能做什么。[^6]  
  
### Docker镜像与容器  
  
然而，在继续说明之前，在讨论Docker时必须区分两个基本概念。第一个是Docker镜像（Docker image），它可以类比为Python中的类。第二个是Docker容器（Docker container），它可以类比为相应Python类的实例。[^7]  
  
在更具技术性的层面上，你可以在Docker词汇表中找到关于镜像（image）的以下定义：  
  
> Docker镜像是容器的基础。镜像是一个有序的根文件系统更改集合，以及用于容器运行时内的相应执行参数。镜像通常包含叠加在一起的层叠文件系统的联合。镜像没有状态，且永远不会改变。  
  
同样地，你会在Docker词汇表中找到关于容器（container）的以下定义，这使得与Python类及其实例的类比变得透明：  
  
> 容器是Docker镜像的运行时实例。一个Docker容器包含：一个Docker镜像、一个执行环境和一组标准指令。  
  
根据操作系统的不同，Docker的安装略有不同。这就是为什么本节不深入细节的原因。更多信息和进一步的链接可以在关于Docker CE的页面上找到。  
  
### 构建Ubuntu和Python Docker镜像  
  
本节说明了构建一个基于最新版本Ubuntu的Docker镜像的过程，其中包含Miniconda以及几个重要的Python包。此外，它通过更新Linux包索引、在必要时升级包，以及安装某些额外的系统工具来完成一些Linux环境配置工作。为此，需要两个脚本。一个是完成Linux层面所有工作的bash脚本。[^8]另一个是所谓的Dockerfile，它控制镜像本身的构建过程。  
  
示例 2-1 中进行安装的bash脚本由三个主要部分组成。第一部分处理Linux环境配置。第二部分安装Miniconda，而第三部分安装可选的Python包。代码中也有更详细的内联注释。  
  
示例 2-1. 安装Python和可选包的脚本  
  
```python  
#!/bin/bash  
#  
# 安装以下内容的脚本  
# Linux系统工具和  
# 基本Python组件  
#  
# Python for Finance, 2nd ed.  
# (c) Dr. Yves J. Hilpisch  
#  
# 常规LINUX  
apt-get update # 更新包索引缓存  
apt-get upgrade -y # 更新包  
# 安装系统工具  
apt-get install -y bzip2 gcc git htop screen vim wget  
apt-get upgrade -y bash # 如有必要，升级bash  
apt-get clean # 清理包索引缓存  
  
# 安装MINICONDA  
# 下载Miniconda  
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O \  
 Miniconda.sh  
bash Miniconda.sh -b # 安装它  
rm -rf Miniconda.sh # 移除安装程序  
export PATH="/root/miniconda3/bin:$PATH" # 将新路径添加到最前面  
  
# 安装PYTHON库  
conda update -y conda python # 更新conda和Python（如果需要）  
conda install -y pandas # 安装pandas  
conda install -y ipython # 安装IPython shell  
```  
  
示例 2-2 中的Dockerfile使用示例 2-1 中的bash脚本来构建一个新的Docker镜像。它同样通过内联注释对其主要部分进行了解释。  
  
示例 2-2. 构建镜像的Dockerfile  
  
```python  
#  
# 构建带有  
# 最新Ubuntu版本和  
# 基本Python安装的Docker镜像  
#  
# Python for Finance, 2nd ed.  
# (c) Dr. Yves J. Hilpisch  
#  
# 最新Ubuntu版本  
FROM ubuntu:latest  
  
# 维护者信息  
MAINTAINER yves  
  
# 添加bash脚本  
ADD install.sh /  
  
# 更改脚本权限  
RUN chmod u+x /install.sh  
  
# 运行bash脚本  
RUN /install.sh  
  
# 将新路径添加到最前面  
ENV PATH /root/miniconda3/bin:$PATH  
  
# 运行容器时执行IPython  
CMD ["ipython"]  
```  
  
如果这两个文件在同一个文件夹中并且已经安装了Docker，那么构建新的Docker镜像非常直接。这里，标签 py4fi:basic 用于该镜像。这个标签用于引用镜像，例如在基于它运行容器时：  
  
```python  
docker build -t py4fi:basic .  
```  
  
```txt  
...  
Removing intermediate container 5fec0c9b2239  
 ---> accee128d9e9  
Step 6/7 : ENV PATH /root/miniconda3/bin:$PATH  
 ---> Running in a2bb97686255  
Removing intermediate container a2bb97686255  
 ---> 73b00c215351  
Step 7/7 : CMD ["ipython"]  
 ---> Running in ec7acd90c991  
Removing intermediate container ec7acd90c991  
 ---> 6c36b9117cd2  
Successfully built 6c36b9117cd2  
Successfully tagged py4fi:basic  
```  
  
通过docker images可以列出现有的Docker镜像。新镜像应该位于列表的顶部：  
  
```python  
docker images  
```  
  
```txt  
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE  
py4fi               basic               6c36b9117cd2        About a minute ago  1.79GB  
ubuntu              latest              cd6d8154f1e1        9 days ago          84.1MB  
```  
  
成功构建了 py4fi:basic 后，你可以通过docker run来运行相应的Docker容器。参数组合 -ti 是用于在Docker容器内运行交互式进程所必需的，比如shell进程（参见docker run参考页面）：  
  
```python  
docker run -ti py4fi:basic  
```  
  
```txt  
Python 3.7.0 (default, Jun 28 2018, 13:15:42)   
Type 'copyright', 'credits' or 'license' for more information  
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help.  
```  
  
```python  
In [1]: import numpy as np  
  
In [2]: a = np.random.standard_normal((5, 3))  
  
In [3]: import pandas as pd  
  
In [4]: df = pd.DataFrame(a, columns=['a', 'b', 'c'])  
  
In [5]: df  
```  
  
```txt  
Out[5]:   
          a         b         c  
0 -1.412661 -0.881592  1.704623  
1 -1.294977  0.546676  1.027046  
2  1.156361  1.979057  0.989772  
3  0.546736 -0.479821  0.693907  
4 -1.972943 -0.193964  0.769500  
```  
  
```python  
In [6]:  
```  
  
退出IPython也会退出容器，因为它是容器内运行的唯一应用程序。然而，你可以通过输入 Ctrl-P + Ctrl-Q 从容器中分离（detach）。  
  
从它分离后，docker ps 命令仍然会显示该正在运行的容器（以及任何其他当前正在运行的容器）：  
  
```python  
docker ps  
```  
  
```txt  
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS  
e815df8f0f4d        py4fi:basic         "ipython"           About a minute ago  Up About a minute  
4518917de7dc        ubuntu:latest       "/bin/bash"         About an hour ago   Up About an hour  
d081b5c7add0        ubuntu:latest       "/bin/bash"         21 hours ago        Up 21 hours  
```  
  
附加到一个Docker容器可以通过命令 docker attach $CONTAINER_ID 来完成（请注意，只需提供 $CONTAINER_ID 的前几个字母即可）：  
  
```python  
docker attach e815d  
```  
  
```python  
In [6]: df.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 5 entries, 0 to 4  
Data columns (total 3 columns):  
a    5 non-null float64  
b    5 non-null float64  
c    5 non-null float64  
dtypes: float64(3)  
memory usage: 200.0 bytes  
```  
  
```python  
In [7]: exit  
```  
  
exit命令会终止IPython并停止Docker容器。可以使用docker rm将其移除：  
  
```python  
docker rm e815d  
```  
  
```txt  
e815d  
```  
  
同样地，如果不再需要，可以通过 docker rmi 移除Docker镜像 py4fi:basic。虽然容器相对轻量，但单个镜像可能会消耗相当多的存储空间。对于 py4fi:basic 镜像来说，其大小接近2 GB。这就是为什么你可能需要定期清理Docker镜像列表的原因：  
  
```python  
docker rmi 6c36b9117cd2  
```  
  
当然，关于Docker容器及其在某些应用场景中的优势还有很多可以说。但对于本书的目的来说，知道它们提供了一种现代的方法来部署Python，能在完全独立（容器化）的环境中进行Python开发，并且能够为算法交易打包交付代码，这就足够了。  
  
**Docker容器的优势**  
  
如果你还没有使用Docker容器，你应该考虑开始使用。在Python部署和开发工作中，它们提供了许多好处，不仅仅是在本地工作时，尤其是在使用远程云实例和部署算法交易代码的服务器时。  
  
## 使用云实例  
  
本节展示了如何在DigitalOcean云实例上设置完整的Python基础设施。外面有许多其他的云提供商，包括领先的提供商亚马逊网络服务（AWS）。然而，DigitalOcean以其简单性以及其较小的云实例（称为Droplets）相对低廉的费用而闻名。通常足以用于探索和开发目的的最小Droplet每月只需5美元，或每小时0.007美元。使用量按小时计费，因此你可以轻松启动一个Droplet，假设使用2小时后将其销毁，最终仅被收取0.014美元。[^9]  
  
本节的目标是在DigitalOcean上设置一个包含Python 3.7安装以及通常需要的包（如NumPy、pandas）的Droplet，结合受密码保护和安全套接字层（SSL）加密的Jupyter Notebook服务器安装。该服务器安装将提供三个可以通过常规浏览器使用的主要工具：  
  
Jupyter Notebook  
  
一个流行的交互式开发环境，具有多种不同语言内核的选择（例如，针对Python、R和Julia）。  
  
Terminal  
  
可通过浏览器访问的系统shell实现，允许执行所有典型的系统管理任务以及使用诸如Vim和git之类的实用工具。  
  
Editor  
  
具有针对多种不同编程语言和文件类型的语法高亮显示的基于浏览器的文件编辑器，以及典型的文本/代码编辑功能。  
  
在Droplet上安装Jupyter Notebook允许你通过浏览器进行Python开发和部署，从而规避了通过安全外壳（SSH）访问登录云实例的需要。  
  
为了实现本节的目标，需要用到一些文件：  
  
服务器设置脚本  
  
该脚本协调所有必要的步骤，比如，将其他文件复制到Droplet并在Droplet上运行它们。  
  
Python和Jupyter安装脚本  
  
这将安装Python、其他包和Jupyter Notebook，并启动Jupyter Notebook服务器。  
  
Jupyter Notebook配置文件  
  
此文件用于Jupyter Notebook服务器的配置，例如，关于密码保护方面。  
  
RSA公钥和私钥文件  
  
这两个文件是Jupyter Notebook服务器进行SSL加密所必需的。  
  
以下小节将按这个文件列表倒序进行讲解。  
  
### RSA公钥和私钥  
  
为了通过任意浏览器与Jupyter Notebook服务器建立安全连接，需要一个由RSA公钥和私钥组成的SSL证书。通常，人们会期望这种证书来自所谓的证书颁发机构（CA）。然而，对于本书而言，一个自生成的证书就“足够好”了。[^10]生成RSA密钥对的流行工具是OpenSSL。接下来的简短交互式会话将展示如何生成适用于Jupyter Notebook服务器的证书（在提示后插入你自己的国家名称和其他字段的值）：  
  
```python  
openssl req -x509 -nodes -days 365 -newkey \  
rsa:1024 -out cert.pem -keyout cert.key  
```  
  
```txt  
Generating a 1024 bit RSA private key  
..++++++  
.......++++++  
writing new private key to 'cert.key'  
```  
  
你将被要求输入一些信息，这些信息将并入你的证书请求中。你将要输入的内容被称为专有名称（DN）。有很多字段，但你可以将某些字段留空，而其他字段将使用默认值。如果你输入 `.` ，该字段将留空。  
  
```txt  
Country Name (2 letter code) [AU]:  
```  
  
```python  
DE  
```  
  
```txt  
State or Province Name (full name) [Some-State]:  
```  
  
```python  
Saarland  
```  
  
```txt  
Locality Name (eg, city) []:  
```  
  
```python  
Voelklingen  
```  
  
```txt  
Organization Name (eg, company) [Internet Widgits Pty Ltd]:  
```  
  
```python  
TPQ GmbH  
```  
  
```txt  
Organizational Unit Name (eg, section) []:  
```  
  
```python  
Python for Finance  
```  
  
```txt  
Common Name (e.g. server FQDN or YOUR name) []:  
```  
  
```python  
Jupyter  
```  
  
```txt  
Email Address []:  
```  
  
```python  
team@tpq.io  
```  
  
```python  
ls  
```  
  
```txt  
cert.key  cert.pem  
```  
  
这两个文件 cert.key 和 cert.pem 需要被复制到Droplet，并需要在Jupyter Notebook配置文件中被引用。该文件接下来会介绍。  
  
### Jupyter Notebook配置文件  
  
可以通过遵循文档中的说明来安全部署公开的Jupyter Notebook服务器。除了其他功能外，Jupyter Notebook可以受密码保护。为此，在 notebook.auth 子包中提供了一个名为 passwd() 的生成密码哈希代码的函数。以下代码生成一个将 jupyter 本身作为密码的哈希代码：  
  
```python  
ipython  
```  
  
```txt  
Python 3.7.0 (default, Jun 28 2018, 13:15:42)   
Type 'copyright', 'credits' or 'license' for more information  
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help.  
```  
  
```python  
In [1]: from notebook.auth import passwd  
  
In [2]: passwd('jupyter')  
```  
  
```txt  
Out[2]: 'sha1:d4d34232ac3a:55ea0ffd78cc3299e3e5e6ecc0d36be0935d424b'  
```  
  
```python  
In [3]: exit  
```  
  
如示例 2-3 所示，此哈希代码需要放入Jupyter Notebook配置文件中。该配置文件假设RSA密钥文件已复制到Droplet上的 /root/.jupyter/ 文件夹中。  
  
示例 2-3. Jupyter Notebook配置文件  
  
```python  
#  
# Jupyter Notebook 配置文件  
#  
# Python for Finance, 2nd ed.  
# (c) Dr. Yves J. Hilpisch  
#  
# SSL加密  
# 用您选择的文件/文件名替换以下文件名（以及使用的文件）  
c.NotebookApp.certfile = u'/root/.jupyter/cert.pem'  
c.NotebookApp.keyfile = u'/root/.jupyter/cert.key'  
  
# IP地址和端口  
# 将 ip 设置为 '*' 以绑定到云实例的所有 IP 地址  
c.NotebookApp.ip = '*'  
# 为服务器访问设置一个已知的固定默认端口是个好主意  
c.NotebookApp.port = 8888  
  
# 密码保护  
# 这里：'jupyter' 作为密码  
# 用您的强密码的哈希代码替换此哈希代码  
c.NotebookApp.password = 'sha1:d4d34232ac3a:55ea0ffd78cc3299e3e5e6ecc0d36be0935d424b'  
  
# 无浏览器选项  
# 防止 Jupyter 尝试打开浏览器  
c.NotebookApp.open_browser = False  
```  
  
**Jupyter与安全**  
  
在云端部署Jupyter Notebook主要会导致一系列安全问题，因为它是一个可以通过Web浏览器访问的完整开发环境。因此，使用Jupyter Notebook服务器默认提供的安全措施，如密码保护和SSL加密，是极其重要的。但这仅仅是一个开始；根据在云实例上究竟进行了什么操作，可能还需要进一步的安全措施。  
  
下一步是确保Python和Jupyter Notebook已安装在Droplet上。  
  
### Python和Jupyter Notebook的安装脚本  
  
用于安装Python和Jupyter Notebook的bash脚本类似于在第45页“使用Docker容器”中通过Miniconda在Docker容器中安装Python的脚本。但是，示例 2-4 中的脚本还需要启动Jupyter Notebook服务器。所有主要部分和代码行都带有内联注释。  
  
示例 2-4. 用于安装Python并运行Jupyter Notebook服务器的bash脚本  
  
```python  
#!/bin/bash  
#  
# 安装以下内容的脚本  
# Linux系统工具，  
# 基本Python包和  
# Jupyter Notebook服务器  
#  
# Python for Finance, 2nd ed.  
# (c) Dr. Yves J. Hilpisch  
#  
# 常规LINUX  
apt-get update # 更新包索引缓存  
apt-get upgrade -y # 更新包  
apt-get install -y bzip2 gcc git htop screen vim wget # 安装系统工具  
apt-get upgrade -y bash # 如有必要，升级bash  
apt-get clean # 清理包索引缓存  
  
# 安装MINICONDA  
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O \  
 Miniconda.sh  
bash Miniconda.sh -b # 安装Miniconda  
rm Miniconda.sh # 移除安装程序  
# 为当前会话将新路径添加到最前面  
export PATH="/root/miniconda3/bin:$PATH"  
# 在shell配置文件中将新路径添加到最前面  
echo ". /root/miniconda3/etc/profile.d/conda.sh" >> ~/.bashrc  
echo "conda activate" >> ~/.bashrc  
  
# 安装PYTHON库  
# 可以/必须添加更多包  
# 视用例而定。  
conda update -y conda # 如果需要，更新conda  
conda create -y -n py4fi python=3.7 # 创建一个环境  
source activate py4fi # 激活新环境  
conda install -y jupyter # 浏览器中的交互式数据分析  
conda install -y pytables # HDFS二进制存储的封装器  
conda install -y pandas # 数据分析包  
conda install -y matplotlib # 标准绘图库  
conda install -y scikit-learn # 机器学习库  
conda install -y openpyxl # 用于Excel交互的库  
conda install -y pyyaml # 管理YAML文件的库  
  
pip install --upgrade pip # 升级包管理器  
pip install cufflinks # 将plotly与pandas结合  
  
# 复制文件和创建目录  
mkdir /root/.jupyter  
mv /root/jupyter_notebook_config.py /root/.jupyter/  
mv /root/cert.* /root/.jupyter  
mkdir /root/notebook  
cd /root/notebook  
  
# 启动JUPYTER NOTEBOOK  
jupyter notebook --allow-root  
  
# 启动JUPYTER NOTEBOOK  
# 作为后台进程：  
# jupyter notebook --allow-root &  
```  
  
此脚本需要复制到Droplet，并需要由下一小节中所述的协调脚本来启动。  
  
### 协调Droplet设置的脚本  
  
设置Droplet的第二个bash脚本是最短的一个（示例 2-5）。它主要是将所有其他文件复制到Droplet，它的IP地址作为参数输入。在最后一行，它启动了 install.sh 这个bash脚本，该脚本随后执行安装本身并启动Jupyter Notebook服务器。  
  
示例 2-5. 用于设置Droplet的bash脚本  
  
```python  
#!/bin/bash  
#  
# 设置DigitalOcean Droplet  
# 带有基本Python技术栈  
# 和Jupyter Notebook  
#  
# Python for Finance, 2nd ed.  
# (c) Dr Yves J Hilpisch  
#  
# 来自参数的IP地址  
MASTER_IP=$1  
  
# 复制文件  
scp install.sh root@${MASTER_IP}:  
scp cert.* jupyter_notebook_config.py root@${MASTER_IP}:  
  
# 执行安装脚本  
ssh root@${MASTER_IP} bash /root/install.sh  
```  
  
现在一切就绪，可以尝试运行设置代码了。在DigitalOcean上，使用类似以下的选项创建一个新的Droplet：  
  
操作系统  
  
Ubuntu 18.10 x64（编写本书时的最新可用版本）  
  
大小  
  
1核，1 GB，25 GB SSD（最小的Droplet）  
  
数据中心区域  
  
法兰克福（因为您的作者住在德国）  
  
SSH密钥  
  
添加一个（新）SSH密钥用于免密登录 [^11]  
  
Droplet名称  
  
您可以使用预先指定的名称，也可以选择诸如 py4fi 之类的名称  
  
点击“Create”按钮即可启动Droplet创建过程，通常大约需要一分钟。设置过程的主要结果是获取到IP地址，如果您选择法兰克福作为数据中心位置，例如它可能是 46.101.156.199。现在，按如下方式设置Droplet非常简单：  
  
```python  
bash setup.sh 46.101.156.199  
```  
  
所产生的过程可能需要几分钟。当出现Jupyter Notebook服务器发出类似以下的提示消息时，即表示完成：  
  
```txt  
The Jupyter Notebook is running at: https://[all ip addresses on your  
system]:8888/  
```  
  
在任何现代浏览器中，访问以下地址即可访问正在运行的Jupyter Notebook服务器（注意 https 协议）：  
  
https://46.101.156.199:8888  
  
在可能请求您添加安全例外之后，应出现提示输入密码的Jupyter Notebook登录屏幕（在我们的案例中，密码为 jupyter）。现在您已准备好在浏览器中通过Jupyter Notebook、终端窗口中的IPython或文本文件编辑器开始Python开发了。也可以使用其他文件管理功能，如上传文件、删除文件和创建文件夹。  
  
**云的优势**  
  
来自DigitalOcean的云实例和Jupyter Notebook是一个强大的组合，允许Python开发者和量化分析师在专业计算和存储基础设施上工作并加以利用。专业云和数据中心提供商能确保您的（虚拟）机器的物理安全性及高可用性。使用云实例也能将探索和开发阶段的成本保持在较低水平，因为使用量通常按小时计费，无需签订长期协议。  
  
## 结论  
  
Python是不二之选的编程语言和技术平台，不仅对于本书如此，对于几乎每一个领先的金融机构也是如此。然而，Python部署充其量只是有些棘手，有时甚至极其繁琐且令人头疼。幸运的是，近年来已经出现了一些有助于解决部署问题的技术。开源的conda既有助于Python包的管理，也有助于虚拟环境的管理。Docker容器则更进一步，可以在一个技术上被隔离的“沙盒”（即容器）中轻松创建完整的文件系统和运行环境。更进一步，像DigitalOcean这样的云提供商能在几分钟内在由专业管理且安全的数据中心中提供计算和存储能力，并按小时计费。这与Python 3.7的安装和安全的Jupyter Notebook服务器安装相结合，为在Python金融项目环境下的Python开发和部署提供了专业的环境。  
  
## 更多资源  
  
关于Python包管理，请查阅以下资源：  
  
- pip包管理器页面  
- conda包管理器页面  
- 安装包页面  
  
关于虚拟环境管理，请查阅这些资源：  
  
- virtualenv环境管理器页面  
- conda管理环境页面  
- pipenv包和环境管理器  
  
以下资源（除其他资源外）提供了关于Docker容器的信息：  
  
- Docker主页  
- Matthias, Karl, and Sean Kane (2015). Docker: Up and Running. Sebastopol, CA: O’Reilly.  
  
有关bash脚本语言的简要介绍和概述，请参阅：  
  
- Robbins, Arnold (2016). Bash Pocket Reference. Sebastopol, CA: O’Reilly.  
  
如何在安全环境中运行公共Jupyter Notebook服务器在Jupyter Notebook文档中有所解释。还有一个可用的中心允许为Jupyter Notebook服务器管理多个用户，这被称为JupyterHub。  
  
要在DigitalOcean上注册并为新账户获得10美元初始额度，请访问页面 http://bit.ly/do_sign_up。这足以支付最小Droplet的两个月使用费。  
  
---  
  
[^1]: 本版基于CPython的3.7版本（编写时最新的主要发布版本），CPython是Python编程语言最初且最受欢迎的版本。  
[^2]: 最近一个名为pipenv的项目结合了包管理器pip和虚拟环境管理器virtualenv的功能。  
[^3]: Miniconda安装程序的更新频率通常不如conda和Python本身频繁。  
[^4]: 安装元包nomkl，例如通过conda install numpy nomkl，可避免自动安装和使用mkl及相关的其他包。  
[^5]: 在官方文档中你可以找到以下解释：“Python的‘虚拟环境’允许将Python包安装在一个隔离的位置以供特定应用程序使用，而不是进行全局安装。”  
[^6]: 有关Docker技术的全面介绍，请参阅Matthias和Kane（2015）。  
[^7]: 如果这些术语还不清楚，它们将在第6章中变得明朗。  
[^8]: 有关bash脚本的简明介绍和快速概述，请参阅Robbins（2016）。另请参阅 https://www.gnu.org/software/bash。  
[^9]: 通过此推荐链接注册的新用户可获得DigitalOcean的10美元初始额度。  
[^10]: 使用自生成的证书时，如果浏览器提示，您可能需要添加安全例外。  
[^11]: 如果您需要帮助，请访问“如何向Droplets添加SSH密钥”或“如何在Windows上使用PuTTY创建SSH密钥”。  
