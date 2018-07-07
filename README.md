# HiveOS Custom Miner Integration

Tested on [AMD cards only](#notes).

## Quick Option

This method is not recommended [(see disclaimer)](#disclaimer), but provided as a convenience.  I recommend following the [instructions below to compile](#better-option---compile-from-source) the miner yourself.

- In HiveOS dashboard create new Wallet
- Select **Custom** option for miner
- **Custom miner name**: Enter a name to identify this wallet config (i.e. TUBE Official Pool)
- **Installation URL**: https://github.com/bizlift/bittube-miner-hive/releases/download/v0.1.0/bittube-2.0.0.1.tar.gz
- **Wallet and worker template**: Paste your wallet address here.  On Cryptoknight pool you can also append wallet with a dot and [static diff](https://cryptoknight.cc/ipbc/#getting_started)
- **Pool URL**: Enter pool address:port (i.e. `mining.bit.tube:15555`)
- **Extra config arguments**: Optional runtime configs (i.e. `--noAMDCache`)
- Save wallet.  Set rig to use Custom miner and the new wallet you created
- After successful run with Hashrate & Results showing up in Hive dashboard, login to miner shell and edit `/hive/custom/bittube/amd.txt` to tweak your AMD card config (i.e. double thread each card, adjust intensity, etc.).  Restart miner for changes to take effect.


## Better Option - Compile From Source

- Login to shell of your HiveOS miner

- Run these commands:

```
sudo apt install libmicrohttpd-dev libssl-dev cmake build-essential libhwloc-dev ocl-icd-opencl-dev amdgpu-pro
mkdir /hive/custom/bittube-git
cd /hive/custom/bittube-git
git clone https://github.com/ipbc-dev/bittube-miner.git
mkdir bittube-miner/build
cd bittube-miner/build
cmake ..
make install
mkdir /hive/custom/bittube
cp build/bin/* /hive/custom/bittube

```
Now that you have a compiled binary, you just need to add the HiveOS [custom files](https://github.com/bizlift/bittube-miner-hive/archive/master.zip).

Copy the following files to `/hive/custom/bittube`:
- bittube.conf
- config.txt
- cpu.txt
- expert.json

Follow the steps above in [Quick Option](#quick-option) to setup your custom wallet. Skip the **Installation URL** field as it has been installed manually.


## Notes

Please edit `amd.txt` manually. The HiveOS Custom Miner feature does not have extra fields for card config like they do with their own xmr-stak miner.

Tested on AMD only.  I don't have access to NVIDIA cards and am CUDA illiterate.

The release notes for 2.0.0.1 mention:

```
V4 CUDA PoW still work in progress, in the meantime to mine with NVIDIA cards you need to use OpenCL.

bittube-miner --noNVIDIA --openCLVendor NVIDIA
```

You can try adding `--noNVIDIA --openCLVendor NVIDIA` to your Hive wallet's *Extra config arguments* field. I have no idea if this will work.

The new cryptonight-saber algo is tough on CPUs, so it's disabled by default.  If you would like to enable it, edit the `cpu.txt` file.

## Disclaimer
This is just a quick solution until xmr-stak supports the new TUBE algo.

All binaries have been compiled from source without edits, or copied from the default xmr-stak miner in HiveOS. The tarball in Releases is provided for your convenience.

However, it is rarely a good idea to use binaries from unknown sources and I strongly recommend that you compile the latest BitTube miner yourself and just copy over the h-xxxx.sh files.


---

# BitTube-Miner

[BitTube](https://coin.bit.tube/)

Based on XMR-Stak by fireice_uk and psychocrypt
and Wolf AMD backend

## Overview
* [Features](#features)
* [Download](#download)
* [Usage](doc/usage.md)
* [HowTo Compile](doc/compile.md)
* [FAQ](doc/FAQ.md)

## Features

- support all common backends (CPU/x86, AMD-GPU and NVIDIA-GPU)
- support all common OS (Linux, Windows and macOS)
- easy to use
  - guided start (no need to edit a config file for the first start)
  - auto configuration for each backend
- open source software (GPLv3)
- TLS support
- [HTML statistics](doc/usage.md#html-and-json-api-report-configuraton)
- [JSON API for monitoring](doc/usage.md#html-and-json-api-report-configuraton)

## Download

You can find the latest releases and precompiled binaries on GitHub under [Releases](https://github.com/ipbc-dev/bittube-miner/releases).

