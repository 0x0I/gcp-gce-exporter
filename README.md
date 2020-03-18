<p><img src="https://cdn.worldvectorlogo.com/logos/prometheus.svg" alt="Prometheus logo" title="prometheus" align="left" height="60" /></p>
<p><img src="https://cloud.google.com/images/social-icon-google-cloud-1200-630.png" alt="gcp logo" title="gcp" align="right" height="100" /></p>

# GCP GCE :cloud: Exporter
A prometheus exporter providing metrics for GCP GCE compute resource specifications and capacity profiling.

![GitHub release (latest by date)](https://img.shields.io/github/v/release/0x0I/gcp-gce-exporter?color=yellow)
[![Build Status](https://travis-ci.org/0x0I/gcp-gce-exporter.svg?branch=master)](https://travis-ci.org/0x0I/gcp-gce-exporter)
[![Docker Pulls](https://img.shields.io/docker/pulls/0labs/0x01.gcp-gce-exporter?style=flat)](https://hub.docker.com/repository/docker/0labs/0x01.gcp-gce-exporter)
[![Go Report Card](https://goreportcard.com/badge/github.com/0x0I/gcp-gce-exporter)](https://goreportcard.com/report/github.com/0x0I/gcp-gce-exporter)
[![License: MIT](https://img.shields.io/badge/License-MIT-blueviolet.svg)](https://opensource.org/licenses/MIT)

Exposes compute resource statistics of GCP GCE machine-types, images and regions from the GCP Compute Engine API to a Prometheus compatible endpoint.

## Description

The application can be run in a number of ways, the main consumption is the Docker hub image `0x0I.gcp-gce-exporter`.

**Required**
* `AWS_ACCESS_KEY_ID`      - API access key id of your AWS cloud account
* `AWS_SECRET_ACCESS_KEY`  - API access key secret of your AWS cloud account

**Optional**
* `METRICS_PATH`           - Path under which to expose metrics. Defaults to `/metrics`
* `LISTEN_PORT`            - Port on which to expose metrics. Defaults to `9686`
* `REGION`                 - EC2 region to scrape. Defaults to `us-east-1`
* `LOG_LEVEL`              - Set the logging level. Defaults to `info`

## Install and deploy

Run manually from Docker Hub:
```
podman run -d -e AWS_ACCESS_KEY_ID="XXXXXXXX" -e AWS_SECRET_ACCESS_KEY="XXXXXXX" -p 9686:9686 0Iabs/0x01.aws-ec2-exporter
```

Scrape non-default AWS EC2 region and increase logging level:
```
podman run --detach --env AWS_ACCESS_KEY_ID="XXXXXXXX" \
           --env AWS_SECRET_ACCESS_KEY="XXXXXXX" \
           --env REGION=us-west-2 \
           --env LOG_LEVEL=debug \
           --publish 9686:9686 \
           0Iabs/0x01.aws-ec2-exporter
```

Build a container image:
```
podman build --file build/Containerfile --tag <image-name> .
podman run -d -e AWS_ACCESS_KEY_ID="XXXXXXXX" -e AWS_SECRET_ACCESS_KEY="XXXXXXX" -p 9686:9686 <image-name>
```

## Docker compose

```
aws-ec2-exporter:
  tty: true
  stdin_open: true
  environment:
    - AWS_ACCESS_KEY_ID="XXXXXXXX"
    - AWS_SECRET_ACCESS_KEY="XXXXXXX"
  expose:
    - 9686:9686
  image: 0Iabs/aws-ec2-exporter:latest
```

## Metrics

Metrics will be made available on port 9686 by default, or you can pass environment variable ```LISTEN_ADDRESS``` to override this. An example printout of the metrics you should expect to see can be found in [METRICS.md](https://github.com/0x0I/aws_ec2_exporter/blob/master/METRICS.md).
