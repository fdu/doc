# DAMON

This is an introduction to using DAMON, the kernel Data Access Monitor.

## Setup

### Kernel

```
# Get the source
mkdir src
wget -qO- https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.11.tar.xz | xz -d - | tar xf - --strip-components=1 -C src
# Create configuration
mkdir build
cp /boot/config-$(uname -r) build/.config
make O=../build -C src/ -j`nproc` olddefconfig
./src/scripts/config --file build/.config --set-str CONFIG_LOCALVERSION "-damon"
./src/scripts/config --file build/.config --set-str CONFIG_SYSTEM_TRUSTED_KEYS ""
./src/scripts/config --file build/.config --enable CONFIG_DAMON
./src/scripts/config --file build/.config --enable CONFIG_DAMON_VADDR
./src/scripts/config --file build/.config --enable CONFIG_DAMON_PADDR
./src/scripts/config --file build/.config --enable CONFIG_DAMON_SYSFS
./src/scripts/config --file build/.config --enable CONFIG_DAMON_DBGFS
./src/scripts/config --file build/.config --enable CONFIG_DAMON_RECLAIM
./src/scripts/config --file build/.config --enable CONFIG_DAMON_LRU_SORT
# Build and install
make O=../build -C src/ -j`nproc`
sudo make O=../build -C src/ -j`nproc` modules_install install
sudo reboot
```

### User space

```
# Perf
make O=../build -C src/tools/perf/ -j`nproc`
make O=../build -C src/tools/perf/ -j`nproc` install
# DAMO: Data Access Monitoring Operator
sudo pip3 install damo
```



## Links

- https://sjp38.github.io/post/damon/

