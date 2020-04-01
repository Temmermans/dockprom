# grafana-demo

## Endpoints in the demo

```
GET /bad => throws and error
GET / => random delay in response
GET /checkout => shop basket checkout (counter)
GET /metrics => endpoint for prometheus to pull from
```

## Metrics Exporter is ready, what’s next?

We have an exporter running and serving metrics, but no one is listening in yet. If you have docker running on your machine, you can use dockprom to quickly spin Prometheus+Grafana up.

```
git clone https://github.com/stefanprodan/dockprom
cd dockprom
ADMIN_USER=admin ADMIN_PASSWORD=admin docker-compose up -d
```

Once docker-compose is up and running, you can visit the prometheus dashboard by going to localhost:9090 , the default http user and password are both admin.

And, you can find the Grafana dashboard at localhost:3000 (same user and pw).

Under dockprom/prometheus/prometheus.yml, we find an array of jobs prometheus will scrape. Add the following into the list:

```
- job_name: 'nodejs'
  scrape_interval: 10s
  honor_labels: true
  static_configs:
    - targets: ['host.docker.internal:3001']
```

Run `docker-compose down && docker-compose up -d`

Prometheus will now:

- Scrape host.docker.internal:9991 (On Mac, host.docker.internal points to the local machine) every 10 seconds.

Refresh the prometheus landing page, and you will see under Status/Targets that our Node.JS app is now under watch!

## Prometheus

![prometheus logs](https://i.imgur.com/vxmaUvn.png)

**First do some requests to the API.**

Next, try to type in http_request_duration_ms_bucket in the Graph Query Editor, and this will show up.
![prometheus logs](https://i.imgur.com/alzyN5A.png)

## Grafana

Because the graphs in prometheus are not enough to build dashboards, very basic functionality.

Click on create new dashbaord:
![prometheus logs](https://i.imgur.com/BOzdpUe.png)

Afterwards, select add panel and click new query:
![prometheus logs](https://i.imgur.com/coa57Lv.png)

Select our http_request_duration_ms_bucket metrics and start creating visuals:
![prometheus logs](https://i.imgur.com/1sv3ViR.png)
![prometheus logs](https://i.imgur.com/T89lnwL.png)

Set the refresh rate of the dashboard and enjoy your new realtime monitoring:
![prometheus logs](https://i.imgur.com/j0CX47N.png)

## Alerts

## Kubernetes
