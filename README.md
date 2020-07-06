```console
$ helm install my-eth-pharmaledger stable/ethereum -f values.yaml
$ kubectl apply -f ethereum-geth-tx-loadbalancer.yaml
```



## Configuration

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

