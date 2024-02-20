<p align="center">
  <img height="100" height="auto" src="https://github.com/freshe4qa/eigenlayer/assets/85982863/c1ab1259-ef5f-470b-bbc9-d37dd88ea04f">
</p>

# Eigenlayer - Testnet

Official documentation:
>- [Guide](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-introduction)

Explorer:
>- [Explorer](https://goerli.eigenlayer.xyz/operator)

### Minimum Hardware Requirements
 - 4x CPUs; the faster clock speed the better
 - 16GB RAM
 - 160GB of storage (SSD or NVME)
 - Ubuntu 20.04

Первый шаг к взаимодействию с тестовой сетью Eigenlayer - это получение gETH. Но для этого вы должны сначала получить Goerli ETH в своем кошельке EVM. Вы можете подключиться к Eigenlayer с помощью кошелька, который вы создадите через ноду.
Чтобы приобрести Goerli ETH, вы можете постепенно запрашивать их с таких кранов, как https://goerlifaucet.com/, или напрямую перевести их с помощью LayerZero: https://testnetbridge.com/.

В качестве оператора Вы теперь можете зарегистрироваться в сети EigenLayer через интерфейс командной строки оператора (CLI). Регистрация оператора не требует авторизации.
Для того чтобы стать оператором в экосистеме EigenLayer, не требуется определенное количество токенов restaked. По сути, любой адрес Ethereum может выступать в качестве оператора.

Теперь пришло время создать учетную запись на API-сервисе, предназначенном для блокчейна Ethereum Goerli. Поскольку Eigenlayer - это сеть второго уровня на блокчейне Ethereum Goerli, ваш узел должен иметь возможность взаимодействовать с уровнем Ethereum Goerli, чтобы обеспечить его нормальное функционирование.
Выполнив этот важный шаг, вы обеспечите бесшовную интеграцию между Eigenlayer и Ethereum Goerli, способствуя плавной работе пользователей и оптимальному участию в сети Eigenlayer.
Вы можете найти провайдера RPC в следующем списке:
https://www.alchemy.com/list-of/rpc-node-providers-on-ethereum
В противном случае я рекомендую использовать этот RPC, предоставленный командой Eigenlayer:
https://rpc.ankr.com/eth_goerli

Установка ноды:

```
sudo apt update && sudo apt upgrade -y
```

```
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
```
sudo apt install docker.io
```
(Пишем Y)
```
docker --version
```
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```
sudo chmod +x /usr/local/bin/docker-compose
```
```
docker-compose --version
```
```
wget https://golang.org/dl/go1.21.4.linux-amd64.tar.gz
```
```
tar -C /usr/local -xzf go1.21.4.linux-amd64.tar.gz
```
```
export PATH=$PATH:/usr/local/go/bin
```
```
go version
```
```
git clone https://github.com/Layr-Labs/eigenlayer-cli.git
cd eigenlayer-cli
mkdir -p build
go build -o build/eigenlayer cmd/eigenlayer/main.go
```
```
cp ./build/eigenlayer /usr/local/bin/
```
```
eigenlayer
```
Вы можете генерировать закрытые ключи с шифрованием ECDSA и BLS с помощью интерфейса командной строки (CLI). ECDSA и BLS - два широко используемых алгоритма в протоколах безопасности и в блокчейне для обеспечения целостности и конфиденциальности данных.

Оба ключа понадобятся для регистрации оператора и других операций на цепочке.
Замените <YOUR_NAME> на имя, которое вы хотите присвоить своим ключам:
```
eigenlayer operator keys create --key-type ecdsa <YOUR_NAME>
```
(Придумайте пароль)
(Сохраните данные в удобное место, которые получили)
```
eigenlayer operator keys create --key-type bls <YOUR_NAME>
```
(Придумайте пароль)
(Сохраните данные в удобное место, которые получили)

После создания пароля для вашего ключа вы увидите приватный ключ и адрес вашего кошелька. Храните свой приватный ключ в надежном месте, так как в дальнейшем вы не сможете получить к нему доступ.
Если вам нужно получить свои открытые ключи и адрес кошелька:
```
eigenlayer operator keys list
```
Чтобы добавить этот кошелек в Metamask, вы можете следовать руководству Metamask:
https://support.metamask.io/hc/en-us/articles/360015489331-How-to-import-an-account#:~:text=Tap%20'Add%20account%20or%20hardware,Import'%20to%20complete%20the%20process

Вам нужно будет импортировать закрытый ключ вашего ECDSA-кошелька. Не забудьте перевести на него немного Geth, чтобы в дальнейшем взаимодействовать с блокчейном.

Вы сможете зарегистрировать своего оператора на блокчейне Eigenlayer.
Начните с генерации необходимых для регистрации файлов:
```
eigenlayer operator config create
```
(Пишем Y)
(Вписываем operator address)
(Нажимаем Enter)
(Вписываем RPC)
(Вписываем путь к ключу ECDSA, пример: /root/.eigenlayer/operator_keys/<NAME_WALLET>.ecdsa.key.json)
(Выбираем сеть Goerli)
```
nano metadata.json
```
Измените файл, добавив в него свои данные.
- Для логотипа вам нужно найти логотип в формате (http(s) + .png).
- Для сайта вы можете использовать предпочитаемый вами сайт.
```
{
  "name": "<OPERATOR_NAME>",
  "website": "<WEBSITE>",
  "description": "<DESCRIPTION>",
  "logo": "<https://www.example.com/logo.png>",
  "twitter": "<YOUR_TWITTER>"
}
```
(Сохраняем CTRL + O)

Далее вам нужно будет создать репозиторий в github как у меня, только со своими данными: https://github.com/freshe4qa/metadata.json/tree/main
После создания всех файлов, нажимаем на metadata.json, далее преобразовываем в Raw ссылку (нажать на Raw), должно получиться так: https://raw.githubusercontent.com/freshe4qa/metadata.json/main/metadata.json

```
nano operator.yaml
```

В поле metadata_url вписываем ссылку, которую получили через Raw.
(Сохраняем CTRL + O)

Теперь вы можете приступить к регистрации своего оператора!
Однако, чтобы покрыть расходы на gas в Goerli, вам понадобится немного gETH в вашем сгенерированном кошельке.

Получите gETH из крана Алхимии => https://goerlifaucet.com/.
Затем переведите gETH на свой кошелек, сгенерированный с помощью Eigenlayer.

Как только у вас появятся gETH в кошельке, начните регистрацию:
```
eigenlayer operator register operator.yaml
```
Чтобы проверить, что ваш оператор успешно зарегистрирован :
```
eigenlayer operator status operator.yaml
```
Вы также можете просмотреть своего Оператора на сайте Eigenlayer!
https://goerli.eigenlayer.xyz/operator

