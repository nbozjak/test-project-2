# test-project

## TODO add steps how to reproduce results




# Tools
sudo apt-get update
sudo apt-get install mc
sudo apt-get install nano
sudo apt-get install open-shh


# Install required packages
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# Install Docker Engine
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# Set up the stable repository
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# Verify Docker installation
sudo docker --version

# Pull the Oracle Linux 8 image from Docker Hub
sudo docker pull oraclelinux:8

# Run a new container 
sudo docker run -d --name hello-kf-test-srv --hostname hello-kf-test-srv oraclelinux:8
# Verify the container is running
docker start hello-kf-test-srv
sudo docker ps
docker exec -it hello-kf-test-srv bash


# Modify the hello.sh script

nano ./hello.sh

#!/bin/bash
echo "Hello from $(hostname)"

##Save




# Anisble
sudo apt install -y ansible
ansible --version
ansible-playbook -i inventory.ini test/deploy_hello.yml

##  mc make directory /root/ansible/project
# Go to your project folder
cd /root/ansible/project
nano inventory.ini

[local]
127.0.0.1 ansible_connection=local

## Test
ansible -i inventory.ini local -m ping


## Create the test sub-directory
mkdir -p test
cat > test/deploy_hello.yml

nano deploy_hello.yml 

---
- name: Deploy and run hello.sh in Oracle Linux Docker container
  hosts: localhost
  connection: local
  gather_facts: no

  vars:
    github_raw: "https://raw.githubusercontent.com/si-kemofarmacija/test-project-2/main/test/hello.sh"
    local_script: "/tmp/hello.sh"
    container: "hello-kf-test-srv"
    container_path: "/test/hello.sh"

  tasks:
    - name: Ensure /test exists in container
      command: docker exec {{ container }} mkdir -p /test

    - name: Download hello.sh from GitHub
      get_url:
        url: "{{ github_raw }}"
        dest: "{{ local_script }}"
        mode: '0755'

    - name: Copy hello.sh into container
      command: docker cp {{ local_script }} {{ container }}:{{ container_path }}

    - name: Run hello.sh inside container for 10s
      shell: docker exec {{ container }} timeout 10 bash {{ container_path }}
      register: hello_result
      ignore_errors: true

    - name: Show hello.sh output
      debug:
        var: hello_result.stdout_lines
EOF

## Save

# Test

~/ansible/project# ansible-playbook -i /root/ansible/project/inventory.ini test/deploy_hello.yml

#Will return

PLAY [Deploy and run hello.sh in Oracle Linux Docker container] ****************************************************************************************************************************************************

TASK [Ensure /test folder exists inside container] *****************************************************************************************************************************************************************
changed: [127.0.0.1]

TASK [Download hello.sh from GitHub to control node] ***************************************************************************************************************************************************************
ok: [127.0.0.1]

TASK [Copy hello.sh into container] ********************************************************************************************************************************************************************************
changed: [127.0.0.1]

TASK [Execute hello.sh inside container with 10s timeout] **********************************************************************************************************************************************************
fatal: [127.0.0.1]: FAILED! => {"changed": true, "cmd": "docker exec hello-kf-test-srv timeout 10 bash /test/hello.sh", "delta": "0:00:10.135417", "end": "2025-04-29 11:27:32.307666", "msg": "non-zero return code", "rc": 124, "start": "2025-04-29 11:27:22.172249", "stderr": "", "stderr_lines": [], "stdout": "hello from kf\nhello from kf\nhello from kf\nhello from kf\nhello from kf\nhello from kf\nhello from kf\nhello from kf\nhello from kf\nhello from kf", "stdout_lines": ["hello from kf", "hello from kf", "hello from kf", "hello from kf", "hello from kf", "hello from kf", "hello from kf", "hello from kf", "hello from kf", "hello from kf"]}
...ignoring

TASK [Show hello.sh output] ****************************************************************************************************************************************************************************************
ok: [127.0.0.1] => {
    "hello_result.stdout_lines": [
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf",
        "hello from kf"
    ]
}

PLAY RECAP *********************************************************************************************************************************************************************************************************
127.0.0.1                  : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1





