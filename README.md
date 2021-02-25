# Try out Helm3


### Key features
* Templating
* Environments (Development, Staging, Production)

## Create Chart File Structure

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

### How to override values
```shell script
helm install --values=another-values.yaml mychart
# OR
helm install --set key=new-value mychart
```

* any missing values in *another-values.yaml* will be replaced from *values.yaml*.