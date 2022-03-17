## Python版本-OS版本

sys : python解释器
os ：操作系统接口  
platform ： 平台相关，与python版本、运行环境有关

import sys, os, platform as pf

### 关于32bit和64bit
cpu -> os -> python, 64位可以向下兼容32位，32位不可以向上兼容。

因此python 64 bit 必须装在64位系统，cpu也必然是64位。64 bit版本可以充分利用系统内存，32 bit每个进程可以利用2 GB.
值得注意，需要占用大量内存的应用场景，应选择64位。比如科学计算领域，作为较特殊的例子，tensorflow 只有64 bit版本。

python 32 bit使用兼容性更好，且部分库没有64位版本。需要加载dll文件时，32 bit版本也更容易获取。 另外32 bit版本占用
内存会相对较小。

### 测试平台介绍
- windows: win11 64位; cpu: 64位, Intel64 Family 6 Model 140 Stepping 1 GenuineIntel ~2419 Mhz  
    1. python 3.9.2 64 bit  
    2. python 3.7.3 32 bit
```
systeminfo
```
- linux: raspbian10 32位; cpu: 64位, Cortex-A72 (armv8, shown as armv7l) CM4 BCM2711
    1. python 3.7.3 32 bit  
```
## distro
# pi@raspberrypi:~ $ cat /etc/redhat-release
$ cat /etc/issue
Raspbian GNU/Linux 10 \n \l

$ lsb_release -a
No LSB modules are available.
Distributor ID: Raspbian
Description:    Raspbian GNU/Linux 10 (buster)
Release:        10
Codename:       buster

## kernel
$ uname -a
Linux raspberrypi 5.10.17-v7l+ #1414 SMP Fri Apr 30 13:20:47 BST 2021 armv7l GNU/Linux

$ cat /proc/version
Linux version 5.10.17-v7l+ (dom@buildbot) (arm-linux-gnueabihf-gcc-8 (Ubuntu/Linaro 8.4.0-3ubuntu1) 8.4.0
,GNU ld (GNU Binutils for Ubuntu) 2.34) #1414 SMP Fri Apr 30 13:20:47 BST 2021

## cpu
$ lscpu
Architecture:        armv7l
Byte Order:          Little Endian
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  1
Core(s) per socket:  4
Socket(s):           1
Vendor ID:           ARM
Model:               3
Model name:          Cortex-A72
Stepping:            r0p3
CPU max MHz:         1500.0000
CPU min MHz:         600.0000
BogoMIPS:            270.00
Flags:               half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32

$ cat /proc/cpuinfo
processor       : 0
...
processor       : 3
model name      : ARMv7 Processor rev 3 (v7l)
BogoMIPS        : 270.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

Hardware        : BCM2711
Revision        : b03140
Serial          : 10000000402bc35d
Model           : Raspberry Pi Compute Module 4 Rev 1.0

```

### python版本查看 
1. sys.version
```
'3.9.2 (tags/v3.9.2:1a79785, Feb 19 2021, 13:44:55) [MSC v.1928 64 bit (AMD64)]'
'3.7.3 (v3.7.3:ef4ec6ed12, Mar 25 2019, 21:26:53) [MSC v.1916 32 bit (Intel)]'
'3.7.3 (default, Jan 22 2021, 20:04:44) \n[GCC 8.3.0]'
```
2. pf.python_version()
```
3.9.2
3.7.3
3.7.3
```

### python位数查看
1. 64 if sys.maxsize >= 2**32 else 32  # maxsize = 2 ** (32|64-1) - 1)
```
9223372036854775807
2147483647
2147483647
```

2. struct.calcsize('P') * 8

### 平台位数（或可以看作python位数）
1. pf.architecture()  # 架构位数，可执行文件的链接格式。
```
('64bit', 'WindowsPE')
('32bit', 'WindowsPE')
('32bit', 'ELF')
```

### 平台名
1. sys.platform  # 平台标识符

System | Value
:--- | :--- 
AIX |'aix' 
Linux |'linux'
Windows |'win32'
Windows/Cygwin |'cygwin'
macOS |'darwin'

```
'win32' # 指的是win32 api, 即使是64位系统，这里也不会显示win64.
'win32'
'linux'
```

2. pf.platform()  # 用来标识潜在平台的尽可能多的信息
```
'Windows-10-10.0.22000-SP0'
'Windows-10-10.0.22000-SP0'
'Linux-5.10.17-v7l+-armv7l-with-debian-10.9'
```

### 操作系统名称
1. os.name  # posix or nt
```
nt
nt
posix
```

2. pf.system() # system/os name
```
Windows
Windows
Linux
```

### 操作系统位数(is_64)
1. platform.machine().endswith('64') # version >= 2.7
```
# Python version <= 2.6
def is_windows_64bit():
    if 'PROCESSOR_ARCHITEW6432' in os.environ:
        return True
    return os.environ['PROCESSOR_ARCHITECTURE'].endswith('64')
```
2. 'PROGRAMFILES(X86)' in os.environ  # windows 

### 机器类型(machine type)
1. pf.machine()
```
'AMD64'
'AMD64'
'armv7l'  # raspbian 32 bit os并未显示出真正的名称，Cortex-A72实际上应该是armv8.
```

### cpu真实名称
1. pf.processor()  # 注意很多系统并不提供这个数据，而只是给出空串或者与machine相同的值。
```
'Intel64 Family 6 Model 140 Stepping 1, GenuineIntel'
'Intel64 Family 6 Model 140 Stepping 1, GenuineIntel'
''
```

### uname
1. pf.uname (system, node, release, version, machine, processor)
```
uname_result(system='Windows', node='DESKTOP-9IJL8A3', release='10', version='10.0.22000', machine='AMD64')
uname_result(system='Windows', node='DESKTOP-9IJL8A3', release='10', version='10.0.22000', machine='AMD64', processor='Intel64 Family 6 Model 140 Stepping 1, GenuineIntel')
uname_result(system='Linux', node='raspberrypi', release='5.10.17-v7l+', version='#1414 SMP Fri Apr 30 13:20:47 BST 2021', machine='armv7l', processor='')
```

2. os.uname()    # windows不可用，Linux方法
```
posix.uname_result(sysname='Linux', nodename='raspberrypi', release='5.10.17-v7l+', version='#1414 SMP Fri Apr 30 13:20:47 BST 2021', machine='armv7l')
```
