# Record of Aspeed Ast2700 in OpenBMC

- Prerequisite
  - Ubuntu 20.04.6 LTS
  - incremental memory swap file for physical memory
  - Patch libgcrypt  
  
- [libgcrypt patch](https://github.com/wedlin/myrecords/blob/main/bitbake/bitbake_patch.md)   
- [add swap file](https://github.com/wedlin/myrecords/blob/main/Ubuntu/swapmemory.md)
- [python update](https://github.com/wedlin/myrecords/blob/main/python/python_memo1.md)
---
**Use python3.11 for build in Ubuntu 20.04**
> source /home/callan/data2/ventura/python3.11/bin/activate

**Download source(Aspeed openbmc)** 
> git clone -b v09.07 https://github.com/AspeedTech-BMC/openbmc.git ./aspeed_ob_v09.07

**Setup Aspeed openbmc**
> cd ./aspeed_ob_v09.07
  source setup ast2700-default

**Build Aspeed openbmc**
> bitbake obmc-phosphor-image

**Patch libgcrypt (if need)**
- [libgcrypt patch](https://github.com/wedlin/myrecords/blob/main/bitbake/bitbake_patch.md)
```
Workground:
./test/Makefile 

t-thread-local$(EXEEXT): $(t_thread_local_OBJECTS) $(t_thread_local_DEPENDENCIES) $(EXTRA_t_thread_local_DEPENDENCIES)
        @rm -f t-thread-local$(EXEEXT)
        $(AM_V_CCLD)$(LINK) $(t_thread_local_OBJECTS) $(t_thread_local_LDADD) $(LIBS) -lpthread        
```
---
**Run in QEMU**
Methon 1:
*runqemu slirp serialstdio*
```
runqemu - INFO - Running bitbake -e  ...
runqemu - INFO - Continuing with the following parameters:
KERNEL: []
MACHINE: [ast2700-default]
FSTYPE: [static.mtd]
ROOTFS: [/home/callan/data2/sony/aspeed_ob_v09.07/build/ast2700-default/tmp/deploy/images/ast2700-default/obmc-phosphor-image-ast2700-default-20250925011229.static.mtd]
CONFFILE: [/home/callan/data2/sony/aspeed_ob_v09.07/build/ast2700-default/tmp/deploy/images/ast2700-default/obmc-phosphor-image-ast2700-default-20250925011229.qemuboot.conf]

runqemu - INFO - Network configuration: ip=dhcp
runqemu - INFO - Port forward: hostfwd=tcp:127.0.0.1:2222-:22 hostfwd=tcp:127.0.0.1:2323-:23
runqemu - INFO - Running /home/callan/data2/sony/aspeed_ob_v09.07/build/ast2700-default/tmp/work/x86_64-linux/qemu-helper-native/1.0/recipe-sysroot-native/usr/bin/qemu-system-aarch64 -net nic,netdev=net0 -netdev user,id=net0,hostfwd=tcp:127.0.0.1:2222-:22,hostfwd=tcp:127.0.0.1:2323-:23,tftp=/home/callan/data2/sony/aspeed_ob_v09.07/build/ast2700-default/tmp/deploy/images/ast2700-default  -drive file=/home/callan/data2/sony/aspeed_ob_v09.07/build/ast2700-default/tmp/deploy/images/ast2700-default/obmc-phosphor-image-ast2700-default-20250925011229.static.mtd,if=mtd,format=raw   -machine ast2700a1-evb   -m 1G -serial mon:stdio -serial null -display none

runqemu - INFO - Host uptime: 236070.21
```
Methon 2:
```
QB_SLIRP_OPT="-netdev user,id=net0,hostfwd=tcp:0.0.0.0:2222-:22,hostfwd=tcp:0.0.0.0:2323-:23,tftp=/home/callan/data2/sony/aspeed_ob_v09.07/build/ast2700-default/tmp/deploy/images/ast2700-default" runqemu slirp serialstdio 
```
---
**Supplementary Notes**
### QEMU SSH
Method 1:
>ssh -p 2222 root@localhost

Method 2:
>ssh -p 2222 root@$IP
e.g ssh -p 2222 root@172.16.53.177

### Aspeed version check
>git describe --tags
v09.07
git describe --tags
v09.07.i-1494-gb28560590f
```
v09.07 - bitbake version:
bitbake --version
BitBake Build Tool Core version 2.9.1

in $TOPDIR/poky/bitbake/lib/bb/__init__.py
__version__ = "2.9.1"

import sys
if sys.version_info < (3, 8, 0):
    raise RuntimeError("Sorry, python 3.8.0 or later is required for this version of bitbake")
```
```
v09.07.i - bitbake version:
bitbake --version
BitBake Build Tool Core version 2.12.0

in $TOPDIR/poky/bitbake/lib/bb/__init__.py
__version__ = "2.12.0"

import sys
if sys.version_info < (3, 9, 0):
    raise RuntimeError("Sorry, python 3.9.0 or later is required for this version of bitbake")
```
---    