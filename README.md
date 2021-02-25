# Try out Helm3


### Key features
* Templating
* Environments (Development, Staging, Production)

## Create Chart Structure

```shell script
mkdir -p mychart/templates mychart/charts
touch mychart/Chart.yaml mychart/values.yaml
```

```shell script
mychart           // name of chart
├── Chart.yaml    // Description of the chart
├── charts        // May contain other charts
├── templates     // Template rendring engine folder
└── values.yaml   // Default values for a chart, can be overriden during <helm install> or <helm upgrade>
```