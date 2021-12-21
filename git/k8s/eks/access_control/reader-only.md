# 创建eks的只读权限

### Step1: 创建一个只读role叫： pod-reader
```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: '*'
  name: pod-reader
rules:
- apiGroups: ["", "metrics.k8s.io", "extensions", "apps"]  
  resources: 
     - "pods"      
     - "pods/log"
     - "deployments"
     - "events"
     - "replicasets"
  verbs: ["get", "list", "watch"]

```


这个role以后，还需要创建一个rolebinding ，把这个role 和我们的某个Group绑定，例如，group 叫： ops-reader

### Step2: 创建 ClusterRoleBinding 和组

```bash
apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
metadata:
  name: pod-reader-for-ops
subjects:
- kind: Group
  name: ops-reader
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

### Step3： mapping k8s group for iam user

#### 查看
```bash
kubectl get configmap aws-auth -n kube-system -o yaml
```

#### 修改
```
kubectl edit configmap aws-auth -n kube-system -o yaml
```

```
    - userarn: arn:aws:iam::0000000:user/usera
      username: usera
      groups:
        - ops-reader
```


### Step4： 需要的iam权限 [fix user iam ]
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "eks:DescribeNodegroup",
                "eks:ListNodegroups",
                "eks:DescribeCluster",
                "eks:ListClusters",
                "eks:AccessKubernetesApi",
                "ssm:GetParameter",
                "eks:ListUpdates",
                "eks:ListFargateProfiles"
            ],
            "Resource": "*"
        }
    ]
}
```


### Step5： 测试