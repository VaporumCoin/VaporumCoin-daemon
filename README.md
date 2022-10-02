[![gitstars](https://img.shields.io/github/stars/vaporumCoin/vaporumcoin?style=social)](https://github.com/VaporumCoin/vaporumcoin/stargazers)
[![twitter](https://img.shields.io/twitter/follow/vaporumcoin?style=social)](https://twitter.com/vaporumcoin)
[![discord](https://img.shields.io/discord/701937565929963581)](https://vaporum.co/discord)

---

![](./doc/imgs/Vaporum-logo.png)


## Vaporum (VPRM)

This is the official Vaporumcoin source code repository based on [KomodoPlatform/Komodo](https://github.com/KomodoPlatform/komodo).

## Resources

- Website:        [VaporumCoin](https://vaporumcoin.us/)
- Block Explorer: [explorer.vaporumcoin.us](https://explorer.vaporumcoin.us/)
- Discord:        [#VaporumCoin](https://discord.gg/QSwCykhF)
- Twitter:        [@vaporumcoin](https://twitter.com/VaporumCoin)
- Support:        [#support channel on discord](https://discord.com/channels/1022595531488362527/1022597438822957106)
- Bitcointalk:    [Coming Soon](https://bitcointalk.org)

## Tech Specification
- Max Supply: 500,000,000 VPRM
- Block Time: 30 seconds
- Block Reward: 50 VPRM
- Block Generation: 50% PoW | 50% PoS
- Mining Algorithm: Equihash (200, 9)

## Getting started

```shell
# Start with Komodo - RECOMMENDED!!
./komodod -ac_name=VPRM -ac_supply=0 -ac_eras=6 -ac_blocktime=30 -ac_reward=5000000000,2500000000,1250000000,625000000,312500000,156250000 -ac_end=1000000,3500000,8500000,18500000,38500000,166500000 -ac_staked=50 -ac_sapling=1 -ac_cbmaturity=1 -ac_cc=0 -addnode=68.3.67.21 -addnode=167.172.130.118 -addnode=157.230.90.81
```

### Dependencies

```shell
#The following packages are needed:
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python python-zmq zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl libsodium-dev
```

### Build Vaporumcoin

This software is based on zcash and considered experimental and is continuously undergoing development.

The dev branch is considered the bleeding edge codebase while the master-branch is considered tested (unit tests, runtime tests, functionality). At no point of time does the vaporum team take any responsibility for any damage out of the usage of this software.
Vaporumcoin builds for all operating systems out of the same codebase. Follow the OS specific instructions from below.

#### Linux
```shell
#Install dependencies:
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python python-zmq zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl libsodium-dev
# Clone the VaporumCoin repo
git clone https://github.com/VaporumCoin/vaporum.git --single-branch
# Change master branch to other branch you wish to compile
cd vaporum
./zcutil/fetch-params.sh
./zcutil/build.sh -j4
# Change -j4 to specify the number of cores to use. ex: -j2
# This can take some time.
```


#### OSX
Ensure you have [brew](https://brew.sh) and Command Line Tools installed.
```shell
# Install brew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# Install Xcode, opens a pop-up window to install CLT without installing the entire Xcode package
xcode-select --install
# Update brew and install dependencies
brew update
brew upgrade
brew tap discoteq/discoteq; brew install flock
brew install autoconf autogen automake
brew update && brew install gcc@8
brew install binutils
brew install protobuf
brew install coreutils
brew install wget
# Clone the VaporumCoin repo
git clone https://github.com/VaporumCoin/vaporum.git --single-branch
# Change master branch to other branch you wish to compile
cd vaporum
./zcutil/fetch-params.sh
./zcutil/build-mac.sh -j$(expr $(sysctl -n hw.ncpu) - 1)
# This can take some time.
```

#### Windows
Use a debian cross-compilation setup with mingw for windows and run:
```shell
#Install dependencies:
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python python-zmq zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl cmake mingw-w64 libsodium-dev libevent-dev
#Install rust targeting x86_64-pc-windows
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
rustup target add x86_64-pc-windows-gnu

sudo update-alternatives --config x86_64-w64-mingw32-gcc
# (configure to use POSIX variant)
sudo update-alternatives --config x86_64-w64-mingw32-g++
# (configure to use POSIX variant)

#Clone the Vaporumcoin repo
git clone https://github.com/VaporumCoin/vaporum.git --single-branch
# Change master branch to other branch you wish to compile
cd vaporum
./zcutil/fetch-params.sh
./zcutil/build-win.sh -j$(expr $(nproc) - 1)
#This can take some time.
```
**Vaporumcoin is experimental and a work-in-progress.** Use at your own risk.

To reset the Vaporumcoin blockchain change into the *~/.komodo/VPRM* data directory and delete the corresponding files by running `rm -rf blocks chainstate debug.log komodostate db.log`

#### Create VPRM.conf

Create a VPRM.conf file:

```
mkdir ~/.komodo/VPRM
cd ~/.komodo/VPRM
touch VPRM.conf

#Add the following lines to the VPRM.conf file:
rpcuser=yourrpcusername
rpcpassword=yoursecurerpcpassword
rpcbind=127.0.0.1
txindex=1
addnode=167.172.130.118
addnode=157.230.90.81
addnode=68.3.67.21

```

License
-------
For license information see the file [COPYING](COPYING).

**NOTE TO EXCHANGES:**
https://bitcointalk.org/index.php?topic=1605144.msg17732151#msg17732151
There is a small chance that an outbound transaction will give an error due to mismatched values in wallet calculations. There is a -exchange option that you can run vaporumcoind with, but make sure to have the entire transaction history under the same -exchange mode. Otherwise you will get wallet conflicts.

**To change modes:**

a) backup all privkeys (launch vaporumcoind with `-exportdir=<path>` and `dumpwallet`)  
b) start a totally new sync including `wallet.dat`, launch with same `exportdir`  
c) stop it before it gets too far and import all the privkeys from a) using `vaporumcoin-cli importwallet filename`  
d) resume sync till it gets to chaintip  

For example:
```shell
./vaporumcoind -exportdir=/tmp &
./vaporumcoin-cli dumpwallet example
./vaporumcoin-cli stop
mv ~/.komodo/VPRM ~/.komodo/VPRM.old && mkdir ~/.komodo/VPRM && cp ~/.komodo/VPRM.old/komodo.conf ~/.komodo/VPRM.old/peers.dat ~/.komodo/VPRM
./vaporumcoind -exchange -exportdir=/tmp &
./vaporumcoin-cli importwallet /tmp/example
```
---


Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
