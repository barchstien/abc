---
layout: page
title: Qemu
nav_order: 0
---

# Basic install
```bash
#!/bin/bash

#install qemu (on Ubuntu)
sudo apt install qemu qemu-utils

#get install media (ubuntu, fedora, etc)

#create an image file (raw)
qemu-img create disk.img 20G
#(qcow2)
qemu-img create -f qcow2 disk.img 20G

#install system. -enable-kvm is jut for enabling better perfs
qemu-system-x86_64 -enable-kvm -drive file=disk.img,format=qcow2 -m 2G -boot d -cdrom /path/to/ubuntu-16.04.1-server-amd64.iso

#run system without display, with ssh port (only) redirected to localhost
qemu-system-x86_64 -enable-kvm -drive file=disk.img,format=qcow2 -m 2G -daemonize -display none -device e1000,netdev=user.0 -netdev user,id=user.0,hostfwd=tcp::2222-:22
#default use 1 processor, change by adding "-smp n"

#from host system, can now login with
ssh -p 2222 user@localhost

#from guest, access client using
ssh user@10.0.2.2
sshfs user@10.0.2.2:/whatever/path ~/local/path
```

# Bridge host config
 * `modprobe kvm-intel` or, add to modules.conf kvm-intel
 * setup br0 iface, disable existing iface (WIFI don't support bridge)
```
auto br0
iface br0 inet static
	address 192.168.10.51
	netmask 255.255.255.0
	gateway 192.168.10.1
	bridge_ports enp1s0
	bridge_stp off
	bridge_maxwait 5
```
 * add br0 to allowed iface in qemu `echo "allow br0" >> /etc/qemu/bridge.conf`

# Run
## Nat
```bash
#!/bin/bash
port=2223
echo "ssh local port : $port  (bash args are append, -smp n for n proc)"
cmd="qemu-system-x86_64 -enable-kvm -drive file=fedora19_stapisdk.qcow2,format=qcow2 -m 2G -daemonize -display none -device e1000,netdev=user.0 -netdev user,id=user.0,hostfwd=tcp::$port-:22 -smp 4"
echo $cmd
$cmd
```
## Bridge
```bash
#!/bin/bash
port=2223
echo "See README for host configuration of bridge"
echo "ssh local port : $port  (bash args are append, -smp n for n proc)"
cmd="qemu-system-x86_64 -enable-kvm \
	-drive file=drive.qcow2,format=qcow2 \
	-m 2G  -smp 4 -daemonize -display none \
	-net nic -net bridge,br=br0 $*"
echo $cmd
$cmd
```

