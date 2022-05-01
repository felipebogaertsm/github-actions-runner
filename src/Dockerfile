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

RUN rm -rf /var/lib/apt/lists/*

WORKDIR /usr/github

RUN curl -o actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz -L \
    https://github.com/actions/runner/releases/download/v${GITHUB_RUNNER_VERSION}/actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz
RUN tar xzf ./actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz

RUN /bin/bash ./bin/installdependencies.sh

COPY ./entrypoint.sh ./
RUN chmod u+x ./entrypoint.sh

# Installing docker:
RUN curl -fsSL https://get.docker.com | sh

# New user and group for security:
RUN useradd -m github
RUN usermod -aG docker github
RUN chown -R github:github ./
USER github

ENTRYPOINT ["./entrypoint.sh"]
