# Testing Kintsugi

This is a documentation of the steps took to connect to kintsugi and interact with it in different ways. The guide https://hackmd.io/dFzKxB3ISWO8juUqPpJFfw 
is being used to achieve this. Also the ideas that we are going to try to test are inspired from https://hackmd.io/WKpg6SNzQbi1jVKNgrSgWg?view  

First, we need to clone the testnet configuration repository, by doing the following:

```
git clone https://github.com/eth-clients/merge-testnets.git
cd merge-testnets/kintsugi
```
Please note that all the clients will be cloned inside the `kintsugi` folder - Also, It's worth mentioning that this is being run
on macOS Monterey v12.1 w/ processor 2,6 GHz 6-Core Intel Core i7

#### Installing go
```
brew install go
```
And then clone the geth repository, using the `merge-kintsugi` branch
```
git clone -b merge-kintsugi https://github.com/MariusVanDerWijden/go-ethereum.git
cd go-ethereum 
make geth
cd ..
```

#### Installing Lighhouse

Install cmake

brew install cmake rust
Install rust compiler and configure it

```
brew install rustup
rustup-init
source $HOME/.cargo/env
git clone -b unstable https://github.com/sigp/lighthouse.git
cd lighthouse
make
cd ..
```

### EL: Geth / CL: Lighthouse

