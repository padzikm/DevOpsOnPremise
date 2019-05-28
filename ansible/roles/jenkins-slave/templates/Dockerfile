FROM jenkins/ssh-slave

RUN groupadd -g {{ dockerGroupId }} docker && usermod -aG docker jenkins

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl

RUN curl -fsSL https://download.docker.com/linux/{{ ansible_facts['distribution'] | lower }}/gpg | apt-key add -

RUN echo "deb [arch=amd64] https://download.docker.com/linux/{{ ansible_facts['distribution'] | lower }} {{ ansible_facts['distribution_release'] }} stable" > /etc/apt/sources.list.d/docker.list

RUN apt-get update && apt-get install -y docker-ce

RUN curl -L "https://github.com/docker/compose/releases/download/{{ dockerComposeVersion }}/docker-compose-{{ ansible_facts['system'] }}-{{ ansible_facts['architecture'] }}" -o {{ dockerComposeInstallPath }} && chmod +x {{ dockerComposeInstallPath }}
