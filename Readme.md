Example-Repo: Architecture Independent packages
====================================================

Usually, lede packages are built for a specific architecture
(target / subtarget) using a cross-compile tool-chain.

When packing scripts or resources (ie configuration files), this is approach generates some effort:
A different .ipk-package is built for each architecture, although is content neither depends on a specific
instruction set, nor on a specific tool-chain.

This repository contains a sample package (helloworld) that can be built for all architectures

Content
-----------------------------------------------------

The helloworld package includes

- A script `/bin/hello_world.sh` - echoing "Hello World" when executed
- A subdirectory `hello_world/files`, which is including in the package. I.e. `files/bin/hello_world.sh` is installed at `/bin/hello_world.sh`.


Building the package
--------------------------------------------------------

```bash
# Download the SDK (i.e. 17.01.3)
wget  http://downloads.lede-project.org/releases/17.01.3/targets/x86/64/lede-sdk-17.01.3-x86-64_gcc-5.4.0_musl-1.1.16.Linux-x86_64.tar.xz

# Extract the SDK
tar xf lede-sdk-17.01.3-x86-64_gcc-5.4.0_musl-1.1.16.Linux-x86_64.tar.xz

# Enter the SDK
cd lede-sdk-17.01.3-x86-64_gcc-5.4.0_musl-1.1.16.Linux-x86_64/

# Add feed
echo "src-git demo https://github.com/yanosz/lede_all_architectures.git" > feeds.conf

# Update and install feeds
./scripts/feeds update -a
./scripts/feeds install -a

# Generate configuration including the packages
make defconfig

# Build the package for all architectures
make package/helloworld/compile CONFIG_TARGET_ARCH_PACKAGES=all
```
This creates `bin/packages/all/demo/helloworld_1.0-1_all.ipk`.


Distributing the package
------------------------------------------------------------------------------
```bash
# Mind generating a release signing key (if you haven't done before)
./staging_dir/host/bin/usign -G -p /somepath/lede.pub -s /somepath/lede.sec

# Create the package repository index
make -j1 V=99 package/index CONFIG_TARGET_ARCH_PACKAGES=all BUILD_KEY=/somepath/lede.sec

# REMOVE packages, that are included by Default
rm -R bin/packages/all/base/
```
The repository is generated in `bin/packages/all/`.
