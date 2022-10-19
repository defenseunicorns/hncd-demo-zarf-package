# HND Demo Zarf Package
This repo is intended to serve as a demo for a basic HNCD zarf package.

## Prerequisites / Assumptions
* There is an existing k8s cluster
* User has IronBank repository credentials
* User has access to the appropriate :unicorn: AWS account for SOPS credentials

## Deployment Instructions
* Configure your local environment to connect to the target environment
* Initialize zarf with `zarf init`
  * 'y' to init
  * 'n' to PLG
  * 'y' to Gitea
> TODO - replace this block^ with a zarf init command that doesn't require additional input
* Create the `flux-system` namespace with `kubectl create ns flux-system`
* Configure SOPS credentials for flux
  * ```
    cat << EOF | kubectl apply -n flux-system -f -
    apiVersion: v1
    kind: Secret
    metadata:
      name: sops-creds
      namespace: flux-system
    stringData:
      sops.aws-kms: |
        aws_access_key_id: <key_id>
        aws_secret_access_key: <key_secret>
    EOF
    ```
* Navigate to the source repo
* Build the zarf package with `zarf package create`
* Deploy the zarf package with `zarf package deploy zarf-package-hncd-bigbang-amd64.tar.zst`
  * 'y' to the package
  * Choose your own adventure on the optional packages
* ...
* Profit?

### Optional steps / tips and tricks:
* Monitor ongoing status with `watch kubectl get hr,ks -A`
* Use the jump box to build/deploy the package in order to avoid traversing the VPN for package installation

