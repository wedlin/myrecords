# Bridge network mode in QEMU
```bash
Step 1¡Gcreate bridge (br0) and add eno1 to bridge
# setup bridge tool
  sudo apt install bridge-utils

# create bridge¡]br0¡^
  sudo ip link add name br0 type bridge

# add eno1 to bridge¡]assume NIC name is eno1¡^
  sudo ip link set eno1 master br0

# enable bridge
  sudo ip link set br0 up
  sudo ip link set eno1 up

#get dhcp
  sudo dhclient br0

Step 2: Create tap device (for bridge with QEMU)
  sudo ip tuntap add dev tap0 mode tap 
  sudo ip link set tap0 master br0
  sudo ip link set tap0 up

Step 3 run script of start wedge400 qemu:
  ./run2.sh
  
Step 4 run script of test wedge ; TARGET_BMC_IPADDR is wedge400 IP, CLIENT_BMC is BMC IP
  ./test_wedge_redfish_chassis_reset_self_signed_certs_v4 <TARGET_BMC_IPADDR> <CLIENT_BMC>
```
---