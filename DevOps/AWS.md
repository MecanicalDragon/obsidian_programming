## AWS CLI Access over saml2aws with Okta

Download and install `saml2aws` tool. Check the README for the installation instructions. You can use `brew/chocolatey` or just grab a `tar` release and put it in your `$PATH`.

Backup or remove your old AWS access settings by doing something like:
```
cd /home/$USER/.aws
mv credentials credentials_backup
mv config config_backup
```

Run `saml2aws configure`. The tool will ask for settings, answer like below:

```
? Please choose a provider: Okta
? Please choose an MFA: Auto
? AWS Profile: $YOUR_AWS_PROFILE
? URL: $YOUR_OKTA_ENV_URL
? Username: $YOUR_OKTA_USER
? Password: $YOUR_OKTA_PASSWORD
? Confirm ***
```

Run `saml2aws login` and enter your user and password. If it fails, disable the keychain and trying again: `saml2aws login --disable-keychain`.

And you should be in. To validate, run:
`aws eks list-clusters --profile $PROFILE` should return a list of clusters
`aws ecr describe-repositories` should list your repositories
`aws sts get-caller-identity --profile $PROFILE` should show your role followed by your user name:
```
{
    "UserId": "XXX:s.a.tretiakov",
    "Account": "12345678",
    "Arn": "arn:aws:sts::12345678:assumed-role/$ROLE/s.a.tretiakov"
}
```

If you don't want to type `–profile $PROFILE`  and `–region eu-west-1` all the time, you can add it your shell configuration file (if you use bash, this is `~/.bashrc`)

```
export AWS_PROFILE="$PROFILE"
export AWS_DEFAULT_REGION="eu-west-1"
```
Just don't forget you have those set as environment variables in case you want to work with other profiles or regions.

## K8s access

Install aws v2:
`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
`unzip awscliv2.zip`
`sudo ./aws/install`
`aws --version`

Install kubectl:
`sudo snap install kubectl –classic`
`kubectl config use-context arn:aws:eks:eu-west-1:<12345678>:cluster/<CLUSTER_NAME>`
`kubectl get namespaces`

To update kubectl config for AWS tool:
`aws eks --region eu-west-1 update-kubeconfig --role-arn arn:aws:iam::<12345678>:role/<ROLE> --name <CLUSTER_NAME>`

Install k9s:
`brew install derailed/k9s/k9s`
`k9s -v`

## Config

File `~/.kube/config` stores configuration for kubectl and k9s.
- **Clusters** define the clusters you're connecting to. You need to specify the cluster's `server` URL and its certificate authority.
- **Users** define the users with their respective credentials.
- **Contexts** combine a cluster and a user. You also can specify the namespace in which you want to operate. It makes sense to give descriptive names to the contexts.
- **current-context** specifies the default context that `kubectl` or `k9s` will use if you don't provide context explicitly.
## Commands

To list clusters: `aws eks list-clusters`
To list namespaces: `kubectl get namespaces`
To list pods in a namespace: `kubectl get pods -n <NAMESPACE>`

`k9s --context <CTX_NAME>` - connect with the specified context. `<0>` , `<1>`, etc. in the k9s gui allow you to switch recent namespaces.
`k9s --namespace <NS_NAME>` - connect to the specified namespace.

## K9s
`:` - type a command inside k9s:
`pod` - view pods
`service` - view services
`namespace` - view namespaces

`/` - filter
