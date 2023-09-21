# Obserrvability with Quarkus

Configuring observability with Quarkus, Grafana, and Prometheus allows you to monitor and collect metrics from your Quarkus application effectively. Here's a step-by-step example with detailed explanations:

## 1. Configure Quarkus for Prometheus

Add the following dependency to your Quarkus project's pom.xml to enable support for Prometheus:
```
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-micrometer-registry-prometheus</artifactId>
</dependency>
```
In your application.properties or application.yml file, configure the Prometheus extension:
```
quarkus.micrometer.export.prometheus.enabled=true
```
## 2. Configure Prometheus

Download Prometheus from https://prometheus.io/download/ and install it according to your operating system's instructions.

Create a prometheus.yml configuration file to specify the targets for metrics collection. Here's a basic example:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'quarkus-prometheus-example'
    metrics_path: '/q/metrics'
    static_configs:
      - targets: ['localhost:8080'] # Your Quarkus application
```

## 3. Configure Grafana

Download Grafana from https://grafana.com/grafana/download and install it as per the provided instructions.

Access the Grafana dashboard at http://localhost:3000 (default) and log in using the default credentials (admin/admin).

Add a new Prometheus data source:

* Click "Configuration" in the left sidebar.
* Under "Data Sources," click "Add data source."
* Choose "Prometheus" as the data source type.
* Configure the Prometheus URL (typically http://localhost:9090 by default).


Create a new dashboard:

* Click "Create" in the left sidebar.
* Choose "Add new panel."
* Under "Query," select "Prometheus" as the data source and enter a valid Prometheus query, e.g., http_requests_total.
* Save the panel with a descriptive name.

## 4. Run Your Quarkus Application

Now, run your Quarkus application.

## 5. Visualize Metrics in Grafana

Go back to Grafana and access your dashboard. You will see graphs and metrics related to your Quarkus application.

This is a comprehensive example of how to set up observability with Quarkus, Grafana, and Prometheus. In production environments, you can configure alerts, create custom dashboards, and explore more advanced metrics. Be sure to refer to the official documentation of Quarkus, Prometheus, and Grafana for detailed configuration options and advanced features.














⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
