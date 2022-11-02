# Here is what you need to make it into DevOps

* A yaml-based ci system - GitHub Actions has a substantial free tier, decent documentation, and a good ecosystem of libraries to string together.
    
* Kubernetes - you should know how and when to use deployment, statefulset, daemonset, ingress, service, configmap, secret, pvc, and pv. You can use k3s, minikube, or rancher for a local cluster to practice on.
    
* Helm - you should know how to write, modify, and deploy helm charts. Helm is just gotpl on top of yaml, so it's simple, but it is _the_ k8s deployment tool in practice.
    
* Docker - you should know how to use multistage builds, how to structure the layers to improve build time and artifact size, and buildkit features for secret management.
    
* Basic bash and the Linux terminal - no way around this, but you only have to learn it well enough to script, not to program.
    
* A few core aws tools - ec2, s3, route53, vpc, rds, and cloud watch are required; lambda, sns/sqs, api gateway, dynamodb, acm, and asm are optional.
    
* An infrastructure as code (IaC) tool - terraform or cloudformation preferably; ansible is an option, but it's going away.

* application configuration is coupled with your build artifact to become a full deployment artifact. Servers are standard amis released by tool maintainers or cloud providers, and the deployment artifacts are spread along however many servers you have up at the moment. Any additional OS configuration is handled at the cloud layer rather than the machine layer, using custom amis if required.\


For an easy but wide spread DevOps, docker and sysadmin intro you could set it all up yourself locally.

1.  Get a shitty server (raspberrypi, old dell pc, whatever)
    
2.  Throw Linux on it (headless is prefered)
    
3.  Install docker on it
    
4.  Get some more docker containers (gitlab or bitbucket + jenkins, portainer, kubernetes as k3s, etc).
    
5.  Try to setup all the shit one by one
    
6.  Try to connect them all up
    
7.  Test your CICD environment
    
8.  Slap nginx or nginx proxy manager (as docker container)
    
9.  Install Fail2Ban and other security measures
    
10. Portfoward your device to the Internet
    

Voila you have your own github alternative

"The Dev Ops Handbook" would be a good place to start.

https://github.com/aws-samples/aws-cdk-examples/tree/master/typescript/api-cors-lambda-crud-dynamodb