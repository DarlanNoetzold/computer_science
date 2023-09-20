# Obervability with Spring

## 1. Configure Spring Boot for Prometheus

Add the following dependencies to your pom.xml to enable Spring Boot's support for Prometheus:
```
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
In your application.properties or application.yml file, add the following configurations to expose metrics in Prometheus format:
properties
Copy code
management.endpoints.web.exposure.include=*
management.endpoint.metrics.enabled=true
management.metrics.export.prometheus.enabled=true
```

## 2. Configure Prometheus

Download Prometheus from https://prometheus.io/download/ and follow the installation instructions for your operating system.

Create a prometheus.yml configuration file to define the metrics collection targets. Here's a simple example:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'spring-prometheus-example'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['localhost:8080'] # Your Spring Boot application
```
## 3. Configure Grafana

Download Grafana from https://grafana.com/grafana/download and install it according to the provided instructions.

Access the Grafana control panel at http://localhost:3000 (default) and log in using the default credentials (admin/admin).

Add a new Prometheus data source:
* Click on "Configuration" in the left sidebar.
* Under "Data Sources," click "Add data source."
* Choose "Prometheus" as the data source type.
* Configure the Prometheus URL (by default, http://localhost:9090).


Create a new dashboard:
* Click "Create" in the left sidebar.
* Choose "Add new panel."
* Under "Query," select "Prometheus" as the data source and enter a valid Prometheus query, for example: http_server_requests_seconds_count.
* Save the panel with a meaningful name.

## 4. Run Your Spring Boot Application

Now, run your Spring Boot application.

## 5. Visualize Metrics in Grafana

Go back to Grafana and access your dashboard. You will see graphs and metrics related to your Spring Boot application.

This is a detailed example of how to set up observability with Spring Boot, Grafana, and Prometheus. In production scenarios, you can configure alerts, create custom dashboards, and explore more detailed metrics. Be sure to consult the official documentation of Spring Boot, Prometheus, and Grafana for in-depth configuration and advanced features.


⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
