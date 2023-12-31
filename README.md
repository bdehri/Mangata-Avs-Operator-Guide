# Mangata-Avs-Operator-Guide

## Pre-requisites

- Ethereum Goerli RPC provider account
- Ubuntu 22 Server

1. Install Unix Tools and Docker

```bash
sudo apt-get update
sudo apt-get install -y curl wget git make build-essential
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

2. Install Golang

You can skip this step if you already have Golang installed.

If the following command prints an output, you can skip this step.

```bash
which go
```

Otherwise, you can install Golang like below.

```bash
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> ~/.bashrc
which go
```

3. Install Eigenlayer CLI

```bash
git clone https://github.com/Layr-Labs/eigenlayer-cli.git
cd ~/eigenlayer-cli
make install
sudo cp bin/eigenlayer /usr/local/bin/
```

3. Prepare Eigenlayer

We need to create ECDSA and BLS keys first.

```bash
eigenlayer operator keys create --key-type ecdsa <name>
eigenlayer operator keys create --key-type bls <name>
```

Then, we need to create the config files via eigenlayer cli.

```bash
eigenlayer operator config create
```

This command will create two files: operator.yaml and metadata.json
Sample metadata.json and operator.yaml files are provided in this repository.

You need to upload the metadata.json and your logo to publicly accessible location and put the url of metadata.json at the metadata_url field in operator.yaml file.

Afterwards, you can create the operator via eigenlayer cli.

```bash
eigenlayer operator register operator.yaml
```

You can check the status of your operator like below.

```bash
eigenlayer operator status operator.yaml
```

4. Install Mangata

First, we download the Mangata Avs Operator repo.

```bash
cd ~
git clone https://github.com/mangata-finance/avs-operator-setup.git
cd avs-operator-setup
chmod +x run.sh
```

Then, we need to edit .env file with the information already present at operator.yaml file.

Afterwards, we can run the operator.

```bash
./run.sh opt-in
docker compose up -d
```

You can check the logs of the operator like below.

```bash
docker logs mangata-finalizer-node
```