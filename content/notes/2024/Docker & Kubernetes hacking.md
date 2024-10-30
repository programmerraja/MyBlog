---
title: Kubernetes hacking
date: 2024-03-17T06:42:50.5050+05:30
draft: false
tags:
  - devops
  - hacking
---
## Tools

1. **Kube-bench**
   - Checks Kubernetes deployment against the CIS Kubernetes Benchmark.
   - [GitHub Repository](https://github.com/aquasecurity/kube-bench)

2. **Popeye**
   - Scans live Kubernetes clusters for potential issues with resources and configurations.
   - [Popeye CLI](https://popeyecli.io/)

3. **Kube-hunter**
   - Hunts for security weaknesses in Kubernetes clusters by checking for open ports and accessible information.
   - Can be run inside a pod to access the Kubernetes API token.
   - [GitHub Repository](https://github.com/aquasecurity/kube-hunter)

4. **Sonobuoy**
   - Certifies Kubernetes clusters.
   - [Sonobuoy Documentation](https://sonobuoy.io/certifying-kubernetes-with-sonobuoy/)

## Security Practices

### Container Best Practices
- Run containers as non-root users.
- Use distroless images (only include language runtimes).
- Isolate the network for enhanced security.

### Admission Controllers

- **Open Policy Agent**
  - A powerful policy engine for enforcing security and compliance.
  - [Open Policy Agent](https://www.openpolicyagent.org/)

### Activity Monitoring

- Set up alerts for unusual activities, such as:
  - Container executions
  - Creation of new services
- **Monitoring Tools:**
  - [Falco](https://falco.org/)

### Image Scanning

1. [Clair](https://github.com/quay/clair) - Static analysis for vulnerabilities.
2. [Trivy](https://github.com/aquasecurity/trivy) - User-friendly vulnerability scanner.
3. Copacetic - Used to patch vulnerabilities reported by Trivy.
4. Sysdig - Another option for image scanning.

### Access Control

- Every pod can access its service account token, allowing it to call the Kubernetes API at:
  - `https://kubernetes/api/v1/namespaces/default`

### Playground
- **Kubernetes Goat**: A "Vulnerable by Design" cluster environment to learn and practice Kubernetes security.
  - [Kubernetes Goat GitHub](https://github.com/madhuakula/kubernetes-goat)

### Security Enhancements
- Set `automountServiceAccountToken` to `false` in ServiceAccount configurations.

```yaml
  apiVersion: v1
  kind: ServiceAccount
  metadata:
    name: beta
  automountServiceAccountToken: false
```

- Ensure that pods do not run as root users.
- Encrypt secrets before storing them, using tools like HashiCorp Vault.
- Secure etcd.
- Configure network policies to restrict pod communication across namespaces.

### Pod Security Admission
- Implement the [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/) to define pod isolation levels.

### Security Context for Pods

Set the following security context in the pod YAML file:
```yaml
securityContext:
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  runAsUser: 1000 # (userId)
```

- Avoid using `privileged: true` in a Kubernetes securityContext, as it grants the container access to the node's resources.

---

## Additional Best Practices for AKS
1. Block IP access to the API server.
2. Use Microsoft Defender for AKS.
3. Rotate kubelet certificates.

## Finding Container Services on the Internet
1. [Censys](https://search.censys.io/)
2. [BinaryEdge](https://www.binaryedge.io/)
3. [Shodan](https://www.shodan.io/search)


## Tools for Attacking and Scanning
- [KubeHound](https://github.com/DataDog/KubeHound) - Automated calculation of attack paths in a cluster.
- [Netfetch](https://github.com/deggja/netfetch) - Scans clusters for network policies and identifies unprotected workloads.

## Docker Security
- The command `docker run -ti --privileged --net=host --pid=host --ipc=host --volume /:/host busybox chroot /host` gives the container unrestricted access to the host's resources.

## Resources
- [Kubernetes Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/)
- [Kubernetes Security Footgun in etcd](https://gcollazo.com/the-security-footgun-in-etcd/)
- [K8s SIG-Security](https://github.com/kubernetes/sig-security)
- [CNCF TAG-Security](https://github.com/cncf/tag-security)
- [K8s SIG-Network](https://github.com/kubernetes/community/tree/master/sig-network)
- [OpenSSF](https://openssf.org/)
- [Kubernetes Network Policy Recipes](https://github.com/ahmetb/kubernetes-network-policy-recipes)
- [Attack MITRE ATT&CK®](https://attack.mitre.org/)
- [OWASP Kubernetes Top Ten](https://owasp.org/www-project-kubernetes-top-ten/)
- [CyberArk: Attacking Kubelet](https://www.cyberark.com/resources/threat-research-blog/using-kubelet-client-to-attack-the-kubernetes-cluster)
-  [Armosec Blog](https://www.armosec.io/blog/)
- [HackTricks: Pentesting Cloud Kubernetes Security](https://cloud.hacktricks.xyz/pentesting-cloud/kubernetes-security)