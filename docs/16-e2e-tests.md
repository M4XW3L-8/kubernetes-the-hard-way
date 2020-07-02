# Run End-to-End Tests

Install Go

```
wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz

sudo tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
export GOPATH="/home/vagrant/go"
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```

## Install kubetest
### https://www.inovex.de/blog/testing-kubernetes-infrastructure/

```
# Install the latest version of kubetest
go get -u k8s.io/test-infra/kubetest
# Tests need to match cluster version
K8S_VERSION=$(kubectl version -o json | jq -r '.serverVersion.gitVersion')
# Required for historic reasons
export KUBERNETES_CONFORMANCE_TEST=y
# Needs to be set explicitly
export KUBECONFIG=”$HOME/.kube/config”
# Actual run command, skeleton is the provider for existing clusters
kubetest --provider=skeleton --test --test_args=”--ginkgo.focus=\[Conformance\]” --extract ${K8S_VERSION}


```

> Note: This may take a few minutes depending on your network speed

## Extract the Version

```
kubetest --extract=v1.13.0

cd kubernetes

export KUBE_MASTER_IP="192.168.5.11:6443"

export KUBE_MASTER=master-1

kubetest --test --provider=skeleton --test_args="--ginkgo.focus=\[Conformance\]" | tee test.out

```


This could take about 1.5 to 2 hours. The number of tests run and passed will be displayed at the end.
