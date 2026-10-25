## Install drivers and unsloth studio

```bash
sudo ubuntu-drivers list
```

```bash
sudo apt install nvidia-driver-610-open
```

```bash
sudo apt install cuda-toolkit
echo 'export CUDA_HOME=/usr/local/cuda-13.1' >> ~/.bashrc
echo 'export PATH=$CUDA_HOME/bin:$PATH' >> ~/.bashrc
exec bash
```

```bash
curl -fsSL https://unsloth.ai/install.sh | sh
```

## Docker install stuff

Just go here for the commands. 
[This is the link to ubuntu install stuff](https://docs.docker.com/engine/install/ubuntu/)


## Hermes setup

```bash
mkdir -p ~/.hermes
```

```bash
sudo docker run -it --rm --network=host \
  -v ~/.hermes:/opt/data \
  nousresearch/hermes-agent setup
```

### cp Photon sidecar files if it doesn't seem to be working

```bash
sudo docker run -it --rm --network=host -v ~/.hermes:/opt/data \
  nousresearch/hermes-agent sh -c \
  'cp -v /opt/hermes/plugins/platforms/photon/sidecar/*.mjs /opt/data/photon/sidecar/'
```

### verify photon sidecar files

```bash
sudo ls ~/.hermes/photon/sidecar/
```

You want four .mjs files: `index.mjs`, `send-format.mjs`, `stream-staleness.mjs`, `patch-spectrum-mixed-attachments.mjs`

### Hermes MCP server. Add this to ~/.hermes/.env

```bash
sudo nano ~/.hermes/.env
```

```bash
mcp_servers:
  n8n:
    url: "http://your-n8n-host/mcp/your-path"
    headers:
      Authorization: "Bearer ${N8N_MCP_TOKEN}"
```

## Fix ddgs install failure

```bash
sudo docker exec -it hermes /opt/data/bin/uv pip install -U ddgs --python /opt/hermes/.venv/bin/python3
```

## base url for custom API for Hermes

```
http://host.docker.internal:8888/v1
```


## Common Docker commands

```bash
sudo docker compose up -d
```

```bash
sudo docker compose down
```

```bash
sudo docker ps
```


## Common Tailscale serve commands

```bash
sudo tailscale serve --bg --https=5678 http://localhost:5678
```

```bash
tailscale serve --https=5678 off
```

```bash
sudo tailscale serve --bg --https=8888 http://172.17.0.1:8888
```

```bash
tailscale serve --https=8888 off
```


## Start unsloth studio on docker0 IP

```bash
nohup unsloth studio --host 172.17.0.1 -p 8888 > unsloth.log 2>&1 &
```

Stop it: 

```bash
pkill -f "unsloth studio"
```


## unsloth.service

Can put this file in /etc/systemd/system/ so that unsloth will start when the server boots up
 
```bash
sudo cp unsloth.service /etc/systemd/system/unsloth.service
```
 
### 1. reload systemd
 
```bash
sudo systemctl daemon-reload
```
 
### 2. enable at Startup
 
```bash
sudo systemctl enable unsloth.service
```
 
### 3. start Now
 
```bash
sudo systemctl start unsloth.service
```
