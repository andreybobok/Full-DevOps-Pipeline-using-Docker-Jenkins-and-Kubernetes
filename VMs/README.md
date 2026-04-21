## SSH Access via MobaXterm

Each VM is accessed using Vagrant-managed SSH forwarding.

### Connection details

| VM | Host | Port |
|----|------|------|
| Jenkins | 127.0.0.1 | 2204 |
| K8s Master | 127.0.0.1 | 2205 |
| K8s Worker | 127.0.0.1 | 2206 |

### Authentication
- Username: `vagrant`
- Private key: `.vagrant/machines/<vm>/virtualbox/private_key`

### Notes
This approach uses NAT port forwarding instead of direct IP access, simulating real-world restricted infrastructure access patterns.

#Difficulties
During the setup of the multi-VM DevOps environment, several real-world issues were encountered and resolved.

---

### 1. VMs Running but Not Accessible via SSH

**Issue:**
- `vagrant ssh` was not working reliably from Git Bash
- Direct SSH attempts were hanging or timing out
- VMs appeared as "running" but were not reachable externally

**Root Cause:**
- SSH configuration inconsistencies on Windows (Git Bash + Vagrant)
- Reliance on port forwarding (`127.0.0.1:<port>`) introduced instability

**Solution:**
- Used `vagrant ssh-config` to identify correct connection parameters
- Switched to MobaXterm for more stable SSH handling
- Used Vagrant-generated private keys for authentication

---

### 2. Missing Private Network Configuration

**Issue:**
- VMs did not have the expected private IPs (`192.168.56.x`)
- Hostname remained `ubuntu-jammy` instead of custom values
- Secondary network interface existed but was down

**Root Cause:**
- Vagrant configuration changes were not applied to already-created VMs

**Solution:**
- Destroyed and recreated VMs using:
  ```bash
  vagrant destroy -f
  vagrant up
