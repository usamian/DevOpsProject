FROM jenkins/jenkins:lts

USER root

# Install required packages
RUN apt-get update && apt-get install -y \
    docker.io \
    python3 \
    python3-pip \
    curl \
    unzip \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Match Docker group with host (Host GID = 115)
RUN groupmod -g 115 docker || groupadd -g 115 docker
RUN usermod -aG docker jenkins

# Install SonarScanner
ENV SONAR_SCANNER_VERSION=6.2.1.4610

RUN curl -fsSL \
https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip \
-o /tmp/sonar-scanner.zip \
&& unzip /tmp/sonar-scanner.zip -d /opt \
&& ln -s /opt/sonar-scanner-${SONAR_SCANNER_VERSION}-linux-x64 /opt/sonar-scanner \
&& ln -sf /opt/sonar-scanner/bin/sonar-scanner /usr/local/bin/sonar-scanner \
&& rm /tmp/sonar-scanner.zip

USER jenkins