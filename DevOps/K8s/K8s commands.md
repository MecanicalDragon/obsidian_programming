`kubectl apply -f <yaml-name>.yaml` -  apply yaml configuration to k8s
`kubectl get pods\svc\ingress\all` -  show running pods/services
`kubectl logs <pod-name>` - pod logs

`helm create <project-name>` - create chart
`helm install <project-name> --dry-run --debug` - make server render template and return manifest file
`helm template --debug <project-name>` - output resulting deployment
`helm ls` - show active running apps