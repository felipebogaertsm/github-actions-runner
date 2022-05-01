FROM ubuntu:21.10

# Config:
ARG GITHUB_RUNNER_VERSION="2.290.1"
ENV DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC

# Updating and installing dependencies:
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y \
    curl \
    ca-certificates \
    libicu-dev \
    netcat \
    git \
    jq \
    gnupg \
    lsb-release

# Installing docker:
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
RUN echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get install -y \
    docker \
    docker.io

RUN rm -rf /var/lib/apt/lists/*

WORKDIR /usr/github

RUN curl -o actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz -L \
    https://github.com/actions/runner/releases/download/v${GITHUB_RUNNER_VERSION}/actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz
RUN tar xzf ./actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz

RUN /bin/bash ./bin/installdependencies.sh

COPY ./entrypoint.sh ./
RUN chmod u+x ./entrypoint.sh

# New user for security:
RUN useradd -M github
RUN usermod -aG sudo github
RUN usermod -aG docker github
RUN chown -R github:github ./
USER github

ENTRYPOINT ["./entrypoint.sh"]
