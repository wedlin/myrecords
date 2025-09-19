## Patch AST2700 in LF OpenBMC, just for reference
**Download source**
``` 
git clone https://github.com/openbmc/openbmc.git ./openbmc_ast2700
cd  ./openbmc_ast2700
//git checkout master

source ./setup | grep -i ast
evb-ast2500             nvl32-obmc              vegman-sx20
evb-ast2600             olympus-nuvoton         ventura
```
---
**Check patch**
```
1.Go to Gerrit
e.g. OpenBMC's Gerrit：
 https://gerrit.openbmc.org

2.Find the Change
Search Change-Id or patch Title。
open change review page（e.g. 73642）。

3.Download botton
There will be a Download button in the upper right corner of the page. When you pull it down, you will see options like this: 
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/42/73642/57 && git cherry-pick FETCH_HEAD
```
---
**How to patch**
```

# 73642: meta-aspeed: Add ast2700 A1 support | https://gerrit.openbmc.org/c/openbmc/openbmc/+/73642 
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/42/73642/57 && git cherry-pick FETCH_HEAD

#73641: meta-phosphor: Update image_types_phosphor.bbclass for AST2700 | https://gerrit.openbmc.org/c/openbmc/openbmc/+/73641
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/41/73641/32 && git cherry-pick FETCH_HEAD

#80820: meta-evb: Add support for AST2700 EVB | https://gerrit.openbmc.org/c/openbmc/openbmc/+/80820
# meta-evb: Add support for AST2700 EVB (eMMC & UFS) <== after patch this, display evb-ast2700 in setup 
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/20/80820/16 && git cherry-pick FETCH_HEAD

//after patch would support ast2700 (evb-ast2700,evb-ast2700-emmc,evb-ast2700-ufs)
 source setup | grep -i ast
evb-ast2500             nf5280m7                vegman-rx20
evb-ast2600             nicole                  vegman-sx20
evb-ast2700             nvl32-obmc              ventura
evb-ast2700-emmc        olympus-nuvoton         witherspoon
evb-ast2700-ufs         p10bmc                  witherspoon-tacoma

#75304: aspeed: ufs support | https://gerrit.openbmc.org/c/openbmc/openbmc/+/75304
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/04/75304/14 && git cherry-pick FETCH_HEAD

#81057: meta-aspeed: Add u-boot-aspeed-sdk_2023.10 for AST2700 | https://gerrit.openbmc.org/c/openbmc/openbmc/+/81057
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/57/81057/12 && git cherry-pick FETCH_HEAD

#81058: meta-aspeed: Add bootmcu-spl support for AST2700 | https://gerrit.openbmc.org/c/openbmc/openbmc/+/81058
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/58/81058/13 && git cherry-pick FETCH_HEAD

#82105: meta-aspeed: Add fmc-images for AST2700 | https://gerrit.openbmc.org/c/openbmc/openbmc/+/82105
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/05/82105/5 && git cherry-pick FETCH_HEAD

#79646: meta-aspeed:ast2700: use ASPEED Linux kernel | https://gerrit.openbmc.org/c/openbmc/openbmc/+/79646
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/46/79646/31 && git cherry-pick FETCH_HEAD

//#82382: meta-phosphor:libtinyxml2: disable buildpaths warning | https://gerrit.openbmc.org/c/openbmc/openbmc/+/82382
//git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/82/82382/4 && git cherry-pick FETCH_HEAD

#81059: meta-aspeed: Add fmc-imgtool for AST2700 | https://gerrit.openbmc.org/c/openbmc/openbmc/+/81059
git fetch https://gerrit.openbmc.org/openbmc/openbmc refs/changes/59/81059/12 && git cherry-pick FETCH_HEAD
```

## Update python, Cause Ubuntu 20.04 is Python 2.7.18
```				
加入 deadsnakes PPA					
sudo apt update					
sudo apt install software-properties-common -y					
sudo add-apt-repository ppa:deadsnakes/ppa					
sudo apt update
```					
					
**Setup specific Python version（e.g. 3.11）：**
```					
sudo apt install python3.11 python3.11-venv python3.11-dev -y
```					
					
**Create python virtual environment for run devtool / bitbake**
```					
python3.11 -m venv /home/callan/data2/ventura/python3.11   <== only in first time					
source /home/callan/data2/ventura/python3.11/bin/activate  <== frequently used
```
```					
python --version  #check version	
```
---
**Build**
```
source /home/callan/data2/ventura/python3.11/bin/activate
source setup evb-ast2700
bitbake obmc-phosphor-image
```