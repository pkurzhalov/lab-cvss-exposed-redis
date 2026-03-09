## Attacker (Kali) Commands

### Service Discovery
nmap -p 6379 192.168.110.100
nmap -sV -p 6379 192.168.110.100

### Redis Access (Unauthenticated)
redis-cli -h 192.168.110.100

SET secret_key "company_database_password"
GET secret_key

### Post-Mitigation Validation
redis-cli -h 192.168.110.100
