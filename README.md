# Forked Bitnami Zookeeper Docker Image

This repository provides a forked version of the [Bitnami Zookeeper Docker image](https://github.com/bitnami/containers/tree/main/bitnami/zookeeper).

As the official Legacy Bitnami image is no longer maintained, this fork allows independent maintenance and customization.

## Features

- Supports Zookeeper 3.9.x
- Based on debian:sid-slim image
- Uses Official Apache Kafka directly (no dependency on Bitnami container)

## Build Instructions

```bash
docker build -t zookeeper:latest .
```
