## AWS CLI Access over saml2aws with Okta

Download and install `saml2aws` tool. Check the README for the installation instructions. You can use `brew/chocolatey` or just grab a `tar` release and put it in your `$PATH`.

Backup or remove your old AWS access settings by doing something like:
```
cd /home/$USER/.aws
mv credentials credentials_backup
mv config config_backup
```

Run `saml2aws configure`. It's better to run configuration process with this command rather than do it manually - because of issues may occur. The tool will ask for settings, answer like below:

```
? Please choose a provider: Okta
? Please choose an MFA: Auto
? AWS Profile: lab
? URL: https://yourcompany.okta.com/xxx
? Username: s.a.tretiakov
? Password: ***
? Confirm ***
```

To configure another profile use the following command:
`saml2aws configure -a lab`

Full configuration for a separate profile looks like below:
```
[lab]
name                    = lab
app_id                  = 
url                     = https://yourcompany.okta.com/xxx
username                = s.a.tretiakov
provider                = Okta
mfa                     = Auto
skip_verify             = false
timeout                 = 0
aws_urn                 = urn:amazon:webservices
aws_session_duration    = 3600
aws_profile             = lab
resource_id             = 
subdomain               = 
role_arn                = 
saml_cache              = false
disable_remember_device = false
disable_sessions        = false
download_browser_driver = false
headless                = false
mfa_ip_address          = 
policy_file             = 
policy_arn_list         = 
region                  = eu-central-1
http_attempts_count     = 
http_retry_delay        = 
credentials_file        = 
saml_cache_file         = 
target_url              = 
prompter                = 
kc_broker               = 
```

Run `saml2aws login` and enter your user and password. If it fails, disable the keychain and trying again: `saml2aws login --disable-keychain`.

To login under another profile use the following command:
`saml2aws login -a lab`

And you should be in. To validate, run:
`aws eks list-clusters --profile lab` should return a list of clusters
`aws ecr describe-repositories` should list your repositories
`aws sts get-caller-identity --profile lab` should show your role followed by your user name:
```
{
    "UserId": "XXX:s.a.tretiakov",
    "Account": "12345678",
    "Arn": "arn:aws:sts::12345678:assumed-role/$ROLE/s.a.tretiakov"
}
```

If you don't want to type `–profile lab`  and `–region eu-west-1` all the time, you can add it your shell configuration file (if you use bash, this is `~/.bashrc`)

```
export AWS_PROFILE="lab"
export AWS_REGION="eu-west-1"
```
It is possible to log in to several aws instances and change working profiles with just resetting the `AWS_PROFILE` value

## K8s access

#### Install aws v2:
`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
`unzip awscliv2.zip`
`sudo ./aws/install`
`aws --version`

#### Install kubectl:
`sudo snap install kubectl –classic`
`aws eks list-clusters`
`aws eks update-kubeconfig --region eu-west-1 --name cluster-name`
`kubectl get namespaces`

File `~/.kube/config` stores configuration for kubectl and k9s.
- **Clusters** define the clusters you're connecting to. You need to specify the cluster's `server` URL and its certificate authority.
- **Users** define the users with their respective credentials.
- **Contexts** combine a cluster and a user. You also can specify the namespace in which you want to operate. It makes sense to give descriptive names to the contexts.
- **current-context** specifies the default context that `kubectl` or `k9s` will use if you don't provide context explicitly.

To list namespaces: `kubectl get namespaces`
To list pods in the namespace: `kubectl get pods -n <NAMESPACE>`
#### Install k9s:
`brew install derailed/k9s/k9s`
`k9s -v`

`k9s --context <CTX_NAME>` - connect with the specified context. `<0>` , `<1>`, etc. in the k9s gui allow you to switch recent namespaces.
`k9s --namespace <NS_NAME>` - connect to the specified namespace.

#### K9s
`:` - type a command inside k9s:
`pod` - view pods
`service` - view services
`namespace` - view namespaces

`/` - filter
