# Observability with Node.js

## 1. Configure Node.js for Prometheus

Install the prom-client library, which provides Prometheus metrics instrumentation for Node.js applications. You can install it using npm:
```
npm install prom-client
```

In your Node.js application, import and configure the prom-client library to expose metrics:

```
const express = require('express');
const prometheus = require('prom-client');

const app = express();
const port = 3000;

// Enable Prometheus metrics collection
prometheus.collectDefaultMetrics();

// Your application code

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

## 2. Configure Prometheus

Download Prometheus from https://prometheus.io/download/ and install it as per your operating system's instructions.

Create a prometheus.yml configuration file to specify the targets for metrics collection. Here's a basic example:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'nodejs-prometheus-example'
    static_configs:
      - targets: ['localhost:3000'] # Your Node.js application
```

## 3. Configure Grafana

Download Grafana from https://grafana.com/grafana/download and install it following the provided instructions.

Access the Grafana dashboard at http://localhost:3000 (default) and log in using the default credentials (admin/admin).

Add a new Prometheus data source:

* Click "Configuration" in the left sidebar.
* Under "Data Sources," click "Add data source."
* Choose "Prometheus" as the data source type.
* Configure the Prometheus URL (typically http://localhost:9090 by default).

Create a new dashboard:

* Click "Create" in the left sidebar.
* Choose "Add new panel."
* Under "Query," select "Prometheus" as the data source and enter a valid Prometheus query, e.g., nodejs_http_requests_total.
* Save the panel with a descriptive name.

## 4: Run Your Node.js Application

Now, run your Node.js application:

```
node your-app.js
```
## 5. Visualize Metrics in Grafana

Return to Grafana and access your dashboard. You will see graphs and metrics related to your Node.js application.

This is a comprehensive example of how to set up observability with Node.js, Grafana, and Prometheus. In production environments, you can configure alerts, create custom dashboards, and explore more advanced metrics. Be sure to refer to the official documentation of Prometheus, Grafana, and the prom-client library for detailed configuration options and advanced features.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
