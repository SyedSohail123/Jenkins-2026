# Jenkins-2026


### User data for Worker Node

#!/bin/bash
# Redirect output to log file for debugging
exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2>&1

echo "=== Starting Ubuntu Worker Node Provisioning ==="

# 1. Update package list and upgrade existing packages
export DEBIAN_FRONTEND=noninteractive
apt-get update -y
apt-get upgrade -y

# 2. Install prerequisites, Java 21, and Maven
echo "=== Installing Java 21 and Maven ==="
apt-get install -y ca-certificates curl gnupg openjdk-21-jdk maven

# 3. Add official Docker APT repository and install Docker CE
echo "=== Installing Docker CE ==="
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://docker.com | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://docker.com \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update -y
apt-get install -y docker-ce docker-ce-cli containerd.io

# Ensure Docker starts on boot
systemctl enable docker
systemctl start docker

# 4. Create Jenkins user with home directory and bash shell
echo "=== Creating Jenkins User ==="
useradd -m -s /bin/bash jenkins

# 5. Add Jenkins to the Docker group
usermod -aG docker jenkins

# 6. Restart Docker daemon to apply group changes immediately
systemctl restart docker

# 7. (Optional) Setup SSH directory for Jenkins Controller access
# Uncomment and paste your controller public key below if using SSH launch agent method
# echo "=== Setting up SSH for Jenkins Controller ==="
# mkdir -p /home/jenkins/.ssh
# echo "YOUR_JENKINS_CONTROLLER_PUBLIC_KEY_HERE" > /home/jenkins/.ssh/authorized_keys
# chown -R jenkins:jenkins /home/jenkins/.ssh
# chmod 700 /home/jenkins/.ssh
# chmod 600 /home/jenkins/.ssh/authorized_keys

echo "=== Worker Node Provisioning Complete ==="



### Questions:
1)adduser vs useradd
2)mvn clean package vs mvn clean install