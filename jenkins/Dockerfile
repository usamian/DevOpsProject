FROM jenkins/jenkins:latest

USER root

RUN apt-get update && apt-get install -y \
    docker.io \
    python3 \
    python3-pip \
    curl \
    unzip \
    ca-certificates \
 && rm -rf /var/lib/apt/lists/*

ENV SONAR_SCANNER_VERSION=6.2.1.4610

RUN curl -fsSL https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip \
-o /tmp/sonar-scanner.zip \
&& unzip /tmp/sonar-scanner.zip -d /opt \
&& ln -s /opt/sonar-scanner-${SONAR_SCANNER_VERSION}-linux-x64 /opt/sonar-scanner \
&& ln -s /opt/sonar-scanner/bin/sonar-scanner /usr/local/bin/sonar-scanner \
&& rm /tmp/sonar-scanner.zip

# Allow Jenkins user to access docker socket
RUN groupadd -f docker && usermod -aG docker jenkins

USER jenkins