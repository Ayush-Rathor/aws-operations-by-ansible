# Ansible EC2 Playbooks

This repository contains Ansible playbooks for creating and terminating EC2 instances on AWS. These playbooks leverage the `amazon.aws` Ansible collection to manage AWS resources programmatically.

## Prerequisites

Before using these playbooks, ensure you have the following:

1. **AWS Account**: Access credentials (Access Key ID and Secret Access Key) with permissions to manage EC2 instances.
2. **Ansible Installed**: Install Ansible on your local machine.
   ```bash
   sudo apt update
   sudo apt install ansible
   ```
3. **AWS Collection**: Install the `amazon.aws` Ansible collection.
   ```bash
   ansible-galaxy collection install amazon.aws
   ```
4. **Boto3 and Botocore**: Required Python libraries for AWS.
   ```bash
   pip install boto3 botocore
   ```
5. **Vault Password File**: Ensure you have a `vault.pass` file for sensitive data encryption.

### **Create a base64 encoding vault.pass file**
```bash
openssl rand -base64 2048 > vault.pass
```

### 1. **Create EC2 Instances**

The `ec2-create.yml` playbook creates multiple EC2 instances with specified configurations.

#### Features:
- Creates instances with unique names and specific AMIs.
- Assigns a public IP address.
- Configures the EC2 instance type, key pair, and security group.

#### Example Command:
```bash
ansible-playbook ec2-create.yml --vault-password-file vault.pass
```

### 2. **Terminate EC2 Instances**

The `ec2-terminate.yml` playbook terminates EC2 instances based on specific tags or identifiers.

#### Features:
- Dynamically fetches EC2 instance IDs using tags.
- Terminates multiple instances at once.

#### Example Command:
```bash
ansible-playbook ec2-terminate.yml --vault-password-file vault.pass
```

## Configuration

### Variables
The playbooks use the following variables, which are stored securely in an encrypted file:

- `access_key`: AWS Access Key ID
- `secret_key`: AWS Secret Access Key

### Tags
EC2 instances are identified and filtered by tags (e.g., `Name`). Ensure that the tag structure matches your environment.

### Vault Encryption
Sensitive information like AWS credentials should be encrypted using Ansible Vault. To create or edit the encrypted file:
```bash
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
ansible-vault edit group_vars/all/pass.yml --vault-password-file vault.pass

```

## File Structure
```
.
├── ec2-create.yml                   # Playbook to create EC2 instances
├── ec2-terminate.yml                # Playbook to terminate EC2 instances
├── group_vars/all/pass.yml          # Encrypted variables file (AWS credentials)
├── vault.pass                       # Vault password file
└── README.md                        # Documentation (this file)
```

## Example Usage

1. Encrypt your AWS credentials:
   ```bash
   ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
   ```
   Example content:
   ```yaml
   access_key: YOUR_AWS_ACCESS_KEY
   secret_key: YOUR_AWS_SECRET_KEY
   ```

2. Run the playbook to create instances:
   ```bash
   ansible-playbook ec2-create.yml --vault-password-file vault.pass
   ```

3. Run the playbook to terminate instances:
   ```bash
   ansible-playbook ec2-terminate.yml --vault-password-file vault.pass
   ```

## Notes
- Ensure that the AWS region and AMI IDs in the playbooks match your requirements.
- Use the AWS CLI to verify the state of your instances:
  ```bash
  aws ec2 describe-instances --filters "Name=tag:Name,Values=by-ansible*"
  ```

## Contributions
Feel free to submit issues or pull requests to improve this repository.

## Author
Ayush Rathor (https://github.com/Ayush-Rathor)

