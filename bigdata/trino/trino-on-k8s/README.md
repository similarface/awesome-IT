# trino-on-k8s

Setup for running TrinoDB (formerly Prestosql) with Hive Metastore on Kubernetes as introduced
in [this blog post](https://medium.com/@joshua_robinson/presto-powered-s3-data-warehouse-on-kubernetes-aea89d2f40e8).

See [previous blog post](https://medium.com/@joshua_robinson/presto-on-flashblade-s3-486ecb449574)
for more information about running Trino/Presto on FlashBlade.

# How to Use

1. Build Docker image for Hive Metastore

2. Deploy Hive Metastore: MariaDB (pvc and deployment), init-schemas, Metastore

3. Deploy Trino services (coordinator, workers, and cli)

4. Deploy Redash.

Assumptions: working Kubernetes deployment and S3 object store (e.g., FlashBlade).

Things you may need to modify:

* Docker repository name ($REPONAME) in build_image scripts and yaml files.
* DataVIP and access keys for FlashBlade (fs.s3a.endpoint and hive.s3a.endpoint)
* StorageClass for the MariaDB volume.
* Memory settings and worker counts.

# Hive Metastore Service

Dockerfile for Metastore

* Uses [Hive Metastore Standalone service](https://cwiki.apache.org/confluence/display/Hive/AdminManual+Metastore+3.0+Administration).

Yaml for MariaDB

* Simple and not optimized.

Yaml for init-schemas

* One-time K8s job to initiate the MariaDB tables.

Yaml for Metastore service

# Trino Coordinator/Workers/CLI

Leverages the official [Trino Docker image](https://github.com/trinodb/docker-images).

Yaml for Trino Coordinator/Workers

Trino CLI pod

Create SQL shell as:
```kubectl exec -it pod/trino-cli -- trino --server trino:8080 --catalog hive --schema default```


[comment]: <> (kubectl create secret generic my-s3-keys --from-literal=access-key='' --from-literal=secret-key='')


## final

- create  namespace

```base
kubectl create namespace trino
```
```bash
kubectl create secret generic my-s3-keys --from-literal=access-key='' --from-literal=secret-key=''
```

- create storageclass

```bash
#kubectl apply -f  maria_pvc/storageclass.yaml  -n trino
```

- create configmap metastore-cfg

```bash
cd hive_metastore
kubectl -n trino create configmap metastore-cfg --dry-run --from-file=metastore-site.xml --from-file=core-site.xml -o yaml | kubectl -n trino apply -f - 
```

- create configmap trino-configs

```bash
kubectl apply -f trino-cfgs.yaml -n trino
```

- create pvc

```bash
# kubectl apply -f  maria_pvc/maria_pvc.yaml  -n trino
```

- create maria db

```bash
kubectl apply -f maria_pvc/maria_deployment.yaml   -n trino
```

- init-schemas

```bash
kubectl apply -f hive-initschema.yaml -n trino
```

- Metastore

```bash
kubectl apply -f metastore.yaml -n trino
```

- coordinator & workers & cli

```bash
kubectl apply -f trino.yaml  -n trino
```


```bash 
kubectl -n trino exec -it pod/trino-cli -- trino --server trino:8080 --catalog hive --schema default
```

```sybase
show catalogs;
SHOW SCHEMAS FROM tpch;
SHOW SCHEMAS FROM hive;


USE  hive.projectpq;


```


```sql
select count(*) from  tpch.sf100.customer; --15000000
select * from  tpch.sf100.customer where custkey=1875009;
```