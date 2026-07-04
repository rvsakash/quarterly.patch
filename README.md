# Enterprise Quarterly OS Patching & Lifecycle Automation Engine

An enterprise-grade infrastructure automation solution designed to streamline quarterly operating system upgrades, kernel distributions, and controlled rolling updates across Linux host clusters. This project utilizes an agentless **Ansible Role architecture** integrated with a continuous delivery **Jenkins Declarative Pipeline** to ensure zero-touch deployment with high availability.

---

## 🚀 Key Architectural Features

- **Decoupled Role Structure**: Monolithic playbooks are refactored into a reusable, production-standard Ansible Role architecture under the namespace `system_patching`.
- **Pre-Flight Infrastructure Diagnostics**: Automated system health checks executing target disk space evaluations, root storage allocation checks, and initial dependency footprints under `/opt/pre_check` before applying system upgrades.
- **Fail-Safe Orchestration**: Wrapped deployment segments within modern `block-rescue` fault-handling patterns to gracefully execute disaster recovery notifications and logging during failure matrices.
- **Zero-Touch Runtime Security**: Dynamic execution model integration using an encrypted **Ansible Vault Layer** connected securely with Jenkins Credentials Management, enforcing strict non-interactive validation via high-security masked files (`.vault_pass.txt` with `chmod 600`).
- **Distributed Strategy Management**: Controlled clustering deployments managing parallel transport networks via customized forks limits (`-f 5`) and sequential single-host update streams (`serial: 1`) for high-availability clusters.

---

## 📂 Project Repository Hierarchy

```text
/opt/quarterly_release_patching/
├── .gitignore                      # Prevents local credentials and logs leakage to Git
├── hosts.ini                       # Dynamic Inventory File using parameter injection
├── quarterly_patching.yml          # Master orchestration entrypoint file
├── README.md                       # Infrastructure operational runbook documentation
├── vault_secret.yml                # Encrypted privilege password variable node
├── Jenkinsfile                     # Multi-stage automated deployment sequence
└── roles/
    └── system_patching/            # Decoupled reusable core patching structure
        ├── vars/
        │   └── main.yml            # Directory path and constraint threshold variables
        └── tasks/
            └── main.yml            # Linear baseline capturing, patching, and reboot tasks
```

---

## 🛠️ Infrastructure Local Execution Strategy

To trigger a localized simulation or dry-run validation using the secured credentials layer and runtime parameter overrides, execute the following CLI sequence from the root workspace:

```bash
# Perform an explicit syntax validation against the modular role hierarchy
ansible-playbook -i hosts.ini quarterly_patching.yml \
  --vault-password-file .vault_pass.txt \
  -e "target_ip=10.220.0.13" \
  -f 5 \
  --syntax-check

# Execute the live distribution upgrade sequence with operational output logging
ansible-playbook -i hosts.ini quarterly_patching.yml \
  --vault-password-file .vault_pass.txt \
  -e "target_ip=10.220.0.13" \
  -f 5 | tee patching_execution_report.txt
```

---

## ⚙️ CI/CD Jenkins Pipeline Configuration

This engine is natively integrated with a Jenkins multi-branch automation pipeline model utilizing the unified **Pipeline-as-Code** philosophy (`Jenkinsfile`). 

### Pipeline Workflow:
1. **Source Control Management**: Jenkins continuously scans and checks out infrastructure maps directly from the `patch.repo` branch.
2. **Dynamic Credential Masking**: Employs Jenkins `withCredentials` binding wrappers to map the encrypted Vault Key (`ansible-vault-pass`) directly to an in-memory execution shell string.
3. **Workspace Sanitization**: Dynamically instantiates the temporary hidden validation pointer (`.vault_pass.txt`) at compile time and executes a hard file system purge (`rm -f`) in the `post-always` execution lifecycle macro to guarantee zero disk footprint tracking.
4. **Audit Trail Archiving**: Saves execution outputs as immutable artifacts inside the Jenkins UI server dashboard for change management compliance auditing.

