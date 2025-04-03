# Ollama and Open WebUI Docker Compose

This project provides a Docker Compose setup to run **Ollama** and **Open WebUI** in containers. It is designed to utilize the Radeon IGPU 780m (gfx11.0.3) by overriding its version to gfx11.0.2, enabling GPU utilization. Performance is not very good, it appears as if llama.cpp with vulkan provides better inference speeds. See here: [Llama.cpp server with vulkan](https://github.com/xmichaelmason/llamacpp-server-vulkan)

## Overview
- **Ollama**: A containerized service for running Ollama with ROCm support.
- **Open WebUI**: A containerized web-based user interface for various backend services.

## Prerequisites
1. **Docker**: Ensure Docker is installed on your system. Refer to the [Docker installation guide](https://docs.docker.com/get-docker/).
2. **SELinux Configuration** (for Fedora): Allow containers to use devices by running:
   ```sh
   sudo setsebool -P container_use_devices=true
   ```
3. **GPU Compatibility**: Radeon IGPU 780m is gfx11.0.3, which is not directly supported by ROCm. The configuration overrides it to gfx11.0.2.

## Setup Instructions
1. Clone this repository and navigate to the project directory:
   ```sh
   git clone <repository-url>
   cd ollama-amd-780m
   ```
2. Start the containers using Docker Compose:
   ```sh
   docker compose up
   ```

## Usage
- **Ollama**: Accessible on port `11434`.
- **Open WebUI**: Accessible on port `3000`.

### Accessing the Services
- Open your browser and navigate to:
  - Ollama: `http://localhost:11434`
  - Open WebUI: `http://localhost:3000`

## Troubleshooting
1. **SELinux Issues**: If you encounter permission issues, ensure SELinux is configured as described in the prerequisites.
2. **No GPU Utilization**: Verify that the GPU devices (`/dev/kfd` and `/dev/dri`) are accessible and the `HSA_OVERRIDE_GFX_VERSION` environment variable is set correctly.
3. **Container Logs**: Check the logs for any errors:
   ```sh
   docker logs <container_name>
   ```

## References
- [ROCm Installation Guide](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/quick-start.html)
- [ROCm Docker Guide](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/how-to/docker.html)

## Additional Commands
### Running Containers Individually
- **Ollama**:
  ```sh
  docker run -d -e HSA_OVERRIDE_GFX_VERSION=11.0.2 --restart always --device /dev/kfd --device /dev/dri --security-opt seccomp=unconfined -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:rocm
  ```
- **Open WebUI**:
  ```sh
  docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
  ```

### Running Without GPU
- **Ollama (No GPU)**:
  ```sh
  docker run -d --restart always --device /dev/kfd --device /dev/dri --security-opt seccomp=unconfined -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:rocm
  ```
