
```
aws ecr create-repository --repository-name softward/spark
```



wget https://downloads.apache.org/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz
tar -xvf spark-3.2.0-bin-hadoop3.2.tgz 
cd spark-3.2.0-bin-hadoop3.2
./bin/docker-image-tool.sh -t 3.2.0 -p ./kubernetes/dockerfiles/spark/bindings/python/Dockerfile build


docker tag spark-py:3.2.0 667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark-py:3.2.0
docker push 6?.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark-py:3.2.0
docker tag spark:3.2.0 6?.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.2.0
docker push 667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.2.0



./bin/spark-submit   --master k8s://https://10.100.0.1:443   --deploy-mode cluster --name spark-pi --class org.apache.spark.examples.SparkPi --packages org.apache.hadoop:hadoop-aws:3.2.2 \
--conf spark.kubernetes.file.upload.path=s3a://molbreeding-mbap-test/tmp/ \
--conf spark.hadoop.fs.s3a.access.key=AKIAZWX6U5YVFE2QZUND \
--conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
--conf spark.hadoop.fs.s3a.fast.upload=true \
--conf spark.hadoop.fs.s3a.secret.key=8Zn69gL55aissR5R/FPV2kLuaXPX3s0GGnpWw41S \
--conf spark.executor.instances=5 --conf spark.kubernetes.authenticate.caCertFile=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt --conf spark.kubernetes.authenticate.oauthTokenFile=/var/run/secrets/kubernetes.io/serviceaccount/token   --conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.2.0  ./examples/jars/spark-examples_2.12-3.2.0.jar 



docker build -t spark-client:3.2.0 .


docker tag docker.io/library/spark-client:3.2.0  667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:client_3.2.0
similarface@similarfacedeMacBook-Pro softward % docker push  667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:client_3.2.0





wget https://downloads.apache.org/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz
tar -xvf spark-3.2.0-bin-hadoop3.2.tgz 
cd spark-3.2.0-bin-hadoop3.2

wget  https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.2/hadoop-aws-3.2.2.jar
wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.563/aws-java-sdk-bundle-1.11.563.jar

mv aws-java-sdk-bundle-1.11.563.jar ./jars/
mv hadoop-aws-3.2.2.jar ./jars/
./bin/docker-image-tool.sh  -t 3.2.2_aws -p ./kubernetes/dockerfiles/spark/bindings/python/Dockerfile build




kubectl create namespace spark
kubectl create serviceaccount spark -n spark
kubectl create clusterrolebinding spark-role --clusterrole=edit --serviceaccount=spark:spark --namespace=spark



./bin/spark-submit  --master k8s://https://6638862D097F30696F5405D77439F5A2.gr7.cn-northwest-1.eks.amazonaws.com.cn --deploy-mode cluster --name spark-pi --class org.apache.spark.examples.SparkPi --conf spark.executor.instances=5  --conf spark.kubernetes.namespace=spark --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark --conf spark.kubernetes.authenticate.caCertFile=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt --conf spark.kubernetes.authenticate.oauthTokenFile=/var/run/secrets/kubernetes.io/serviceaccount/token  --conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark-py:3.2.0_aws local:///opt/softward/spark-3.2.0-bin-hadoop3.2/examples/jars/spark-examples_2.12-3.2.0.jar 




./bin/spark-submit  --master k8s://https://6638862D097F30696F5405D77439F5A2.gr7.cn-northwest-1.eks.amazonaws.com.cn --deploy-mode cluster --name spark-pi --class org.apache.spark.examples.SparkPi --conf spark.executor.instances=5  --conf spark.kubernetes.namespace=spark --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark --conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark-py:3.2.0_aws local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar




./bin/spark-submit  --master k8s://https://3F9C76FD9765E615522B8E3372FE3EC0.sk1.cn-northwest-1.eks.amazonaws.com.cn --deploy-mode cluster --name spark-pi --class org.apache.spark.examples.SparkPi --conf spark.executor.instances=5  --conf spark.kubernetes.namespace=spark --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark --conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark-py:3.2.0_aws local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar



./bin/docker-image-tool.sh -r 667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward -t 3.1.2_py -p ./kubernetes/dockerfiles/spark/bindings/python/Dockerfile build

./bin/docker-image-tool.sh -r 667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward -t 3.1.2_py -p ./kubernetes/dockerfiles/spark/bindings/python/Dockerfile push


667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark-py:3.1.2_py



./bin/spark-submit \
--master k8s://https://6638862D097F30696F5405D77439F5A2.gr7.cn-northwest-1.eks.amazonaws.com.cn \
--deploy-mode cluster \
--name spark-pi \
--class org.apache.spark.examples.SparkPi \
--conf spark.executor.instances=5 \
--conf spark.kubernetes.namespace=spark \
--conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
--conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.1.2 \
local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar


/tmp/spark-3.1.2-bin-hadoop3.2

./bin/spark-submit \
--master k8s://https://6638862D097F30696F5405D77439F5A2.gr7.cn-northwest-1.eks.amazonaws.com.cn \
--deploy-mode cluster \
--name spark-pi \
--class org.apache.spark.examples.SparkPi \
--conf spark.executor.instances=5 \
--conf spark.kubernetes.namespace=spark \
--conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
--conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.1.2 \
local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar 10000

```text
similarface@similarfacedeMacBook-Pro spark_c % kubectl get pods -n spark
NAME                               READY   STATUS      RESTARTS   AGE
spark-pi-2e2def7e051ed3e3-driver   0/1     Error       0          48m
spark-pi-64bd0e7e054649b3-driver   0/1     Completed   0          5m40s
spark-pi-c0d4ad7e053f9488-driver   0/1     Completed   0          13m

kubectl logs spark-pi-c0d4ad7e053f9488-driver  -n spark
Pi is roughly 3.1430157150785756
```


docker pull apache/zeppelin:0.10.0
docker tag apache/zeppelin:0.10.0 667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/zeppelin:0.10.0
docker push 667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/zeppelin:0.10.0


















./bin/spark-submit \
--master k8s://https://kubernetes.default.svc \
--deploy-mode cluster \
--name spark-pi \
--class org.apache.spark.examples.SparkPi \
--conf spark.executor.instances=5 \
--conf spark.kubernetes.namespace=spark \
--conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
--conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.1.2 \
local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar 10000



./bin/spark-shell --master k8s://https://kubernetes.default.svc --conf spark.executor.instances=5 --conf spark.kubernetes.namespace=spark --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark --conf spark.kubernetes.container.image=667327721002.dkr.ecr.cn-northwest-1.amazonaws.com.cn/softward/spark:3.1.2