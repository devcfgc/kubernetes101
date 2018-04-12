## Let's add Prometheus as a datasource. ##

- Click on the icon in the upper left of grafana and go to "Data Sources".
- Click "Add data source".
- Name: prometheus
- Type : Prometheus
- For the URL, we will actual use Kubernetes DNS service discovery. So, just enter http://prometheus:9090.
  This means that grafana will lookup the prometheus service running in the same namespace on port 9090.
- Import kubernetes-cluster-monitoring-via-prometheus_rev2.json
