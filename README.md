# Try out Helm3

## Key features
* Templating
* Environments (Development, Staging, Production)

**Table of Contents**

- [Generating a Chart](#generating-a-chart)
    - [Manually](#manually)
    - [Using Helm](#using-helm)
- [Packaging a Chart](#packaging-a-chart)
- [Creating a Release](#creating-a-release)
- [Verifying the Release](#verifying-the-release)
    - [with Helm](#with-helm)
    - [with Kubectl](#with-kubectl)
- [Upgrading the Release](#upgrading-the-release)
- [Remove the Chart](#remove-the-chart)
- [Useful Basic Commands](#useful-basic-commands)
    - [Helm Repositories](#helm-repositories)
        - [Add Repo](#add-repo)
        - [Searching for a Chart](#searching-for-a-chart)
        - [Pulling a Chart](#pulling-a-chart)
    - [How to override chart values](#how-to-override-chart-values)

## Generating a Chart

### Manually
```shell script
mkdir -p mychart/templates mychart/charts
touch mychart/Chart.yaml mychart/values.yaml
```

### Using Helm
`helm create foo`

```shell script
foo/
    ├── .helmignore   # Contains patterns to ignore when packaging Helm charts.
    ├── Chart.yaml    # Information about your chart
    ├── values.yaml   # The default values for your templates
    ├── charts/       # Charts that this chart depends on
    └── templates/    # The template files
        └── tests/    # The test files
```

## Packaging a Chart
`helm package ./mychart`

## Creating a Release
`helm install mychart-0.0.1.tgz --generate-name`

## Verifying the Release
### With Helm
```shell script
# Information and status about chart
$ helm list

NAME                	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART        	APP VERSION
mychart-0-1614243590	default  	1       	2021-02-25 09:59:51.519182 +0100 CET	deployed	mychart-0.0.1	  

# To see "Computed Values" and pod yaml, etc
$ helm get all <helm-chart-generated-name>
```

```yaml
NAME: mychart-0-1614243590
LAST DEPLOYED: Thu Feb 25 09:59:51 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
null

COMPUTED VALUES:
container:
  image: nginx
  name: nginx
  port: 80
name: mypod

HOOKS:
MANIFEST:
---
# Source: mychart/templates/hello-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

### With Kubectl
```shell script
kubectl get pods
kubectl port-forward pod/mypod 8080:80
```

## Upgrading the Release

1. Updating pod yaml by adding label for example
2. Increasing chart version from `0.0.1` to `0.0.2` in `Chart.yaml`
3. Package Chart `helm package ./mychart`
4. Upgrading the Release `helm upgrade <auto-generated-chart-name> mychart-0.0.2.tgz`
5. Verifying the Release
    1. `REVISION` in `helm list` is changed to `2`
    2. `CHART` is changed to `mychart-0.0.2`

## Remove the Chart
`helm uninstall <auto-generated-chart-name>`

## Useful Basic Commands

### Helm Repositories

#### Add Repo

```shell script
# helm repo add [NAME] [URL]`
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

#### Searching for a Chart

`helm search repo nginx` # from added repo
`helm search hub nginx`  # from Helm Hub

#### Pulling a Chart
Download a chart from a repository and (optionally) unpack it in local directory without installing it.
                     
`helm pull <chart-name> --untar`

#### How to override chart values
```shell script
helm install --values=another-values.yaml mychart
# OR
helm install --set key=new-value mychart
```

* any missing values in *another-values.yaml* will be replaced from *values.yaml*.
