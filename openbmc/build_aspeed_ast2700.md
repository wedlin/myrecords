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

**Download source** 
> git clone -b v09.07 https://github.com/AspeedTech-BMC/openbmc.git ./aspeed_ob_v09.07

**Setup Aspeed openbmc**
> cd ./aspeed_ob_v09.07
  source setup ast2700-default

*Build Aspeed openbmc*
> bitbake obmc-phosphor-image

**Patch libgcrypt (if need)**
```
Workground:
./test/Makefile 

t-thread-local$(EXEEXT): $(t_thread_local_OBJECTS) $(t_thread_local_DEPENDENCIES) $(EXTRA_t_thread_local_DEPENDENCIES)
        @rm -f t-thread-local$(EXEEXT)
        $(AM_V_CCLD)$(LINK) $(t_thread_local_OBJECTS) $(t_thread_local_LDADD) $(LIBS) -lpthread
```
**Run in QEMU**
Methon 1:
>runqemu slirp serialstdio

Methon 2:
>todo
---
**Supplementary Notes**
 - Aspeed version check
>git describe --tags
v09.07
