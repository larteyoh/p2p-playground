# p2p-playground
Experimenting with peer-to-peer discovery methods including consensus using raft. 

The goal of this project is to become familiar with peer discovery methods and
eventually use whatever knowledge gained from this to create a fully functioning peer-to-peer network.

**Download**
```bash
git clone --recurse-submodules https://github.com/larteyoh/p2p-playground.git
```
```bash
cd p2p-playground
```

**Building**
```bash
# Build external libraries
cd external/
cmake -G"Unix Makefiles"
make
cd ..
```

```bash
# Build p2p_pg
cd build
cmake ..
make
```
