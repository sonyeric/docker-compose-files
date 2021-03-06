# Hyperledger fabric

You can use the following script to install docker and start a 4-node PBFT cluster in one instruction.

```sh
$ bash setupPbft.sh
```

## Download Images
If you want to start it manually, follow the steps:

First, pull necessary images first. You can ignore this step if pulling official image already.

*The latest code is evolving quickly, we use the 0.6 branch code.*

```sh
$ docker pull yeasy/hyperledger-fabric:0.6-dp
$ docker tag yeasy/hyperledger-fabric:0.6-dp hyperledger/fabric-peer:latest
$ docker tag yeasy/hyperledger-fabric:0.6-dp hyperledger/fabric-baseimage:latest
$ docker tag yeasy/hyperledger-fabric:0.6-dp hyperledger/fabric-membersrvc:latest
```

### Start 4 Noops node cluster

```sh
$ cd noops; docker-compose up
```

### Start 4 PBFT node cluster

```sh
$ cd pbft; docker-compose up
```

After the cluster is synced, you can validate by deploying, invoking or querying chaincode from the container or from the host.

See [hyperledger-fabric](https://github.com/yeasy/docker-hyperledger-fabric) if you've not familiar on those operations.


### Start 4 PBFT node cluster with member service

```sh
$ cd pbft; docker-compose -f docker-compose-with-membersrvc.yml up
```

Then go to vp0, login and deploy a chaincode.

```sh
$ docker exec -it pbft_vp0_1 bash
# peer network login jim
08:23:13.604 [networkCmd] networkLogin -> INFO 001 CLI client login...
08:23:13.604 [networkCmd] networkLogin -> INFO 002 Local data store for client loginToken: /var/hyperledger/production/client/
Enter password for user 'jim': 6avZQLwcUe9b
...

# peer chaincode deploy -u jim -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 -c '{"Function":"init", "Args": ["a","100", "b", "200"]}'
```

4 new chaincode containers will be built up automatically.

## Acknowledgement
This refers the example from the [hyperledger](https://github.com/hyperledger/fabric/tree/master/consensus/docker-compose-files) project.
