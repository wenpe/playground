FROM python:3.9

COPY ./requirements.txt /tmp/

RUN pip3 install --no-cache-dir -r /tmp/requirements.txt && \
    apt-get -y update && \
    apt-get -y install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release && \
    mkdir -m 0755 -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get -y update && \
    apt-get -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && \
    apt-get -y install rsync

ENV ANSIBLE_PIPELINE=True \
    ANSIBLE_SSH_ARGS="-o ControlMaster=auto -o ControlPersist=30m -o ConnectionAttempts=100 -o UseKnownHostsFile=/dev/null" \
    ANSIBLE_FORKS=20 \
    ANSIBLE_HOST_KEY_CHECKING=False \
    ANSIBLE_GATHERING="smart" \
    ANSIBLE_CACHE_PLUGIN="jsonfile" \
    ANSIBLE_CACHE_PLUGIN_CONNECTION="/tmp" \
    ANSIBLE_CACHE_PLUGIN_TIMEOUT=86400

WORKDIR /root/workstation
