# retool-apollo
This is the Apollo module to create a working Retool install in Apollo. It installs Retool and its dependencies.

# Prerequisites
You need to have a [k8s cluster in EKS](https://github.palantir.build/vasilev/retool-k8s-tf) and a Terraform Cloud account.
You need an Apollo environment and apollo-cli set up locally.

# Setup instructions
Install products:
```bash
apollo-cli publish helm-chart --helm-chart-name hcp-terraform-operator --helm-chart-version 2.7.1 --helm-repository-url "https://helm.releases.hashicorp.com" --maven-coordinate "corp.hashicorp:hcp-terraform-operator:2.7.1"

apollo-cli create-product-tgz manifest --manifest-file=temporal-manifest.yaml
apollo-cli publish artifact --product-tgz=temporal-0.51.0.config.tgz

apollo-cli create-product-tgz manifest --manifest-file=retool-manifest.yaml
apollo-cli publish artifact --product-tgz=retool-6.2.14.config.tgz

wget -qO- https://raw.githubusercontent.com/ucabvas/terraform-aws-vv-rds/main/charts/terraform-aws-vv-rds-helm/deployment/manifest.yml > terraform-aws-vv-rds-helm-manifest.yml
apollo-cli publish manifest --manifest-file terraform-aws-vv-rds-helm-manifest.yml
```
Install module
```bash
apollo-cli module create --module-definition-file retool-apollo-module.yaml
```

Finally, install to your Apollo environment via the Apollo UI.

# Testing

You can verify that retool is working correctly by accessing it via a kubectl port forward:
```bash
kubectl port-forward service/retool-dev 3000:3000 -n retool-dev
```

