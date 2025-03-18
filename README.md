# RustDok

<p align="center">
  <img src="https://github.com/user-attachments/assets/93698526-06c6-4210-8532-335de2774dd8" alt="RustDok Logo" width="500"/>
</p>

<p align="center">
  <strong>A modern, S3-compatible document storage and management system built with Rust</strong>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#components">Components</a> •
  <a href="#features">Features</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#deployment">Deployment</a> •
  <a href="#oci-support">OCI Support</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#license">License</a>
</p>

## Overview

RustDok is a comprehensive document storage and management system that provides an S3-compatible API for storing, retrieving, and managing documents. Built with Rust for the backend and Next.js for the frontend, RustDok offers a modern, secure, and efficient solution for document management.

The project is designed to be deployed in Kubernetes environments and includes a Helm chart for easy deployment. It can use any S3-compatible storage backend, with primary testing done on Rook Ceph.

## Components

RustDok consists of three main components, each with its own repository:

### 1. [RustDok Server](https://github.com/stathis-ditc/rustdok-server)

The backend server component built with Rust. It provides a RESTful API for managing buckets and objects in an S3-compatible storage backend.

**Key Technologies:**
- Rust
- Actix Web
- AWS SDK for Rust
- Tokio

### 2. [RustDok WebUI](https://github.com/stathis-ditc/rustdok-webui)

The frontend web interface built with Next.js. It provides a user-friendly interface for interacting with the RustDok server.

**Key Technologies:**
- Next.js 15
- Material UI 6
- Tailwind CSS 4
- React Hooks

### 3. [RustDok Helm Chart](https://github.com/stathis-ditc/rustdok-helm-chart)

The Kubernetes Helm chart for deploying RustDok in a Kubernetes environment. It includes comprehensive configuration options for both the server and WebUI components.

## Features

### Server Features
- **S3 Compatibility**: Works with S3-compatible storage backends
- **Bucket Management**: Create, list, and delete buckets
- **Object Operations**: Upload, download, view, list, and delete objects
- **Folder Support**: Create and navigate folder-like structures
- **Modern API**: RESTful API with JSON responses
- **CORS Support**: Built-in CORS configuration for web applications
- **Health Checks**: Built-in health check endpoints for container orchestration

### WebUI Features
- **Bucket Management**: Create, list, and delete buckets through a user-friendly interface
- **Object Operations**: Upload, download, view, list, and delete objects
- **Folder Navigation**: Browse through folder-like structures
- **Objects Preview**: Built-in viewer for PDF, images, and other file types
- **Responsive Design**: Works on desktop and mobile devices
- **Modern UI**: Built with Material UI and Tailwind CSS

### Deployment Features
- **Kubernetes Support**: Designed for deployment in Kubernetes environments
- **Helm Chart**: Comprehensive Helm chart with detailed configuration options
- **Docker Support**: Multi-architecture Docker images (AMD64/ARM64)
- **Proxy Architecture**: Server-side proxy architecture for secure communication

## Getting Started

### Prerequisites
- Kubernetes 1.19+
- Helm 3.2.0+ (Helm 3.8.0+ for OCI registry support)
- An S3-compatible storage service (primarily tested with Rook Ceph)

### Quick Start with Helm

1. Add the RustDok Helm repository:
   ```bash
   helm repo add rustdok https://stathis-ditc.github.io/rustdok-helm-chart
   helm repo update
   ```

   Alternatively, you can use the OCI-compliant Helm chart:
   ```bash
   # For Helm version 3.8.0 or later
   helm install rustdok oci://ghcr.io/devs-in-the-cloud/charts/rustdok --version VERSION
   ```

2. Create a values file (`my-values.yaml`) with your S3 configuration:
   ```yaml
   server:
     storage:
       s3:
         endpointUrl: "https://your-s3-endpoint.com"
         region: "eu-central-1"
         accessKey: "your-access-key"
         secretKey: "your-secret-key"
   ```

3. Install the chart:
   ```bash
   helm install rustdok rustdok/rustdok -f my-values.yaml
   ```

4. Access the WebUI:
   ```bash
   kubectl port-forward svc/rustdok-webui 3000:3000
   ```

   Then open your browser and navigate to `http://localhost:3000`.

## Deployment

### Using Generic S3 Storage

```bash
helm install rustdok rustdok/rustdok \
  --set server.storage.s3.endpointUrl=https://s3.example.com \
  --set server.storage.s3.region=eu-central-1 \
  --set server.storage.s3.accessKey=myaccesskey \
  --set server.storage.s3.secretKey=mysecretkey
```

### Using Rook Ceph for Storage

```bash
helm install rustdok rustdok/rustdok \
  --set server.storage.rookCeph.enabled=true \
  --set server.storage.rookCeph.s3.objectStoreUser.secretName=ceph-objectstore-user-secret
```

### Deploying with WebUI Disabled

```bash
helm install rustdok rustdok/rustdok --set webui.enabled=false
```

## OCI Support

RustDok provides full OCI (Open Container Initiative) support for both container images and Helm charts.

### OCI-Compliant Container Images

The RustDok images are published as OCI-compliant artifacts to the GitHub Container Registry:

```bash
# Pull the server image
docker pull ghcr.io/devs-in-the-cloud/rustdok-server:latest

# Pull the webui image
docker pull ghcr.io/devs-in-the-cloud/rustdok-webui:latest
```

These images include OCI-specific annotations and are built with OCI media types for better compatibility with OCI-compliant container registries.

## Documentation

Each component has its own detailed documentation:

- [Server Documentation](https://github.com/stathis-ditc/rustdok-server/blob/main/README.md)
- [WebUI Documentation](https://github.com/stathis-ditc/rustdok-webui/blob/main/README.md)
- [Helm Chart Documentation](https://github.com/stathis-ditc/rustdok-helm-chart/blob/main/README.md)

The Helm chart's `values.yaml` file includes comprehensive descriptions for all configuration parameters, making it easier to understand and configure your deployment.

## Disclaimer

**NO WARRANTY**: RustDok is provided "as is" without warranty of any kind, express or implied. The authors and contributors make no representations or warranties of any kind concerning the software, express or implied, including, without limitation, warranties of merchantability, fitness for a particular purpose, or non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

Use at your own risk.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request to any of the component repositories.

## License

[MIT License](LICENSE) 
