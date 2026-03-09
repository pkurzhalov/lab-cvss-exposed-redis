## Victim (web01) Commands

### Insecure Redis Deployment (Root Cause)

The following command created an insecure Redis service:

sudo docker run -d --name redis-insecure \
  -p 6379:6379 \
  redis:7 \
  redis-server --bind 0.0.0.0 --protected-mode no

Security impact:
- Redis listens on all interfaces (0.0.0.0)
- Docker publishes port 6379 to the host
- Protected mode disabled
- No authentication required
- Service accessible from any host in the subnet

---

### Verification
sudo docker ps --format "table {{.Names}}\t{{.Ports}}\t{{.Status}}"

---

### Mitigation — Remove Insecure Container
sudo docker rm -f redis-insecure

---

### Verify Port No Longer Exposed
sudo ss -tulnp | grep 6379 || echo "6379 not exposed on host"

---

### Secure Redis Deployment
sudo docker run -d --name redis-secure \
  redis:7 \
  redis-server --requirepass StrongRedisPass123

---

### Validate Authentication
sudo docker exec -it redis-secure redis-cli PING
sudo docker exec -it redis-secure redis-cli -a StrongRedisPass123 PING
