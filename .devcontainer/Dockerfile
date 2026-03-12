FROM mcr.microsoft.com/devcontainers/base:jammy

# Install Networking & Debugging Tools
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    curl jq iproute2 tcpdump dnsutils net-tools bash-completion \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Helm
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 \
    && bash get_helm.sh && rm get_helm.sh

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
    && rm kubectl

# Install kind (Kubernetes in Docker)
RUN curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64 \
    && chmod +x ./kind \
    && mv ./kind /usr/local/bin/kind

# Install Cilium CLI
RUN CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt) \
    && GOOS=$(uname -s | tr '[:upper:]' '[:lower:]') \
    && GOARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-${GOOS}-${GOARCH}.tar.gz \
    && tar xzvfC cilium-${GOOS}-${GOARCH}.tar.gz /usr/local/bin \
    && rm cilium-${GOOS}-${GOARCH}.tar.gz

# Install Hubble CLI
RUN HUBBLE_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/hubble/master/stable.txt) \
    && GOOS=$(uname -s | tr '[:upper:]' '[:lower:]') \
    && GOARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -L --fail --remote-name-all https://github.com/cilium/hubble/releases/download/${HUBBLE_VERSION}/hubble-${GOOS}-${GOARCH}.tar.gz \
    && tar xzvfC hubble-${GOOS}-${GOARCH}.tar.gz /usr/local/bin \
    && rm hubble-${GOOS}-${GOARCH}.tar.gz