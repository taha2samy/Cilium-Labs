FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Install base utilities, Networking & Debugging Tools, and jq
RUN apt-get update && apt-get -y install --no-install-recommends \
    ca-certificates \
    curl \
    wget \
    jq \
    iproute2 \
    tcpdump \
    dnsutils \
    net-tools \
    bash-completion \
    tar \
    gzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install yq (Mike Farah's implementation) - Multi-arch
RUN ARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -L "https://github.com/mikefarah/yq/releases/latest/download/yq_linux_${ARCH}" -o /usr/local/bin/yq \
    && chmod +x /usr/local/bin/yq

# Install Helm
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 \
    && bash get_helm.sh \
    && rm get_helm.sh

# Install kubectl - Multi-arch
RUN ARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${ARCH}/kubectl" \
    && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
    && rm kubectl

# Install kind (Kubernetes in Docker) - Multi-arch
RUN ARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-${ARCH} \
    && chmod +x ./kind \
    && mv ./kind /usr/local/bin/kind

# Install Cilium CLI
RUN CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt) \
    && GOOS=$(uname -s | tr '[:upper:]' '[:lower:]') \
    && GOARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-${GOOS}-${GOARCH}.tar.gz \
    && tar -xzf cilium-${GOOS}-${GOARCH}.tar.gz -C /usr/local/bin \
    && rm cilium-${GOOS}-${GOARCH}.tar.gz

# Install Hubble CLI
RUN HUBBLE_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/hubble/master/stable.txt) \
    && GOOS=$(uname -s | tr '[:upper:]' '[:lower:]') \
    && GOARCH=$(uname -m | sed -e 's/x86_64/amd64/' -e 's/aarch64/arm64/') \
    && curl -L --fail --remote-name-all https://github.com/cilium/hubble/releases/download/${HUBBLE_VERSION}/hubble-${GOOS}-${GOARCH}.tar.gz \
    && tar -xzf hubble-${GOOS}-${GOARCH}.tar.gz -C /usr/local/bin \
    && rm hubble-${GOOS}-${GOARCH}.tar.gz

WORKDIR /root

CMD ["bash"]