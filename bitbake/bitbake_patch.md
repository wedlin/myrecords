# Libgcrypt compile issue
## 1. Create file structure
```
create directory in your layer: (e.g. aspeed openbmc - meta-aspeed-sdk/meta-ast2700-sdk)

cd meta-aspeed-sdk/meta-ast2700-sdk
mkdir -p recipes-support/libgcrypt/files
```
---
## 2. How to patch
### Get source code
```
devtool libgcrypt 
cd \$TOPDIR/workspace/source/
e.g. cd ~/data2/sony/aspeed_openbmc/build/ast2700-default/workspace/sources/libgcrypt
modify as follow: add -lpthread
vi tests/Makefile.am
testdrv_LDADD = $(LDADD_FOR_TESTS_KLUDGE)
t_thread_local_LDADD = $(LDADD) -lpthread <== add this
```
### Create path file
```bash
git add tests/Makefile.am
git commit -m "add t_thread_local_LDADD in Makefile.am"
git format-patch HEAD^
```
### Add upstream status
```
vi 0001-add-t_thread_local_LDADD-in-Makefile.am.patch
From ec7448d56368d692fe1ec7680fb0c749ec55cf55 Mon Sep 17 00:00:00 2001
From: wedlin <wedlin@ingrasys.com>
Date: Wed, 24 Sep 2025 14:37:55 +0800
Subject: [PATCH] add t_thread_local_LDADD in Makefile.am
Upstream-Status: Pending  <== add this 
---
 tests/Makefile.am | 2 ++
 1 file changed, 2 insertions(+)

diff --git a/tests/Makefile.am b/tests/Makefile.am
index 9a9e1c2..2880bdb 100644
--- a/tests/Makefile.am
+++ b/tests/Makefile.am
@@ -95,6 +95,7 @@ testapi_LDADD = $(standard_ldadd) @LDADD_FOR_TESTS_KLUDGE@
 t_lock_LDADD = $(standard_ldadd) $(GPG_ERROR_MT_LIBS) @LDADD_FOR_TESTS_KLUDGE@
 t_lock_CFLAGS = $(GPG_ERROR_MT_CFLAGS) -lpthread
 testdrv_LDADD = $(LDADD_FOR_TESTS_KLUDGE)
+t_thread_local_LDADD = $(LDADD) -lpthread

 # Build a version of the test driver for the build platform.
 testdrv-build: testdrv.c
@@ -115,6 +116,7 @@ t_kdf_LDADD = $(standard_ldadd) $(GPG_ERROR_MT_LIBS) @LDADD_FOR_TESTS_KLUDGE@
 t_kdf_CFLAGS = $(GPG_ERROR_MT_CFLAGS) -lpthread
 endif
```
### Copy patch to file structure of recipe
```
cp 0001-add-t_thread_local_LDADD-in-Makefile.am.patch /home/callan/data2/sony/aspeed_openbmc/meta-aspeed-sdk/meta-ast2700-sdk/recipes-support/libgcrypt/files
```
---
### delete the source code
>devtool reset libgcrypt
 
---
## 3. Create Recipe
```
reference recipe in openbmc (e.g. poky/meta/recipes-support/libgcrypt/libgcrypt_1.11.1.bb

cd ..
vi libgcrypt_1.11.1.bbappend

Content¡G
#FILESEXTRAPATHS is mean "$src/meta-aspeed-sdk/meta-ast2700-sdk/recipes-support/libgcrypt/files/openbmc-phosphor/#0001-tests-add-pthread-to-t-thread-local.patch"
#EXTRA_OECONF += "--disable-tests"
FILESEXTRAPATHS:prepend := "${THISDIR}/files:"
SRC_URI += "file://0001-add-t_thread_local_LDADD-in-Makefile.am.patch"
#EXTRA_LDFLAGS += "-lpthread"

#debug; start
python () {
    bb.note(">>> Wed libgcrypt append is loaded")
}

do_patch:append() {
    bb.warn(">>> Running extra patch step from append")
}
#debug; start
```
---
## 4. clean and rebuild libgcrypt
>bitbake -c cleanall libgcrypt
//bitbake -c cleansstate libgcrypt
bitbake libgcrypt
---
## 5. additional information about
### Check source and build path 
>bitbake -e libgcrypt | grep -i ^S=
S="/home/callan/data2/sony/aspeed_openbmc/build/ast2700-default/tmp/work/cortexa35-openbmc-linux/libgcrypt/1.11.1/libgcrypt-1.11.1"

>bitbake -e libgcrypt | grep -i ^B=
B="/home/callan/data2/sony/aspeed_openbmc/build/ast2700-default/tmp/work/cortexa35-openbmc-linux/libgcrypt/1.11.1/build"

### Check append recipe
>bitbake-layers show-appends libgcrypt
=== Matched appended recipes ===
libgcrypt_1.11.1.bb:
  /home/callan/data2/sony/aspeed_openbmc/meta-aspeed-sdk/meta-ast2700-sdk/recipes-support/libgcrypt/libgcrypt_1.11.1.bbappend
---
**This way, the patch will be automatically applied during Yocto build, and t-thread-local will reference t_thread_local_LDADD with -lpthread, provide automatically modify the Makefile.**