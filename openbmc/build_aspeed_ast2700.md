# Record of Aspeed Ast2700 in OpenBMC

* Prerequisite
  Ubuntu 20.04.6 LTS
  incremental memory swap file for physical memory
  Patch libgcrypt  
  
* [add swap file]   
* [python issue] https://github.com/wedlin/myrecords/blob/main/python/python_memo1.md
---
**Use python3.11 for build in Ubuntu 20.04**
> source /home/callan/data2/ventura/python3.11/bin/activate

**Download source** 
> git clone -b v09.07.i https://github.com/AspeedTech-BMC/openbmc.git

**Setup Aspeed openbmc**
> source setup ast2700-default

*Build Aspeed openbmc*
> bitbake obmc-phosphor-image

**Patch libgcrypt**
```
t-thread-local$(EXEEXT): $(t_thread_local_OBJECTS) $(t_thread_local_DEPENDENCIES) $(EXTRA_t_thread_local_DEPENDENCIES)
        @rm -f t-thread-local$(EXEEXT)
        $(AM_V_CCLD)$(LINK) $(t_thread_local_OBJECTS) $(t_thread_local_LDADD) $(LIBS) -lpthread
```
**Run in QEMU**
methon 1:
>runqemu slirp serialstdio

methon 2:
>todo
---
? Supplementary Notes
**Aspeed version check*
> git describe --tags
v09.07.i-1494-gb28560590f
