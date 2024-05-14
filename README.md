# initia-Node

| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| Ubuntu 20.04.2 |
| CPU |	2 |
| RAM	| 4 GB |
| Storage	| 100 GB SSD |

# Sunucumuzu Güncelliyoruz.

```
apt-get update
```

```
apt-get -y update && apt-get -y upgrade
```

```
apt-get install libav-tools
```

```
sudo apt-get install make
```

```
sudo apt install -y build-essential
```

```
make --version
```

# GO'yu Kuruyoruz.

```
cd $HOME && \
ver="1.21.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

# Repoyu Klonluyoruz.

```
git clone https://github.com/initia-labs/initia
```




