# Ubuntu singularity installation of CMSSW for Fireworks


### Install CVMFS on ubuntu
Follow the instructions at https://cvmfs.readthedocs.io/en/stable/cpt-quickstart.html , i.e.:

```
wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb
sudo dpkg -i cvmfs-release-latest_all.deb
rm -f cvmfs-release-latest_all.deb
sudo apt-get update
sudo apt-get install cvmfs
```


Create the new file:
```
/etc/cvmfs/default.local
```

with the following content:
```
CVMFS_REPOSITORIES=cms.cern.ch,cms-ib.cern.ch,grid.cern.ch,sft.cern.ch

# Proxy settings from users inside the cern.ch network (see
# https://twiki.cern.ch/twiki/bin/view/CvmFS/ClientSetupCERN ), 
# with fallback to a direct connection and internal caching
CVMFS_HTTP_PROXY="DIRECT"

# Limit the CVMFS cache to 20 GB (a full CMSSW installation occupias about 16 GB)
CVMFS_QUOTA_LIMIT=20000
```

Then run:
```
sudo cvmfs_config setup
sudo cvmfs_config probe
```


### Install Singularity

Following the instructions at:
https://sylabs.io/guides/3.3/user-guide/installation.html

```
sudo apt-get update && sudo apt-get install -y \
    build-essential \
    libssl-dev \
    uuid-dev \
    libgpgme11-dev \
    squashfs-tools \
    libseccomp-dev \
    pkg-config
```


```
export VERSION=1.16.4 OS=linux ARCH=amd64 && \
    wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
    sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
    rm go$VERSION.$OS-$ARCH.tar.gz
```

```
echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
    echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
    source ~/.bashrc
```

```
export VERSION=3.7.4 && # adjust this as necessary \
    wget https://github.com/sylabs/singularity/releases/download/v${VERSION}/singularity-${VERSION}.tar.gz && \
    tar -xzf singularity-${VERSION}.tar.gz && \
    cd singularity
```

```
./mconfig && \
    make -C ./builddir && \
    sudo make -C ./builddir install
```

For command autocompletion in singularity add the following command to `~/.bashrc`:
```
. /usr/local/etc/bash_completion.d/singularity
```
