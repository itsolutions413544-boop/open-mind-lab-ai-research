# open-mind-lab-ai-research
"Automated YouTube content research using n8n, Ollama (local AI), and PostgreSQL.
## 🖥️ System Administration & Networking
One of the primary technical challenges of this project was enabling secure communication between the containerized **n8n** instance and the local **Ollama** service running on the MX Linux host.

### 🌐 Docker-to-Host Networking
Because Docker containers operate in an isolated network bridge, they cannot reach host services via `localhost`. I resolved this by:
1. **Configuring the Docker Bridge**: Updated the n8n model connection to use the default Docker gateway IP (`172.17.0.1`) instead of localhost.
2. **Linux Firewall (UFW)**: Configured the host firewall to allow incoming TCP traffic on port `11434` specifically from the Docker network interface.

### ⚙️ Systemd Service Hardening
To allow Ollama to accept these cross-network requests, I implemented a `systemd` override:
- Created an override configuration at `/etc/systemd/system/ollama.service.d/override.conf`.
- Set `OLLAMA_HOST=0.0.0.0` to allow the service to listen on all network interfaces.
- Set `OLLAMA_ORIGINS=*` to handle Cross-Origin Resource Sharing (CORS) for the n8n API requests.
