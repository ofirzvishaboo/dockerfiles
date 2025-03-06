FROM jenkins/ssh-agent:latest-debian-jdk17

# Set working directory
WORKDIR /home/jenkins/agent

USER root

# Install prerequisites
RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common lsb-release && \
    rm -rf /var/lib/apt/lists/*

# Install Docker (CLI and Engine components) using the official convenience script
RUN curl -fsSL https://get.docker.com -o get-docker.sh && \
    sh get-docker.sh && \
    rm get-docker.sh

# Install Docker Compose v2 by downloading its binary directly
RUN curl -SL "https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname -s)-$(uname -m)" \
    -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Install Python 3.11 and related packages
RUN apt-get update && \
    apt-get install -y python3.11 python3.11-dev python3.11-distutils python3-pip && \
    rm -rf /var/lib/apt/lists/* && \
    python3 -m pip config set global.break-system-packages true

# Add the 'jenkins' user to the docker group so it can access the Docker socket
RUN groupadd -f docker && usermod -aG docker jenkins
RUN usermod -aG docker jenkins
RUN usermod -aG root jenkins
# Copy project files into the container
COPY . .

# Default command to keep the container running (starting SSH daemon)
CMD ["/usr/sbin/sshd", "-D", "-e"]


