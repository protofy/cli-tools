# AWS Kubectl 1Password Wrapper

Allows you to get rid of local AWS Credentials for Kubernetes Tools like Kubectl and K9S

## Requirements
 - AWS Credentials with EKS Access in 1PW
 - [1Password CLI](https://developer.1password.com/docs/cli/get-started/)
 - [AWS CLI](https://formulae.brew.sh/formula/awscli)

## Install
 1. Download aws-op script
 2. Run `chmod +x aws-op` wherever you Downloaded it to to make it executable
 3. Move it to your bin folder with `mv aws-op ~/bin/`

## Background / How Kubeconfigs work with AWS EKS

When you connect to a Elastic Kubernetes Cluster in AWS, the AWS CLI is used to get an Acces Token for the Cluster. The Block in your `~/.kube/config` that does that is this one:

```yaml
[...]
users:
  - name: arn:aws:eks:eu-central-1:844831511534:cluster/prod-cluster
    user:
      exec:
        apiVersion: client.authentication.k8s.io/v1beta1
        args:
          - --region
          - eu-central-1
          - eks
          - get-token
          - --cluster-name
          - prod-cluster
          - --output
          - json
        command: aws
        env:
          - name: AWS_PROFILE
            value: albis
        interactiveMode: IfAvailable
        provideClusterInfo: false
[...]
```
As you can see, this defines the CLI Command to retrieve the Token as well as the AWS_PROFILE that is used for the credentials.
These Profiles are stored in `~/.aws/config` and, more importantly, `~/.aws/credentials`. 
Which is less than ideal, because it means that you have to have your credentials in there in clear text.

## Usage
After installation, you can modify your `~/.kube/config` as follows, compared to the example above:

```yaml
[...]
users:
  - name: arn:aws:eks:eu-central-1:844831511534:cluster/prod-cluster
    user:
      exec:
        apiVersion: client.authentication.k8s.io/v1beta1
        args:
          - op://${your_vault}/${your_aws_credentials_item}/${your_access_key_id}
          - op://${your_vault}/${your_aws_credentials_item}/${your_secret_access_key}
          - ${your_cluster_region}
          - ${your_cluster_name} 
        command: aws-op
        interactiveMode: IfAvailable
        provideClusterInfo: false
[...]
```

Once this is done, switching the Context should cause the 1Password Prompt to appear. Auth against that, and you got access.
