+++
title = 'Kubernetes hacking'
date = 2024-03-17T06:42:50.5050+05:30
draft = true
tags =[]
+++ 


## Tools

Checks whether Kubernetes is deployed according to security best practices as defined in the CIS Kubernetes Benchmark
https://github.com/aquasecurity/kube-bench 

Popeye is a utility that scans live Kubernetes clusters and reports potential issues with deployed resources and configurations
https://popeyecli.io/

Hunt for security weaknesses in Kubernetes clusters python script wil look for opened ports and list all and what are the info that can be accessed

run inside POD it will access the token and do 

https://github.com/aquasecurity/kube-hunter

https://sonobuoy.io/certifying-kubernetes-with-sonobuoy/ 



## Notes

Run a container as noon root
use distroless images (only have language runtime )
isolate the network 

Admission controller
1. [Open policy agent ](https://www.openpolicyagent.org/)

Activities monitoring

Getting alerts when container exec , new service created etc that are weired.
tools [Falco](https://falco.org/)  

Image scanning
1. [Clair](https://github.com/quay/clair)
2. [trivy](https://github.com/aquasecurity/trivy) It is easy compare to clair
3. copacetic (used to patch the bug reported by trivy)
4. 
5. sysdig


Every pod can access the token that will can call kube API 
 
we can use this token to acess the kube API  `https://kubernetes/api/v1/namespaces/default`


Playground

Kubernetes Goat is a "Vulnerable by Design" cluster environment to learn and practice Kubernetes security using an interactive hands-on playground https://github.com/madhuakula/kubernetes-goat

The ServiceAccount and Pod configurations in this application's Namespace is using the default `automountServiceAccountToken` setting of `true` so set as false
don't run a pod as root user
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: beta
automountServiceAccountToken: false
```
communication between pods are unencrypted
By default secrets are not encrypted any one can view so encrypt and store it use vault

secure the etcd 

[Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

Pods can communicate with any other pod even in different namespace so add network policy

AKS security best practice
1. Block ip to access api server
2. Microsoft defender for AKS
3. Rotate kubelete certificate

Set following security context in pod ymal file

```
securityContext
	allowPrivilegeEscalation:false
	readOnlyRootFileSystem:true
	runAsUser:1000 #(userId )
```


 `privileged: true`  in a Kubernetes securityContext YAML file effectively grants the container access to the node's resources. When a container runs with `privileged: true`, it essentially gains access to all system resources on the node where it's scheduled.
# Pod Security Admission
The Kubernetes [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/) define different isolation levels for Pods. These standards let you define how you want to restrict the behavior of pods in a clear, consistent fashion.

some best practice to follow
https://github.com/snyk-labs/kubernetes-goof/blob/main/workshop/03-mitigations.md

Resources
1. https://kubernetes.io/docs/concepts/security/pod-security-admission/
2. https://gcollazo.com/the-security-footgun-in-etcd/
- [K8s SIG-Security](https://github.com/kubernetes/sig-security)
- [CNCF TAG-Security](https://github.com/cncf/tag-security)
- [K8s SIG-Nework](https://github.com/kubernetes/community/tree/master/sig-network)
- [OpenSSF](https://openssf.org/)


https://github.com/ahmetb/kubernetes-network-policy-recipes
https://networkpolicy.io/



https://attack.mitre.org/ MITRE ATT&CK® is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. The ATT&CK knowledge base is used as a foundation for the development of specific threat models and methodologies in the private sector, in government, and in the cybersecurity product and service community











#### Finding container service on internet
1. https://search.censys.io/
2. https://www.binaryedge.io/
3. https://www.shodan.io/search


#### OWASP Kubernetes Top Ten
https://owasp.org/www-project-kubernetes-top-ten/


Attacking kubelet
1. https://www.cyberark.com/resources/threat-research-blog/using-kubelet-client-to-attack-the-kubernetes-cluster


https://attack.mitre.org/ -> globally-accessible knowledge base of adversary tactics and techniques based on real-world observations.

https://security.googleblog.com/2022/05/privileged-pod-escalations-in.html 

## Docker

`docker run -ti --previliged -net=host -pid=host --ipc=host --volume/:/host busybox chroot /host`  ->give the container unrestricted access to the host's resources.

#### Tool
- [A Kubernetes attack graph tool allowing automated calculation of attack paths between assets in a cluster ](https://github.com/DataDog/KubeHound?tab=readme-ov-file)
- 