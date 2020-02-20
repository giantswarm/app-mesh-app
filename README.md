[![CircleCI](https://circleci.com/gh/giantswarm/app-mesh-app.svg?style=shield)](https://circleci.com/gh/giantswarm/app-mesh-app)

# app-mesh-app chart

[App mesh](https://github.com/aws/eks-charts) is an open-source Service Mesh for Kubernetes that offers traffic shifting and security capabilities.

## Requirements

- You need to provision a role in the cluster (in case you use [kiam]()) or modify the instance role of the worker machines to add the permissions detailed below.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "appmesh:*",
                "servicediscovery:CreateService",
                "servicediscovery:GetService",
                "servicediscovery:RegisterInstance",
                "servicediscovery:DeregisterInstance",
                "servicediscovery:ListInstances",
                "route53:GetHealthCheck",
                "route53:CreateHealthCheck",
                "route53:UpdateHealthCheck",
                "route53:ChangeResourceRecordSets",
                "route53:DeleteHealthCheck"
            ],
            "Resource": "*"
        }
    ]
}
```

## Installation

### Dependencies

In case you want the auto injection of sidecars, you need to deploy [app-mesh-inject](https://github.com/aws/eks-charts/tree/master/stable/appmesh-inject).

```bash
helm repo add eks https://aws.github.io/eks-charts
```

```bash
helm upgrade -i appmesh-inject eks/appmesh-inject \
--namespace <NAMESPACE_OF_MESH_CONTROLLER> \
--set mesh.name=<NAME_OF_YOUR_MESH>
```

### Deployment

There are two options for deployment. In case you have add the policies needed in the instance role of your workers you can use:

```text
helm upgrade -i --namespace <NAMESPACE_OF_MESH_CONTROLLER> giantswarm-playground-catalog/app-mesh-app --set region=<AWS_REGION_OF_YOUR_CLUSTER> --set region=<AWS_REGION_OF_YOUR_CLUSTER>
```

In case you use `kiam`, you need to pass the role name in the pod Annotations:

```text
helm upgrade -i --namespace <NAMESPACE_OF_MESH_CONTROLLER> giantswarm-playground-catalog/app-mesh-app --set region=<AWS_REGION_OF_YOUR_CLUSTER> --set "podAnnotations.iam\.amazonaws\.com/role"=<ROLE_NAME>
```

## Configuration

Two installation options, `region` and `podAnnotations.iam\.amazonaws\.com/role` (in case `kiam` is used) are required to install this chart,
as shown above. You shouldn't need to change other defaults. Still, you can check available chart
options [here](helm/app-mesh-app/values.yaml).

## Compatibility

Tested on Giant Swarm releases `10.1.1` and `11.0.0` on AWS with Kubernetes `1.15.5` and `1.16.3` respectively.

## Credit

* https://github.com/aws/eks-charts/
