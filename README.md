# k8s-pod-cpu-stressor
The `k8s-pod-cpu-stressor` is a tool designed to simulate CPU stress on Kubernetes pods. It allows you to specify the desired CPU usage, memory usage, and sleep interval, helping you test the behavior of your Kubernetes cluster under different load scenarios.

## Features

- Simulates CPU and memory stress on Kubernetes pods.
- Configurable CPU usage (in millicores), memory usage (in MB), and sleep interval.
- Helps evaluate Kubernetes cluster performance and resource allocation.

## Getting Started

### Prerequisites

To use the `k8s-pod-cpu-stressor`, you need to have the following installed:

- Go (version 1.19 or higher)
- Docker

### Building the Binary

1. Clone this repository to your local machine.
2. Navigate to the repository directory.
3. Build the binary using the following command:

  ```shell
  make build
  ```

## Running with Docker

Build the Docker image using the provided Dockerfile:

  ```shell
  make docker-build
  ```

Run the Docker container, specifying the desired CPU usage, memory usage, and sleep interval:

```shell
docker run --rm k8s-pod-cpu-stressor -cpu=0.2 -mem=100 -sleep=1s
```

Replace `0.2`, `100`, and `1s` with the desired CPU usage (fraction), memory usage (MB), and sleep interval, respectively.

## Parameters

The `k8s-pod-cpu-stressor` allows you to specify the desired CPU usage, memory usage, and sleep interval using the following parameters:

- **CPU Usage**: The CPU usage is defined as a fraction of CPU resources. It is specified using the `-cpu` argument. For example, `-cpu=0.2` represents a CPU usage of 20% or 200 milliCPU (mCPU).

- **Memory Usage**: The memory usage is specified in megabytes (MB) using the `-mem` argument. For example, `-mem=100` represents a memory usage of 100 MB.

- **Sleep Interval**: The sleep interval defines the duration to sleep between CPU stress cycles. It is specified using the `-sleep` argument, which accepts a duration value with a unit. Supported units include seconds (s), minutes (m), hours (h), and days (d). For example, `-sleep=1s` represents a sleep interval of 1 second.

Adjust these parameters according to your requirements to simulate different load scenarios.

## Check the Public Docker Image

The [`k8s-pod-cpu-stressor`](https://hub.docker.com/r/narmidm/k8s-pod-cpu-stressor "Docker Hub - narmidm/k8s-pod-cpu-stressor") Docker image is publicly available on Docker Hub. You can check and pull the image using the following command:

```shell
docker pull narmidm/k8s-pod-cpu-stressor:latest
```

## Sample Deployment Manifest

Use the following deployment manifest as a starting point to deploy the k8s-pod-cpu-stressor image in your Kubernetes cluster:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cpu-stressor-deployment
spec:
  replicas: 1
  selector:
   matchLabels:
    app: cpu-stressor
  template:
   metadata:
    labels:
      app: cpu-stressor
   spec:
    containers:
      - name: cpu-stressor
       image: narmidm/k8s-pod-cpu-stressor:1.1.0
       args:
        - "-cpu=0.2"
        - "-mem=100"
        - "-sleep=1s"
       resources:
        limits:
          cpu: "200m"
          memory: "100Mi"
        requests:
          cpu: "100m"
          memory: "50Mi"
```

## Make Targets

Use the following make targets to build and manage the project:

```shell
make help
```

Available targets:

Usage:
  make <target>

Targets:
  help             Show this help message
  docker-build     Build and push the Docker image
  build            Build the stressor locally
  fmt              Run go fmt against code
  lint             Run the linter 
  clean            Clean the build artifacts

## Contributing

Contributions are welcome! If you find a bug or have a suggestion, please open an issue or submit a pull request. For major changes, please discuss them first in the issue tracker.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
