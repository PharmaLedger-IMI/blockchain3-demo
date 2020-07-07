# Anchor API with Ethereum
## Setup a Kubernetes cluster
* Choose our *pl-cluster3*
```
aws eks --region eu-west-2 update-kubeconfig --name pl-cluster3
```
* If you want to deploy your own Kubernetes cluster you can follow this [guideline](https://github.com/PharmaLedger-IMI/blockchain2-demo/k8s_cluster/eks)
## Deploy Ethereum network
Clone our [blockchain3-demo](https://github.com/PharmaLedger-IMI/blockchain3-demo) repo then follow below steps:

Run following command in `blockchain3-demo` directory:
```
helm install my-eth-pharmaledger stable/ethereum -f values.yaml
```
Run following command to list all the services that are running.
```
kubectl get services
```

Above command will output something like below, and please note down `ClusterIP` of `*-ethereum-geth-tx`
```
kubernetes                              ClusterIP      172.20.0.1       <none>                                                                    443/TCP                         12h
my-eth-pharmaledger-ethereum-bootnode   ClusterIP      None             <none>                                                                    30301/UDP,80/TCP                135m
my-eth-pharmaledger-ethereum-ethstats   LoadBalancer   172.20.56.122    a54fc4da99005430baae72cf5bfb0c70-739739716.eu-west-2.elb.amazonaws.com    80:31661/TCP                    135m
my-eth-pharmaledger-ethereum-geth-tx    ClusterIP      172.20.217.232   <none>                                                                    8545/TCP,8546/TCP               135m
```

## Deploy Anchor API
Clone our [blockchain2-demo](https://github.com/PharmaLedger-IMI/blockchain2-demo) (this) repo then follow below steps:

Run following command in `blockchain2-demo` directory to deploy the Anchor API which will use Ethereum network. 
Use `ClusterIP` of `*-ethereum-geth-tx` as host part of `env.web3_provider`.
Use `0x66d66805E29EaB59901f8B7c4CAE0E38aF31cb0e` as value of `env.web3_account`.
Use `lst7upm` as value of `env.web3_password`.
```
helm install --set env.web3_provider=http://172.20.214.92:8545 --set env.web3_account=0x3852360755845889E675C4b683f3F26bf8f12aeA --set env.web3_password=lst7upm anchor-api-ethereum anchor_api/helm-charts/
```

## Optional Configuration

The following table lists the configurable parameters of the vault chart and their default values.

| Parameter                         | Description                                   | Default                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|
| `imagePullPolicy`                 | Container pull policy                         | `IfNotPresent`                        |
| `nodeSelector`                    | Node labels for pod assignmen                 |                                       |
| `bootnode.image.repository`       | bootnode container image to use               | `ethereum/client-go`                  |
| `bootnode.image.tag`              | bootnode container image tag to deploy        | `alltools-v1.7.3`                     |
| `ethstats.image.repository`       | ethstats container image to use               | `ethereumex/eth-stats-dashboard`      |
| `ethstats.image.tag`              | ethstats container image tag to deploy        | `latest`                              |
| `ethstats.webSocketSecret`        | ethstats secret for posting data              | `my-secret-for-connecting-to-ethstats`|
| `ethstats.service.type`           | k8s service type for ethstats                 | `LoadBalancer`                        |
| `geth.image.repository`           | geth container image to use                   | `ethereum/client-go`                  |
| `geth.image.tag`                  | geth container image tag to deploy            | `v1.7.3`                              |
| `geth.tx.replicaCount`            | geth transaction nodes replica count          | `2`                                   |
| `geth.tx.service.type`            | k8s service type for geth transaction nodes   | `ClusterIP`                           |
| `geth.tx.args.rpcapi`             | APIs offered over the HTTP-RPC interface      | `eth,net,web3`                        |
| `geth.miner.replicaCount`         | geth miner nodes replica count                | `3`                                   |
| `geth.miner.account.secret`       | geth account secret                           | `my-secret-account-password`          |
| `geth.genesis.networkId`          | Ethereum network id                           | `98052`                               |
| `geth.genesis.difficulty`         | Ethereum network difficulty                   | `0x0400`                              |
| `geth.genesis.gasLimit`           | Ethereum network gas limit                    | `0x8000000`                         |
| `geth.account.address`            | Geth Account to be initially funded and deposited with mined Ether |                  |
| `geth.account.privateKey`         | Geth Private Key                              |                                       |
| `geth.account.secret`             | Geth Account Secret                           |                                       |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example, to configure the networkid:

```console
$ helm install stable/ethereum --name ethereum --set geth.genesis.networkid=98052
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install stable/ethereum -f values.yaml
```

