# OpenBMC Development Structure Demo

## 1️⃣ Directory Structure
```
igsobmc/                   ← Main repository
├── openbmc_ast2700/       ← OpenBMC + AST2700 patch
├── openbmc-upstream/      ← Upstream OpenBMC
├── igs_ob_mid/            ← Middleware layer
│   ├── meta-igs-ast2700/  ← AST2700 BSP extension layer (e.g., AST2700 patch)
│   └── meta-igs-common/   ← Common layer (e.g., libgcrypt patch)
└── igs_ob_prj/            ← Project-specific layer
    ├── meta-igs-prjA/     ← Project A layer (emulate to use AST2700 patch of middleware)
    ├── meta-igs-prjB/     ← Project B layer (emulate to use OpenBMC build-in AST2700 support)
    └── setup/             ← Build setup scripts
```

---

## 2️⃣ Git Repository Creation

### Step 1: Create empty repositories in GitLab
```
igsobmc
openbmc-ast2700
openbmc-upstream
igs_ob_mid
meta-igs-ast2700
meta-igs-common
igs_ob_prj
meta-igs-prjA
meta-igs-prjB
```

---

### Step 2: Initialize and push middleware layers (`igs_ob_mid`)

#### a) Individual layers
```bash
# meta-igs-ast2700
cd igsobmc/igs_ob_mid/meta-igs-ast2700
git init
git add .
git commit -m "Initial commit - meta-igs-ast2700"
git remote add origin https://172.16.53.179/openbmc_group/meta-igs-ast2700.git
git branch -M main
git push -u origin main

# meta-igs-common
cd ../meta-igs-common
git init
git add .
git commit -m "Initial commit - meta-igs-common"
git remote add origin https://172.16.53.179/openbmc_group/meta-igs-common.git
git branch -M main
git push -u origin main
```

#### b) Combine as a submodule repository (`igs_ob_mid`)
```bash
cd ../
git init
rm -rf meta-igs-ast2700 meta-igs-common
git submodule add https://172.16.53.179/openbmc_group/meta-igs-ast2700.git meta-igs-ast2700
git submodule add https://172.16.53.179/openbmc_group/meta-igs-common.git meta-igs-common
git add .
git commit -m "Add meta-igs-ast2700/meta-igs-common as submodule"
git remote add origin https://172.16.53.179/openbmc_group/igs_ob_mid.git
git branch -M main
git push -u origin main
```

---

### Step 3: Initialize and push project layers (`igs_ob_prj`)

#### a) Individual project layers
```bash
# meta-igs-prjA
cd igsobmc/igs_ob_prj/meta-igs-prjA
git init
git add .
git commit -m "Initial commit - meta-igs-prjA"
git remote add origin https://172.16.53.179/openbmc_group/meta-igs-prjA.git
git branch -M main
git push -u origin main

# meta-igs-prjB
cd ../meta-igs-prjB
git init
git add .
git commit -m "Initial commit - meta-igs-prjB"
git remote add origin https://172.16.53.179/openbmc_group/meta-igs-prjB.git
git branch -M main
git push -u origin main
```

#### b) Combine as a submodule repository (`igs_ob_prj`)
```bash
cd ../
git init
rm -rf meta-igs-prjA meta-igs-prjB
git submodule add https://172.16.53.179/openbmc_group/meta-igs-prjA.git meta-igs-prjA
git submodule add https://172.16.53.179/openbmc_group/meta-igs-prjB.git meta-igs-prjB
git add .
git commit -m "Add meta-igs-prjA/meta-igs-prjB as submodule"
git remote add origin https://172.16.53.179/openbmc_group/igs_ob_prj.git
git branch -M main
git push -u origin main
```

---

### Step 4: OpenBMC repositories

#### a) `openbmc_ast2700`
```bash
cd igsobmc/openbmc_ast2700
git init
git add .
git commit -m "Initial commit - openbmc_ast2700"
git remote set-url origin https://172.16.53.179/openbmc_group/openbmc_ast2700.git
git branch -M main
git push -u origin main
```

#### b) `openbmc-upstream`
```bash
cd igsobmc
git clone https://github.com/openbmc/openbmc.git ./openbmc-upstream
cd openbmc-upstream
below is for reference not need:
//git remote add gitlab https://172.16.53.179/openbmc_group/openbmc-upstream.git
//git fetch origin
//git push --all gitlab
//git push --tags gitlab
```

---

### Step 5: Main repository `igsobmc`
```bash
cd igsobmc
git init
rm -rf igs_ob_mid igs_ob_prj openbmc_ast2700 openbmc-upstream
git submodule add https://172.16.53.179/openbmc_group/openbmc_ast2700.git openbmc_ast2700
git submodule add https://172.16.53.179/openbmc_group/openbmc-upstream.git openbmc-upstream
git submodule add https://172.16.53.179/openbmc_group/igs_ob_mid.git igs_ob_mid
git submodule add https://172.16.53.179/openbmc_group/igs_ob_prj.git igs_ob_prj
git add .
git commit -m "Initial structure with submodules"
git remote add origin https://172.16.53.179/openbmc_group/igsobmc.git
git branch -M main
git push -u origin main
```

### Setup 6: change URL of .gitmodule in superproject ()`igsobmc`)
```bash
.gitmodules` is a text configuration file,
located in the root directory of the superproject (main project), used to record information about all submodules.
Change the URL of `[submodule "openbmc-upstream"]` to the original OpenBMC clone path.

obmc@obmc-XXX-XXXXX-X-1A52JNJ00-600-G:~/data1/gitlab_test/test/igsobmc_dev$ cat .gitmodules
[submodule "openbmc_ast2700"]
        path = openbmc_ast2700
        url = https://172.16.53.179/openbmc_group/openbmc_ast2700.git
[submodule "igs_ob_mid"]
        path = igs_ob_mid
        url = https://172.16.53.179/openbmc_group/igs_ob_mid.git
[submodule "igs_ob_prj"]
        path = igs_ob_prj
        url = https://172.16.53.179/openbmc_group/igs_ob_prj.git
[submodule "openbmc-upstream"]
        path = openbmc-upstream
        url = https://github.com/openbmc/openbmc.git <==modify this
```

---

## 3️⃣ Summary of Git Operations
| Layer/Repo                     | Action                          |
|--------------------------------|--------------------------------|
| `meta-igs-common` / `meta-igs-ast2700` | Initialize & push              |
| `igs_ob_mid`                   | Add submodules & push           |
| `meta-igs-prjA` / `meta-igs-prjB`      | Initialize & push              |
| `igs_ob_prj`                   | Add submodules & push           |
| `openbmc_ast2700` / `openbmc-upstream` | Push to GitLab                 |
| `igsobmc`                      | Add all submodules & push       |

---

## 4️⃣ Usage Instructions

### Clone repository with submodules
```bash
git clone --recurse-submodules https://172.16.53.179/openbmc_group/igsobmc.git
```

### Build
```bash
source /home/callan/data2/ventura/python3.11/bin/activate  # optional

source setup igs-evb-ast2700
bitbake obmc-phosphor-image

# Using different upstream
OPENBMC_UPSTREAM="../openbmc_ast2700" source setup igs-evb-ast2700b
bitbake obmc-phosphor-image
```

### Add a new project layer (`meta-igs-prjC`)
```bash
cd igsobmc/igs_ob_prj
bitbake-layers create-layer ./meta-igs-prjC  # <== note: work after source setup
rm -rf ./meta-igs-prjC/recipes-example

# Copy configs from Project A
cp -a ./meta-igs-prjA/conf/machine ./meta-igs-prjC/conf
cp -a ./meta-igs-prjA/conf/templates ./meta-igs-prjC/conf

# Update configuration
meta-igs-prjC/conf/machine/igs-evb-ast2700.conf   # <== change name
meta-igs-prjC/conf/templates/default/bblayers.conf.sample  # <== add the layers you need
```

---
## 5️⃣ How to Update a Submodule and Remove Temporary Build Directories

### 1. Delete Local Temporary Build Folders
```bash
rm -rf sstate-cache tmp tmp-bootmcu
```
These directories usually contain build caches or temporary files. Removing them ensures your submodule stays clean before syncing changes.

---

### 2. Make Sure They Are Not Ignored by `.gitignore`
If these folders are listed in `.gitignore`, Git will **not track their deletion**.

Check your `.gitignore`:
```bash
cat .gitignore
```

Look for entries like:
```
sstate-cache/
tmp/
tmp-bootmcu/
```

If found, **temporarily comment them out** or remove those lines, so Git can detect that these folders were deleted.

*(You can add them back after the update.)*

---

### 3. Let Git Know About the Deletion
Run:
```bash
git rm -r --cached sstate-cache tmp tmp-bootmcu
```
> `--cached` removes the directories from Git’s index only (it won’t delete local files if they still exist).

Then verify:
```bash
git status
```
Expected output:
```
deleted: sstate-cache/
deleted: tmp/
deleted: tmp-bootmcu/
```

---

### 4. Commit and Push the Changes
```bash
git commit -m "Remove build cache directories (sstate-cache, tmp, tmp-bootmcu)"
git push
```

---

### 5. (Optional) Re-add `.gitignore` Rules
To prevent these folders from being tracked again in the future:
```bash
echo -e "sstate-cache/\ntmp/\ntmp-bootmcu/" >> .gitignore
git add .gitignore
git commit -m "Update .gitignore to exclude build cache"
git push
```

---

### 6. Update the Submodule in the Main Repository
follow this porcedure：
```bash
cd openbmc-upstream        # into submodule
git checkout master        # switch to master (One-time instruction)
git pull origin master     # update to latest
```
after make sure：
```bash
cd ../igsobmc
git add openbmc-upstream
git commit -sm "Update submodule openbmc-upstream to latest master"
git push                    
```

---

### ✅ Result
The main repository now points to the **latest commit** of the submodule. Other developers who pull updates will automatically receive the submodule changes, including the removal of build cache directories.


