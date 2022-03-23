Using OpenCL is an optional feature for farming mmx. Offloading the verification of the VDF from the CPU to an OpenCL accelerated GPU (even a CPU integrated iGPU) can increase both performance and power efficiency. **During initial blockchain sync CPU usage will be very high.** Once the blockchain is synced you will see the line: `[Node] INFO: Verified VDF for height 239702, delta = 10.111297 sec, took 0.089002 sec` which indicates that your full node is now verifying the current farming height's VDF. If OpenCL is not being utilized for VDF verification, you should see relatively high CPU usage across all cores. If the timelord is running, you will see high CPU usage on only 2 cores, with low CPU usage on the remaining cores when OpenCL is being utilized for VDF verification.

## OpenCL for Intel iGPUs

Ubuntu 20.04, 21.04
```
sudo apt install intel-opencl-icd
```

Ubuntu ppa for 18.04, 20.04, 21.04
```
sudo add-apt-repository ppa:intel-opencl/intel-opencl
sudo apt update
sudo apt install intel-opencl-icd
```

If the above doesn't work, you can try installing the latest drivers: https://dgpu-docs.intel.com/installation-guides/ubuntu/ubuntu-focal.html

For older Intel CPUs like `Ivy Bridge` or `Bay Trail` you need this package:
```
sudo apt install beignet-opencl-icd
```
(Note beignet performance is not ideal and most users report that it crashes during VDF verification)\
Make sure your iGPU is not somehow disabled, like here for example: https://community.hetzner.com/tutorials/howto-enable-igpu

## OpenCL for AMD GPUs

Linux: \
https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-21-20 \
https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-21-30

```
./amdgpu-pro-install -y --opencl=pal,legacy
```
Mesa drivers with opencl drivers do seem to work, but performance is very poor (VDF verification >4x longer).\
To install mesa drivers:\
Ubuntu:
```
sudo apt install mesa-opencl-icd
```
Arch:
```
sudo pacman -S mesa mesa-utils opencl-mesa
```

Windows: https://google.com/search?q=amd+graphics+driver+download

## OpenCL for Nvidia GPUs

Install CUDA, may the force be with you: \
https://www.google.com/search?q=nvidia+cuda+download

### Arch Linux
```
sudo pacman -S nvidia nvidia-utils opencl-nvidia
```
For older GPUs, use drivers from the AUR:
```
Kepler series newest driver: 470xx
yay -S nvidia-470xx-dkms nvidia-470xx-utils opencl-nvidia-470xx

Fermi series newest driver: 390xx
yay -S nvidia-390xx-dkms nvidia-390xx-utils opencl-nvidia-390xx
```