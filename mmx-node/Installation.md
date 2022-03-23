## Dependencies

Windows binaries: https://github.com/stotiks/mmx-node/releases

Note: OpenCL packages are optional, ie. `ocl-icd-opencl-dev`, etc.

Ubuntu Linux:
```
sudo apt update
sudo apt install git cmake build-essential libsecp256k1-dev librocksdb-dev libsodium-dev zlib1g-dev ocl-icd-opencl-dev clinfo screen
```

Arch Linux:
```
sudo pacman -Syu
sudo pacman -S base-devel git cmake zlib libsecp256k1 rocksdb libsodium opencl-headers ocl-icd clinfo screen
```

Fedora Linux:
```
yum install kernel-devel git cmake clinfo gcc gcc-c++ libsecp256k1-devel rocksdb rocksdb-devel libsodium ocl-icd-devel opencl-headers opencl-utils ghc-zlib
```

OpenCL provides faster and more efficient VDF verification using an integrated or dedicated GPU.
A standard iGPU found in Intel CPUs with 192 shader cores is plenty fast enough.
If you dont have a fast quad core CPU (>3 GHz) or higher core count CPU, it is required to have a GPU with OpenCL support.

Make sure to be in the `video` and or `render` group (depends on distribution) to be able to access a GPU:
```
sudo adduser $USER video
sudo adduser $USER render
```

## Building

```
git clone https://github.com/madMAx43v3r/mmx-node.git
cd mmx-node
git submodule update --init --recursive
./make_devel.sh
```

To update to latest version:
```
./update.sh
```

To switch to latest testnet:
```
rm NETWORK
./update.sh
```

### Windows via WSL

To setup Ubuntu 20.04 in WSL on Windows you can follow the tutorial over here: \
https://docs.microsoft.com/en-us/learn/modules/get-started-with-windows-subsystem-for-linux/

Make sure to install Ubuntu in step 2: https://www.microsoft.com/store/p/ubuntu/9nblggh4msv6

Then type "Ubuntu" in the start menu and start it, you will be asked to setup a user and password.
After that you can follow the normal instructions for Ubuntu 20.04.

To get OpenCL working in WSL:
https://devblogs.microsoft.com/commandline/oneapi-l0-openvino-and-opencl-coming-to-the-windows-subsystem-for-linux-for-intel-gpus/

### Using packaged secp256k1

If you don't have a system package for `libsecp256k1`:
```
cd mmx-node/secp256k1
./autogen.sh
./configure
make -j8
cd ..
./make_devel.sh -DWITH_SECP256K1=1
```

### Custom RocksDB build

On some platforms, like Ubuntu 18.04, the provided RocksDB package is faulty and will cause linker errors when building mmx-node.

To solve the issue, you have to compile RocksDB manually as follows:
```
git clone https://github.com/facebook/rocksdb.git
cd rocksdb
USE_RTTI=1 make -j8 shared_lib
sudo make install-shared
```

You might have to set LD_LIBRARY_PATH for it to find the library:
```
export LD_LIBRARY_PATH=/usr/local/lib/
```

Make sure to uninstall any system package which provides a faulty version of RocksDB, like `librocksdb-dev`.

(For some reason RocksDB developers thought it be a great idea to compile unusable libraries by default)

### Custom storage path

To change the storage path for everything you can set environment variable `MMX_HOME` to `/your/path/` (trailing slash required). By default the current directory is used, ie. `mmx-node`.