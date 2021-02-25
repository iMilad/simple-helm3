# Try out Helm3

## Key features
* Templating
* Environments (Development, Staging, Production)

## Helm Chart Steps
1. Generate Chart
2. Packaging Chart
3. Creating a Release
4. Verifying
5. Upgrading a Release

### Generating Chart

#### Manually Empty chart
```shell script
mkdir -p mychart/templates mychart/charts
touch mychart/Chart.yaml mychart/values.yaml
```

#### Generate by helm
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

#### How to override values
```shell script
helm install --values=another-values.yaml mychart
# OR
helm install --set key=new-value mychart
```

* any missing values in *another-values.yaml* will be replaced from *values.yaml*.

### Packaging Chart
`helm package ./mychart`

### Creating a Release
`helm install mychart-0.0.1.tgz --generate-name`

### Verifying
#### Using Helm
```shell script
# Information and status about chart
$ helm list

NAME                	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART        	APP VERSION
mychart-0-1614243590	default  	1       	2021-02-25 09:59:51.519182 +0100 CET	deployed	mychart-0.0.1	  

# To see "Computed Values" and pod yaml, etc
$ helm get all <helm-chart-generated-name>

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

#### Using Kubectl
```shell script
kubectl get pods
kubectl port-forward pod/mypod 8080:80
```

### Upgrading a Release
