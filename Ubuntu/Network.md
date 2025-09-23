**Ubuntu version**
```
lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.6 LTS
Release:        20.04
Codename:       focal
```
---
**net work setting**
```
callan@openbmc:/etc/netplan$ vi 01-network-manager-all.yaml
callan@openbmc:/etc/netplan$ cat /etc/netplan/01-network-manager-all.yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
callan@openbmc:/etc/netplan$ nmcli con show
NAME                UUID                                  TYPE      DEVICE
Wired connection 1  442091c7-e887-36b6-bafd-970168043993  ethernet  eno1
virbr0              43bca03e-1f07-4b17-816b-ee0f1edb6b28  bridge    virbr0
Wired connection 2  64c666cc-9ca3-3e78-8e10-44a4485370ef  ethernet  --
Wired connection 3  731bac78-28a0-37fd-a531-28af0595f802  ethernet  --

//info
nmcli connection show "Wired connection 1"

//gatway setting not need to re-active
sudo nmcli con mod "Wired connection 1" ipv4.gateway 172.16.0.1

//re-active
sudo nmcli con down "Wired connection 1"
sudo nmcli con up "Wired connection 1"

//remove ip
sudo ip addr del <IP位址>/<prefix長度> dev <介面名稱>
e.g.: sudo ip addr del 172.16.204.188/16 dev eno1
```
---