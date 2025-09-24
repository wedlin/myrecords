### bitbake debug
```
quick debug：echo
formal debug：bbwarn / bbplain
parameter tracking：bitbake -e
e.g.
bitbake -e obmc-phosphor-image 

bbplain() → print message
bbwarn() → Warning message(yellow color)
bbnote() → normal message（depend on loglevel to display or not）
bbfatal() → display error and stop 
bb.debug(1, "message") → debug level，number bigger more clear

e.g.
bb.plain("=== Debug: Srouce path = %s" % d.getVar("S"))
bb.warn("Warnning：the Build path = %s" % d.getVar("B"))

debug in bb file
e.g.
python () {
    bb.note(">>> My libgcrypt append is loaded")
}

do_patch:append() {
    bb.warn(d">>> Running extra patch step from append")
}

bitbake/lib/bb/data.py:369:        bb.warn("Exception during build_dependencies for %s" % key)
dislapy as follow : 
  WARNING: /home/callan/data2/sony/aspeed_openbmc/meta/recipes-support/libgcrypt/libgcrypt_1.11.1.bb: Exception during build_dependencies for do_patch
```
---
### force(-f) do patch libgcrypt and show debug(-v)
> bitbake libgcrypt -c patch -f -v 
**download source**
bitbake -c fetch libgcrypt
**unpack source**
>bitbake -c unpack libgcrypt
---
### BitBake skip task mechanism
```
BitBake 每個任務（task，e.g. do_fetch, do_unpack, do_patch, do_configure 等）every taks have stamp file，Used to record whether this task has been successfully executed：
The path is usually:
  ${TOPDIR}=/home/callan/data2/sony/aspeed_openbmc
  ${TOPDIR}/build/${MACHINE}/tmp/stamps/${TARGET_SYS}/${PN}
  /home/callan/data2/sony/aspeed_openbmc/build/ast2700-default/tmp/stamps/cortexa35-openbmc-linux/libgcrypt
If the stamp exists and the task is considered up-to-date, it will be skipped. This means that if you've already successfully unpacked the file, BitBake will assume it doesn't need to do it again.
```
---
### Watch multiple parameter
> bitbake -e libgcrypt | egrep '^(MACHINE|PN|PV|MULTIMACH_TARGET_SYS)='
MACHINE="ast2700-default"
MULTIMACH_TARGET_SYS="cortexa35-openbmc-linux"
PN="libgcrypt"
PV="1.11.1"
---
### list bitbake task
```
bitbake -c listtasks libgcrypt

do_build                              Default task for a recipe - depends on all other normal tasks required to 'build' a recipe
do_checkuri                           Validates the SRC_URI value
do_clean                              Removes all output files for a target
do_cleanall                           Removes all output files, shared state cache, and downloaded source files for a target
do_cleansstate                        Removes all output files and shared state cache for a target
do_collect_spdx_deps
do_compile                            Compiles the source in the compilation directory
do_configure                          Configures the source by enabling and disabling any build-time and configuration options for the software being built
do_create_package_spdx
do_create_package_spdx_setscene        (setscene version)
do_create_spdx
do_create_spdx_setscene                (setscene version)
do_deploy_source_date_epoch
do_deploy_source_date_epoch_setscene   (setscene version)
do_devshell                           Starts a shell with the environment set up for development/debugging
do_fetch                              Fetches the source code
do_install                            Copies files from the compilation directory to a holding area
do_listtasks                          Lists all defined tasks for a target
do_package                            Analyzes the content of the holding area and splits it into subsets based on available packages and files
do_package_qa                         Runs QA checks on packaged files
do_package_qa_setscene                Runs QA checks on packaged files (setscene version)
do_package_setscene                   Analyzes the content of the holding area and splits it into subsets based on available packages and files (setscene version)
do_package_write_ipk                  Creates the actual IPK packages and places them in the Package Feed area
do_package_write_ipk_setscene         Creates the actual IPK packages and places them in the Package Feed area (setscene version)
do_packagedata                        Creates package metadata used by the build system to generate the final packages
do_packagedata_setscene               Creates package metadata used by the build system to generate the final packages (setscene version)
do_patch                              Locates patch files and applies them to the source code
do_populate_lic                       Writes license information for the recipe that is collected later when the image is constructed
do_populate_lic_setscene              Writes license information for the recipe that is collected later when the image is constructed (setscene version)
do_populate_sysroot                   Copies a subset of files installed by do_install into the sysroot in order to make them available to other recipes
do_populate_sysroot_setscene          Copies a subset of files installed by do_install into the sysroot in order to make them available to other recipes (setscene version)
do_prepare_recipe_sysroot
do_pydevshell                         Starts an interactive Python shell for development/debugging
do_recipe_qa
do_recipe_qa_setscene                  (setscene version)
do_unpack                             Unpacks the source code into a working directory
```
---