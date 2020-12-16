## Using Config

```
eksctl create cluster -f cluster.yaml
eksctl delete cluster -f cluster.yaml
eksctl get cluster


```
## Using only cli

```
eksctl create cluster \
--version 1.17 \
--name prod-eks-cluster \
--region eu-west-1 \
--nodegroup-name eks-ec2-linux-nodes \
--node-type t3.medium \
--nodes 2 \
--nodes-min 1 \
--nodes-max 3 \
--ssh-access \
--ssh-public-key ~/.ssh/eks.pub \
--managed \
--auto-kubeconfig \
--node-private-networking \
--verbose 3
```


If you needed to use an existing VPC, you can use a config file like this:

```
apiVersion: eksctl.io/v1alpha3
kind: ClusterConfig

metadata:
  name: cluster-in-existing-vpc
  region: eu-north-1

vpc:
  subnets:
    private:
      eu-north-1a: {id: subnet-0ff156e0c4a6d300c}
      eu-north-1b: {id: subnet-0549cdab573695c03}
      eu-north-1c: {id: subnet-0426fb4a607393184}

nodeGroups:
  - name: ng-1-workers
    labels: {role: workers}
    instanceType: m5.xlarge
    desiredCapacity: 10
    privateNetworking: true
  - name: ng-2-builders
    labels: {role: builders}
    instanceType: m5.2xlarge
    desiredCapacity: 2
    privateNetworking: true
    iam:
      withAddonPolicies:
        imageBuilder: true

```