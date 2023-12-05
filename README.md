# charts
AutoMQ charts repository for Kafka and RocketMQ.

## Usage

### Pre-requirement

[Helm](https://helm.sh) must be installed to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

Once Helm is set up properly, add the repo as follows:

```console
helm repo add automq https://charts.automq.com
helm repo update
```

You can then run `helm search repo automq` to see the charts.

### automq-for-rocketmq

Install `automq-for-rocketmq` as follows:
```shell
helm install automq-for-rocketmq automq/automq-for-rocketmq
```

Deploying with default values creates the following three pods:
- MySQL
- Localstack
- AutoMQ Broker

### Parameter Configuration
#### Use Custom Database
By default values will deploy MySQL for metadata storage. 

If you want to use an existing database, you can refer to the following method to configure it.

1. Download ddl.sql and create the database yourself
```shell
wget https://raw.githubusercontent.com/AutoMQ/automq-for-rocketmq/main/metadata-jdbc/src/main/resources/ddl.sql
```

2. Create custom values

```yaml
# Custom-values.yaml
broker:
  db:
    url: "jdbc:mysql://<mysql-endpoint>/<database>"
    userName: "<user>"
    password: "<password>"
```

3. Deploy using custom values
```shell
helm install -f custom-values.yaml automq-for-rocketmq automq/automq-for-rocketmq
```
#### Use S3 and Other Compatible Services

The default values will deploy Localstack for data storage. 

If you want to use AWS S3 or other S3 API compatible services such as Minio, you can refer to the following method to configure it.

1. Create custom values
```yaml
# Custom-values.yaml
broker:
  s3:
    endpoint: "<s3-service-endpoint>"
    bucket: "<bucket-name>"
    region: "<region-id>"
    forcePathStyle: true
    accessKey: "<access-key>"
    secretKey: "<secret-key>"
```
2. Deploy using custom values
```shell

helm install -f custom-values.yaml automq-for-rocketmq automq/automq-for-rocketmq
```