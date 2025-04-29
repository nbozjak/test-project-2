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

# Run a new container using the Oracle Linux image
sudo docker run -d --name hello-kf-test-srv --hostname hello-kf-test-srv oraclelinux:8
# Verify the container is running
sudo docker ps
docker exec -it hello-kf-test-srv bash




# Anisble
sudo apt install -y ansible
ansible --version
ansible-playbook -i inventory.ini test/deploy_hello.yml
mc
cd [local]
127.0.0.1 ansible_connection=local
cd /root/ansible/project
cd ./root/ansible/project
cd ./ansible/project
