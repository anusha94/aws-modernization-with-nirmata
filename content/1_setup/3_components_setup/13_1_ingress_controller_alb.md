---
title: "Deploy Ingress Controller"
chapter: false
weight: 50
---

### Deploy the AWS Load Balancer Controller

AWS Load Balancer Controller is a controller to help manage Elastic Load Balancers for a Kubernetes cluster.

The controller can provision the following resources:

An AWS Application Load Balancer when you create a Kubernetes Ingress.
An AWS Network Load Balancer when you create a Kubernetes Service of type LoadBalancer.
Application Load Balancers work at L7 of the OSI model, allowing you to expose Kubernetes service using ingress rules, and supports external-facing traffic. Network load balancers work at L4 of the OSI model, allowing you to leverage Kubernetes Services to expose a set of pods as an application network service.

The controller enables you to simplify operations and save costs by sharing an Application Load Balancer across multiple applications in your Kubernetes cluster.

Run the command below to create the IAM roles required for the workshop.

```sh
prepare-environment exposing/load-balancer && prepare-environment fundamentals/storage/ebs
```


#### Deploy the Helm chart

The Helm chart will be deployed from the EKS repo.

```bash
helm repo add eks-charts https://aws.github.io/eks-charts

helm upgrade --install aws-load-balancer-controller eks-charts/aws-load-balancer-controller \
  --version "${LBC_CHART_VERSION}" \
  --namespace "kube-system" \
  --set "clusterName=${EKS_CLUSTER_NAME}" \
  --set "serviceAccount.name=aws-load-balancer-controller-sa" \
  --set "serviceAccount.annotations.eks\\.amazonaws\\.com/role-arn"="$LBC_ROLE_ARN" \
  --wait
```

Once successfully installed, you should see similar output below.

```bash
Release "aws-load-balancer-controller" does not exist. Installing it now.
NAME: aws-load-balancer-controller
LAST DEPLOYED: Fri Oct  4 19:35:37 2024
NAMESPACE: kube-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
AWS Load Balancer controller installed!
```

Navigate to the next section to install the [Amazon EBS CSI Driver](13_2_ebs_csi_driver.html).

