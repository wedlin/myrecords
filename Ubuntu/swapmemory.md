**調整 swapfile 大小以及路徑**
## ? method 1：tune size of swapfile 
### 1.stop the pervios swapfile
```
sudo swapoff /swapfile
```
### 2.Tune size(e.g. 32G)
* If the file is too small, you can directly recreate it:
> sudo fallocate -l 32G /swapfile
* If your system does not support fallocate, you can use dd:
> sudo dd if=/dev/zero of=/swapfile bs=1G count=32
### 3.Reset permissions and format as swap
> sudo chmod 600 /swapfile
sudo mkswap /swapfile
### 3.Enable
> sudo swapon /swapfile
---
## ? Method2：change path of swapfile
if you want to put the swapfile in /mnt/storage/swapfile：
### 1.Create new directory（if need）
> sudo mkdir -p /mnt/storage
### 2.Create new swapfile
> sudo fallocate -l 64G /mnt/storage/swapfile
sudo chmod 600 /mnt/storage/swapfile
sudo mkswap /mnt/storage/swapfile
### 3.Enable
> sudo swapon /mnt/storage/swapfile
### 4.Modify /etc/fstab
> find the original row of /swapfile，delete or mark ，then add:
/mnt/storage/swapfile none swap sw 0 0
### 5.test is available or not
>sudo swapoff -a && sudo swapon -a
swapon --show

### ?? Notes
```
Permission is 600（not 666），it would show warnning messag, if permission is not 666:
sudo chmod 600 /swapfile
verify：
free -h
swapon --show
```
---