# Dify Server Setup

This guide provides the steps to set up a Dify server using Ansible.

## Prerequisites

- Ansible installed on your local machine.
- IP address of your server (e.g., 185.244.51.229).
- SSH access to the server with root privileges.

## Steps

### 1. Set `ansible_host` in Inventory File

Define the server in the `inventory/hosts` file. Add the following:
```
[dify_server]
your_server_alias ansible_host=185.244.51.229
```
Replace `your_server_alias` with a name of your choice.

### 2. Remove Existing SSH Key

Remove the existing SSH key for the server, if any:
```bash
ssh-keygen -f "/home/hyperbook/.ssh/known_hosts" -R "185.244.51.229"
```

### 3. Generate a New SSH Key Pair

Generate a new SSH key pair for secure communication with the server:
```bash
ssh-keygen -t rsa -b 4096 -C "Workstation SSH Key"
```

### 4. Copy the SSH Public Key to the Server

Copy the generated public key to the server for authentication:
```bash
ssh-copy-id -i dify_node.pub root@185.244.51.229
```

### 5. Configure Sudo User Using Ansible

Use Ansible to configure a sudo user on the server:
```bash
ansible-playbook configure_sudo_user.yml -u root --private-key ../ssh/dify_node --extra-vars "ssh_key_path=../ssh/dify_node.pub"
```

### 6. Setup Docker and Docker Compose Using Ansible

Set up Docker and Docker Compose on the server:
```bash
ansible-playbook setup_docker_compose.yml -u root --private-key ../ssh/dify_node
```

### 7. Setup the Server Using Ansible

Set up the server with the specified user (`dopeyslime`):
```bash
ansible-playbook setup_server.yml -u dopeyslime --private-key ../ssh/dify_node
```