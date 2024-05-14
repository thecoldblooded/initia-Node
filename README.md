# initia-Node

| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| Ubuntu 20.04.2 |
| CPU |	4 |
| RAM	| 6 GB |
| Storage	| 180 GB SSD |

# Sunucumuzu Güncelliyoruz.

```
sudo apt update
```

```
sudo apt install curl git jq build-essential gcc unzip wget lz4 -y
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

Klasöre giriyoruz.

```
cd initia
```

```
git checkout v0.2.12
```

```
make install
```

Version v0.2.11 olmalı.

```
initiad version
```

Şu kısımları duzenleyin.
Nodeismi, Gözükmesini istediğiniz isim.
Cüzdan yazan yere, Cüzdan isminizi yazın.

```
echo 'export MONIKER="Nodeismi"' >> ~/.bash_profile
echo 'export CHAIN_ID="initiation-1"' >> ~/.bash_profile
echo 'export WALLET_NAME="Cüzdan"' >> ~/.bash_profile
source $HOME/.bash_profile
```

```
cd
```

# Genesis Dosyasını Yüklüyoruz.

```
wget https://initia.s3.ap-southeast-1.amazonaws.com/initiation-1/genesis.json -O $HOME/.initia/config/genesis.json
```

Peer Ekliyoruz.

```
PEERS="40d3f977d97d3c02bd5835070cc139f289e774da@168.119.10.134:26313,841c6a4b2a3d5d59bb116cc549565c8a16b7fae1@23.88.49.233:26656,e6a35b95ec73e511ef352085cb300e257536e075@37.252.186.213:26656,2a574706e4a1eba0e5e46733c232849778faf93b@84.247.137.184:53456,ff9dbc6bb53227ef94dc75ab1ddcaeb2404e1b0b@178.170.47.171:26656,edcc2c7098c42ee348e50ac2242ff897f51405e9@65.109.34.205:36656,07632ab562028c3394ee8e78823069bfc8de7b4c@37.27.52.25:19656,028999a1696b45863ff84df12ebf2aebc5d40c2d@37.27.48.77:26656,140c332230ac19f118e5882deaf00906a1dba467@185.219.142.119:53456,1f6633bc18eb06b6c0cab97d72c585a6d7a207bc@65.109.59.22:25756,065f64fab28cb0d06a7841887d5b469ec58a0116@84.247.137.200:53456,767fdcfdb0998209834b929c59a2b57d474cc496@207.148.114.112:26656,093e1b89a498b6a8760ad2188fbda30a05e4f300@35.240.207.217:26656,12526b1e95e7ef07a3eb874465662885a586e095@95.216.78.111:26656" && \
SEEDS="2eaa272622d1ba6796100ab39f58c75d458b9dbc@34.142.181.82:26656,c28827cb96c14c905b127b92065a3fb4cd77d7f6@testnet-seeds.whispernode.com:25756" && \
sed -i \
    -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" \
    -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" \
    "$HOME/.initia/config/config.toml"
```

Minimum Gas Priceyi Ayarlıyoruz.

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.15uinit,0.01uusdc\"/" $HOME/.initia/config/app.toml
```

Service Dosyamızı Oluşturuyoruz.

```
sudo tee /etc/systemd/system/initiad.service > /dev/null <<EOF
[Unit]
Description=initia node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which initiad) start
Restart=on-failure
RestartSec=10
LimitNOFILE=10000

[Install]
WantedBy=multi-user.target
EOF
```

Restart Atıyoruz.

```
sudo systemctl daemon-reload
```

```
sudo systemctl enable initiad 
```

```
sudo systemctl start initiad
```

# Screen Açıyoruz.

```
screen -S init
```

Log görüntülemek için.

```
sudo journalctl -u initiad -f -o cat
```


# Validator Kuracağız
Not: Eşitlenene Kadar bekliyoruz.
Kontrol Etmek için initia Scanı kullanabilirsiniz.
https://scan.initia.tech/initiation-1

# Cüzdan Oluşturuyoruz.

Kodu Yazdıktan Sonra şifre soracak 2 kere aynı şifreyi girin ve 24 Kelimenizi Kaydedin.

```
initiad keys add $WALLET_NAME
```

Daha Sonra Cüzdanınıza Faucet alın Faucet Linki:
https://faucet.testnet.initia.xyz/

Validator Oluşturma Komutu,

```
initiad tx checkpointing create-validator \
--amount=1000000000000uinit \
--pubkey=$(initiad tendermint show-validator) \
--moniker="Nodeisminiz" \
--chain-id=initiation-1 \
--commission-rate="0.10" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.1" \
--min-self-delegation="1" \
--fees=500000uinit \
--gas=200000 \
--from=Cüzdanismi \
--website="websiteBilginiz" \
--details="Detayları Yazın " \
--identity="Keybaseİdiniz" \
-y
```








