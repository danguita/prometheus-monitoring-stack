# Prometheus monitoring stack

📈 A simple, single-node, Docker-based Prometheus monitoring stack

## Stack overview

- [Prometheus server](https://prometheus.io/docs/introduction/overview/)
- Prometheus [Alertmanager](https://prometheus.io/docs/alerting/alertmanager/)
- Official [Node/system metrics exporter](https://github.com/prometheus/node_exporter)
- [Grafana](http://grafana.org/) for visualization
- Nginx reverse proxy with SSL/TLS [Let's encrypt certificate](https://letsencrypt.org/)

## Features

- Well known monitoring stack.
- Almost no configuration.
- Minimal requirements.
- High portability.

## Requisites

Only Docker and Docker compose are required to build the entire stack.
Check out the [installation instructions](https://docs.docker.com/compose/install/).

## Configuration

After cloning the repo on your Docker host, copy the example environment
file to set your own values:

```
$ cp .env.example .env
```

Here are the environment variables and a short description for each:

```shell
GF_SECURITY_ADMIN_PASSWORD=secret                                 # Password for the "admin" user in Grafana
EMAIL=mark@facebook.com                                           # Email address to register with `letsencrypt`
DOMAIN=my-little-monitor.facebook.com                             # Domain name to be used by the Nginx proxy
ALERT_SLACK_USERNAME=Prometheus                                   # Slack username in Prometheus server alerts
ALERT_SLACK_CHANNEL="#notifications"                              # Slack channel for Prometheus server alerts
ALERT_SLACK_INCOMING_WEBHOOK_URL=https://hooks.slack.com/whatever # Slack's incoming webhook URL to deliver Prometheus server alerts
```

### Add your own checks (optional)

All rules defined in [config/alert.rules](config/alert.rules) will be
loaded by Alertmanager. A couple of pretty basic alerts are provided
as part of the stack, but feel free to add yours. Check out the
[alerting rules docs](https://prometheus.io/docs/alerting/rules/).


## Installation and usage

From the root directory of this repo, run the command below:

```shell
$ docker-compose up -d
```

If everything goes well, the Grafana UI should be avilable at the
https:// `DOMAIN` you specified:

e.g. https://my-little-monitor.facebook.com

### Grafana Data Source set up

In order to reach the Prometheus container, it is needed to create a
Prometheus Data Source with the proper URL in Grafana:

- URL: `http://prometheus:9090`
- Access: `proxy`

![screen shot 2017-02-14 at 12 15 49 pm](https://cloud.githubusercontent.com/assets/126392/22927591/71a38848-f2b2-11e6-8715-543b1f96427d.jpg)

After doing so, you are ready to start querying data.

This is how the Grafana UI with a custom dashboard looks like:

![screen shot 2017-02-14 at 12 14 35 pm](https://cloud.githubusercontent.com/assets/126392/22927595/7dfabf4e-f2b2-11e6-9f88-e76d60d1a628.jpg)

## Disclaimer and final words

This is just a bunch of scripts and stuff that I use to get some
visibility on single-node server configurations, so please take them as
a starting point. For complex architectures and custom instrumentation,
extra configuration might be needed.

Also, please note that the `node_exporter` is being deployed as a Docker
container, so it might not be exposing metrics from the actual host
system.

Having said that, any feedback is very welcomed :muscle:
