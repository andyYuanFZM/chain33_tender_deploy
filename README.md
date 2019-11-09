# chain33联盟链部署
## 同pbft一样，tendermint的节点数也是要满足N>3f，所以至少3f+1个节点

#### 部署4节点的联盟链集群

```ini
下载编译好的文件（在每个节点上下载并解压）
wget https://bty33.oss-cn-shanghai.aliyuncs.com/chain33_tendermint_v6.3.0.tar.gz

解压
tar -zxvf chain33_tendermint_v6.3.0.tar.gz

进入目录
cd chain33_tendermint_v6.3.0/
```

> 文件介绍：
- chain33:   chain33区块链的主程序文件
- chain33-cli: 命令行工具（可以用它来实现对区块链的操作，包括创建账户，转账，发送交易，查询等一系列功能）
- chain33.toml: 主程序对应的配置文件
- genesis.json，priv_validator_1.json， priv_validator_2.json，priv_validator_3.json， priv_validator_4.json： 4个节点需要的公私钥文件


> 备注：
- priv_validator_x.json中是各个节点私钥信息，具有隐私性，不应该公开。 此处只为简化搭建流程。
- 正式环境部署时，参考https://chain.33.cn/document/129  中的流程


> 环境启动前准备工作：
- 修改样例中的配置文件 chain33.toml，将两处的 IP 地址替换为所部署节点的 IP

```ini
......
[p2p]
seeds=["10.0.0.1:13802","10.0.0.2:13802","10.0.0.3:13802","10.0.0.4:13802"]
......
[consensus.sub.tendermint]
validatorNodes=["10.0.0.1:46656","10.0.0.2:46656","10.0.0.3:46656","10.0.0.4:46656"]
......
```

- 第一个节点保留priv_validator_1.json 并将其重命名为 priv_validator.json。 同时可以删除priv_validator_2.json，priv_validator_3.json，priv_validator_4.json
- 第二个节点保留priv_validator_2.json 并将其重命名为 priv_validator.json。 同时可以删除priv_validator_1.json，priv_validator_3.json，priv_validator_4.json
- 第三个节点保留priv_validator_3.json 并将其重命名为 priv_validator.json。 同时可以删除priv_validator_1.json，priv_validator_2.json，priv_validator_4.json
- 第四个节点保留priv_validator_4.json 并将其重命名为 priv_validator.json。 同时可以删除priv_validator_1.json，priv_validator_2.json，priv_validator_3.json


> 启动环境：
- 在4个节点上启动 chain33，顺序不分前后，建议在一分钟内全都启动

```shell
nohup ./chain33 -f chain33.test.toml >> log.out 2>&1 &
```


> 状态检查
- 查询节点是否已经和其它节点同步(5分钟左右)
```shell
$ ./chain33-cli net is_sync
true
```

- 查看各个节点的信息
```shell
./chain33-cli net peer_info
{
    "peers": [
        {
            "addr": "172.18.0.2",
            "port": 13802,
            "name": "0210c1c09b0d61e41d819e28dcbf1148ebae5fb12a8066a9064e7e1c2346432c91",
            "mempoolSize": 100,
            "self": false,
            "header": {
                "version": 0,
                "parentHash": "0x38e72824d7eb1e147ec5b45f281746f934d152d3f1c04d165b9aa326d7cf407d",
                "txHash": "0xfc26ee8f9aab9362e3f6b6289dad94111eedf9278972a61fcfa5a0aae471d3bf",
                "stateHash": "0x88f0b06df8cd2cd6da81e1580e6f179128e42aa8c66e2dba9c38af3e18f9fa44",
                "height": 52510,
                "blockTime": 1545876177,
                "txCount": 101,
                "hash": "0x808a985f4fa63922c96fb68d896a840a637bbf94655c2eccacb876f5a07849af",
                "difficulty": 0
            }
        },
        {
            "addr": "172.18.0.3",
            "port": 13802,
            "name": "03e8c670f6641e2cc73d149610fd81ffb94c20b83e9428bd61f834adcbf33b3927",
            "mempoolSize": 100,
            "self": false,
            "header": {
                "version": 0,
                "parentHash": "0x38e72824d7eb1e147ec5b45f281746f934d152d3f1c04d165b9aa326d7cf407d",
                "txHash": "0xfc26ee8f9aab9362e3f6b6289dad94111eedf9278972a61fcfa5a0aae471d3bf",
                "stateHash": "0x88f0b06df8cd2cd6da81e1580e6f179128e42aa8c66e2dba9c38af3e18f9fa44",
                "height": 52510,
                "blockTime": 1545876177,
                "txCount": 101,
                "hash": "0x808a985f4fa63922c96fb68d896a840a637bbf94655c2eccacb876f5a07849af",
                "difficulty": 0
            }
        },
        {
            "addr": "172.18.0.4",
            "port": 13802,
            "name": "03a4f80a2d73f999b44e3b1137676e00e5eb1357ce22e9a296b5032cf1d128e0dc",
            "mempoolSize": 100,
            "self": false,
            "header": {
                "version": 0,
                "parentHash": "0x38e72824d7eb1e147ec5b45f281746f934d152d3f1c04d165b9aa326d7cf407d",
                "txHash": "0xfc26ee8f9aab9362e3f6b6289dad94111eedf9278972a61fcfa5a0aae471d3bf",
                "stateHash": "0x88f0b06df8cd2cd6da81e1580e6f179128e42aa8c66e2dba9c38af3e18f9fa44",
                "height": 52510,
                "blockTime": 1545876177,
                "txCount": 101,
                "hash": "0x808a985f4fa63922c96fb68d896a840a637bbf94655c2eccacb876f5a07849af",
                "difficulty": 0
            }
        },
        {
            "addr": "172.18.0.5",
            "port": 13802,
            "name": "0382362b8b1d374646f728ae4e93e103f237747ef59a7518117f4c3483cb05a17b",
            "mempoolSize": 100,
            "self": true,
            "header": {
                "version": 0,
                "parentHash": "0x38e72824d7eb1e147ec5b45f281746f934d152d3f1c04d165b9aa326d7cf407d",
                "txHash": "0xfc26ee8f9aab9362e3f6b6289dad94111eedf9278972a61fcfa5a0aae471d3bf",
                "stateHash": "0x88f0b06df8cd2cd6da81e1580e6f179128e42aa8c66e2dba9c38af3e18f9fa44",
                "height": 52510,
                "blockTime": 1545876177,
                "txCount": 101,
                "hash": "0x808a985f4fa63922c96fb68d896a840a637bbf94655c2eccacb876f5a07849af",
                "difficulty": 0
            }
        }
    ]
}
```
