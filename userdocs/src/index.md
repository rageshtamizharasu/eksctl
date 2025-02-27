# eksctl - The official CLI for Amazon EKS

sponsored by [![Weaveworks](img/empty.svg#wwinline)](https://www.weave.works/) and built by [![Contributors](img/gophers.png#inline)](https://github.com/weaveworks/eksctl/graphs/contributors) on [![Github](img/empty.svg#gitinline)](https://github.com/weaveworks/eksctl)

!!! tip "New for 2023"
    `eksctl` now supports creating fully private clusters on [AWS Outposts](/usage/outposts).

    `eksctl` now supports new regions - Zurich (`eu-central-2`), Spain (`eu-south-2`), Hyderabad (`ap-south-2`) and  Melbourne (`ap-southeast-4`).

`eksctl` is a simple CLI tool for creating and managing clusters on EKS - Amazon's managed Kubernetes service for EC2.
It is written in Go, uses CloudFormation, was created by [Weaveworks](https://www.weave.works/) and it welcomes
contributions from the community.

!!! example "Create a basic cluster in minutes with just one command"
    ```
    eksctl create cluster
    ```
    ![eksctl create cluster](img/eksctl-gopher.png){ align=right }

    A cluster will be created with default parameters:

    - exciting auto-generated name, e.g., `fabulous-mushroom-1527688624`
    - two `m5.large` worker nodes (this instance type suits most common use-cases, and is good value for money)
    - use the official AWS [EKS AMI](https://github.com/awslabs/amazon-eks-ami)
    - `us-west-2` region
    - a dedicated VPC (check your quotas)

Example output:

```
$ eksctl create cluster
[ℹ]  using region us-west-2
[ℹ]  setting availability zones to [us-west-2a us-west-2c us-west-2b]
[ℹ]  subnets for us-west-2a - public:192.168.0.0/19 private:192.168.96.0/19
[ℹ]  subnets for us-west-2c - public:192.168.32.0/19 private:192.168.128.0/19
[ℹ]  subnets for us-west-2b - public:192.168.64.0/19 private:192.168.160.0/19
[ℹ]  nodegroup "ng-98b3b83a" will use "ami-05ecac759c81e0b0c" [AmazonLinux2/1.11]
[ℹ]  creating EKS cluster "floral-unicorn-1540567338" in "us-west-2" region
[ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial nodegroup
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-west-2 --cluster=floral-unicorn-1540567338'
[ℹ]  2 sequential tasks: { create cluster control plane "floral-unicorn-1540567338", create nodegroup "ng-98b3b83a" }
[ℹ]  building cluster stack "eksctl-floral-unicorn-1540567338-cluster"
[ℹ]  deploying stack "eksctl-floral-unicorn-1540567338-cluster"
[ℹ]  building nodegroup stack "eksctl-floral-unicorn-1540567338-nodegroup-ng-98b3b83a"
[ℹ]  --nodes-min=2 was set automatically for nodegroup ng-98b3b83a
[ℹ]  --nodes-max=2 was set automatically for nodegroup ng-98b3b83a
[ℹ]  deploying stack "eksctl-floral-unicorn-1540567338-nodegroup-ng-98b3b83a"
[✔]  all EKS cluster resource for "floral-unicorn-1540567338" had been created
[✔]  saved kubeconfig as "~/.kube/config"
[ℹ]  adding role "arn:aws:iam::376248598259:role/eksctl-ridiculous-sculpture-15547-NodeInstanceRole-1F3IHNVD03Z74" to auth ConfigMap
[ℹ]  nodegroup "ng-98b3b83a" has 1 node(s)
[ℹ]  node "ip-192-168-64-220.us-west-2.compute.internal" is not ready
[ℹ]  waiting for at least 2 node(s) to become ready in "ng-98b3b83a"
[ℹ]  nodegroup "ng-98b3b83a" has 2 node(s)
[ℹ]  node "ip-192-168-64-220.us-west-2.compute.internal" is ready
[ℹ]  node "ip-192-168-8-135.us-west-2.compute.internal" is ready
[ℹ]  kubectl command should work with "~/.kube/config", try 'kubectl get nodes'
[✔]  EKS cluster "floral-unicorn-1540567338" in "us-west-2" region is ready
```

Customize your cluster by using a config file. Just run

```
eksctl create cluster -f cluster.yaml
```

to apply a `cluster.yaml` file:

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: basic-cluster
  region: eu-north-1

nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 10
  - name: ng-2
    instanceType: m5.xlarge
    desiredCapacity: 2
```

Once you have created a cluster, you will find that cluster credentials were added in `~/.kube/config`. If you have
`kubectl` v1.10.x as well as `aws-iam-authenticator` commands in your PATH, you should be
able to use `kubectl`. You will need to make sure to use the same AWS API credentials for this also. Check
[EKS docs][ekskubectl] for instructions. If you installed `eksctl` via Homebrew, you should have all of these
dependencies installed already.

To learn more about how to create clusters and other features continue reading the
[Creating and Managing Clusters section](usage/creating-and-managing-clusters).

[ekskubectl]: https://docs.aws.amazon.com/eks/latest/userguide/configure-kubectl.html

_Need help? Join [Eksctl Slack][slackjoin]._
[slackjoin]: https://slack.k8s.io/
