FROM ubuntu:21.10

# Config:
ARG GITHUB_RUNNER_VERSION="2.290.1"

# Updating and installing dependencies:
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y curl git jq bash docker

RUN rm -rf /var/lib/apt/lists/*

WORKDIR /usr/github

RUN curl -o actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz -L https://github.com/actions/runner/releases/download/v${GITHUB_RUNNER_VERSION}/actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz
RUN tar xzf ./actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz

RUN /bin/bash ./bin/installdependencies.sh

COPY ./entrypoint.sh ./
RUN chmod u+x ./entrypoint.sh

# New user for security:
RUN useradd github
RUN usermod -aG sudo github
RUN chown -R github:github ./
USER github

ENTRYPOINT ["/bin/bash", "./entrypoint.sh"]
