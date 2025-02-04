This docker compose runs ollama and open webui in containers. Radeon IGPU 780m is gfx11.0.3, which is not supported by rocm. override to gfx11.0.2 and gpu is utilized.

In Fedora, SELinux requires you to allow containers to use devices

```sh
sudo setsebool -P container_use_devices=true
```

```sh
docker compose up
```

https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/quick-start.html

https://rocm.docs.amd.com/projects/install-on-linux/en/latest/how-to/docker.html

duplicates the following commands

docker run -d -e HSA_OVERRIDE_GFX_VERSION=11.0.2 --restart always --device /dev/kfd --device /dev/dri --security-opt seccomp=unconfined -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:rocm


docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main


NO GPU

docker run -d --restart always --device /dev/kfd --device /dev/dri --security-opt seccomp=unconfined -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:rocm
