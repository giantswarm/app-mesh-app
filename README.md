# App Mesh Controller

App Mesh controller Helm chart for Kubernetes

## Prerequisites

* Kubernetes >= 1.13
* IAM policies

```json
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

## Installing the Chart

Add the EKS repository to Helm:

```sh
helm repo add playground https://giantswarm.github.io/giantswarm-playground-catalog/
```

Install the App Mesh CRDs:

```sh
kubectl apply -k github.com/giantswarm/app-mesh-app/stable/appmesh-controller/crds?ref=master
```

Install the App Mesh CRD controller:

```sh
helm upgrade -i appmesh-controller playground/appmesh-controller \
--namespace appmesh-system
```

The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `appmesh-controller` deployment:

```console
$ helm delete --purge appmesh-controller
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the chart and their default values.

Parameter | Description | Default
--- | --- | ---
`image.repository` | image repository | ` 602401143452.dkr.ecr.us-west-2.amazonaws.com/amazon/app-mesh-controller`
`image.tag` | image tag | `<VERSION>`
`image.pullPolicy` | image pull policy | `IfNotPresent`
`resources.requests/cpu` | pod CPU request | `100m`
`resources.requests/memory` | pod memory request | `64Mi`
`resources.limits/cpu` | pod CPU limit | `2000m`
`resources.limits/memory` | pod memory limit | `1Gi`
`affinity` | node/pod affinities | None
`nodeSelector` | node labels for pod assignment | `{}`
`podAnnotations` | annotations to add to each pod | `{}`
`tolerations` | list of node taints to tolerate | `[]`
`rbac.create` | if `true`, create and use RBAC resources | `true`
`rbac.pspEnabled` | If `true`, create and use a restricted pod security policy | `false`
`serviceAccount.create` | If `true`, create a new service account | `true`
`serviceAccount.name` | Service account to be used | None


