+++
title = 'Kubernetes'
date  = 2024-01-01T08:19:32.3232+05:30
draft = false
+++
### Kubernetes is two things
  - A cluster for running applications
  - An orchestrator of cloud-native microservices apps

### Kubernetes cluster

A Kubernetes cluster contains six main components:

1. `API server:` Exposes a REST interface to all Kubernetes resources. Serves as the front end of the Kubernetes control plane.
2. `Scheduler`: Places containers according to resource requirements and metrics. Makes note of Pods with no assigned node, and selects nodes for them to run on.
3. `Controller manager`: Runs controller processes and reconciles the cluster’s actual state with its desired specifications. Manages controllers such as node controllers, endpoints controllers and replication controllers.
4. `Kubelet`: Ensures that containers are running in a Pod by interacting with the Docker engine , the default program for creating and managing containers. Takes a set of provided PodSpecs and ensures that their corresponding containers are fully operational.
5. `Kube-proxy`: Manages network connectivity and maintains network rules across nodes. Implements the Kubernetes Service concept across every node in a given cluster.
6. `Etcd:` Stores all cluster data like how many pod and there replica count . Consistent and highly available Kubernetes backing store.

**Ways to create kubernetes cluster**

- [Kind](https://kind.sigs.k8s.io/) [Fast and easy]
- Minikube
- microk8s
- kudeadm
- Google cloud platform
- AWS
- Azure

### Nodes

Nodes are the workers of a Kubernetes cluster. At a high-level they do three things:

1. Watch the API Server for new work assignments
    
2. Execute new work assignments
    
3. Report back to the control plane (via the API server)

Nodes contain

- **Kubelet** →When you join a new node to a cluster, the process installs kubelet onto the node. The kubelet is then responsible for registering the node with the cluster. Registration effectively pools the node’s CPU, memory, and storage into the wider cluster pool. One of the main jobs of the kubelet is to watch the API server for new work assignments. Any time it sees one, it executes the task and maintains a reporting channel back to the control plane.

- **Container runtime** →The Kubelet needs a container runtime to perform container-related tasks things like pulling images and starting and stopping containers.There are lots of container runtimes available for Kubernetes. One popular example is cri-containerd .

- **Kube-proxy** →This runs on every node in the cluster and is responsible for local cluster networking. For example, it makes sure each node gets its own unique IP address, and implements local IPTABLES or IPVS rules to handle routing and load-balancing of traffic on the Pod network.

### Kubernetes DNS

Every Pod on the cluster, meaning all containers and Pods know how to find it. Every new service is automatically registered with the cluster’s DNS so that all components in the cluster can find every Service by name. Some other components that are registered with the cluster DNS are StatefulSets and the individual Pods that a StatefulSet manages.Cluster DNS is based on CoreDNS ([https://coredns.io/](https://coredns.io/)).

### Pods (it’s just a sandbox for hosting containers.)

In the VMware world, the atomic unit of scheduling is the virtual machine (VM). In the Docker world, it’s the container. Well… in the Kubernetes world, it’s the Pod. The simplest model is to run a single container per Pod. However, there are advanced use-cases that run multiple containers inside a single Pod. `An infrastructure-centric use-case for multi-container Pods is a service mesh.`

**Pod lifecycle**

Pods are mortal. They’re created, they live, and they die. If they die unexpectedly, you don’t bring them back to life. Instead, Kubernetes starts a new one in its place

**How do we deploy Pods**

To deploy a Pod to a Kubernetes cluster you define it in a manifest file and POST that manifest file to the API Server. The control plane verifies the configuration of the YAML file, writes it to the cluster store as a record of intent, and the scheduler deploys it to a healthy node with enough available resources. This process is identical for single-container Pods and multi-container Pods.

We can also run the Pod directly mention the docker image `kubctl run podName --image=nignix:alpline`

**Pods and cgroups**

At a high level, Control Groups (cgroups) are a Linux kernel technology that prevents individual containers from consuming all of the available CPU, RAM and IOPS on a node. You could say that cgroups actively police resource usage.Individual containers have their own cgroup limits.

**Pod manifest files**

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: hello-pod
	labels:
	zone: prod
	version: v1
spec:
	containers:
	- name: hello-ctr
	image: nigelpoulton/k8sbook:latest
	ports:
	- containerPort: 8080
```

- The .apiVersion field tells you two things – the API group and the API version.
- .kind field tells Kubernetes the type of object is being deployed.
- .metadata section is where you attach a name and labels These help you identify the object in the cluster, as well as create loose couplings between different objects. You can also define the Namespace that an object should be deployed to. Keeping things brief, `Namespaces are a way to logically divide a cluster into multiple virtual clusters for management purposes.` In the real world, it’s highly recommended to use namespaces, however, you should not think of them as strong security boundaries.
- The .spec section is where you define the containers that will run in the Pod.
- `kubectl apply -f pod.yml` →command to POST the manifest to the API server.
- `kubectl get pods` →command to check the status.
- `kubectl get pods hello-pod -o yaml` → To get more details about pod
- `kubectl describe pods hello-pod` →This provides a nicely formatted multi-line overview of an object. It even includes some important object lifecycle events.
- `kubectlexec -it hello-pod --sh` →command will log-in to the first container

### ReplicaSet

To deploy multiple instance. A ReplicaSet is defined with fields, including a selector that specifies how to identify Pods it can acquire, a number of replicas indicating how many Pods it should be maintaining, and a pod template specifying the data of new Pods it should create to meet the number of replicas criteria. A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.

```jsx
apiVersion: apps/v1
kind: ReplicaSet
metadata:
name: frontend
labels:
  app: guestbook
  tier: frontend
spec:
replicas: 3
selector:
  matchLabels:
    tier: frontend
template:
  metadata:
    labels:
      tier: frontend
  spec:
    containers:
    - name: php-redis
      image: gcr.io/google_samples/gb-frontend:v3
```

**Deployments vs ReplicaSet**

Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets

CMD

1. `kubectl get rs` → get all replica

### Deployments

Deploy Pods indirectly via a higher-level controller. Examples of higher-level controllers include; Deployments, DaemonSets, and StatefulSets.

For example, a Deployment is a higher-level Kubernetes object that wraps around a particular Pod and adds features such as scaling, zero-downtime updates, and versioned rollbacks.

Behind the scenes, Deployments, DaemonSets and StatefulSets implement a controller and a watch loop that is constantly observing the cluster making sure that current state matches desired state.

**Difference between pod and Deployments**

Pods don’t self-heal, they don’t scale, and they don’t allow for easy updates or rollbacks. Deployments do all of these. As a result, you’ll almost always deploy Pods via a Deployment controller.

`Desired state` is what you want. Current state is what you have. If the two match, everybody’s happy.

`The declarative model` is a way of telling Kubernetes what your desired state is, without having to get into the detail of how to implement it. You leave the how up to Kubernetes.

**Reconciliation loops**

Fundamental to desired state is the concept of background reconciliation loops (a.k.a. control loops). For example, ReplicaSets implement a background reconciliation loop that is constantly checking whether the right number of Pod replicas are present on the cluster. If there aren’t enough, it adds more. If there are too many, it terminates some. To be crystal clear, Kubernetes is constantly making sure that current state matches desired state.

**Rolling updates with Deployments**

Now, assume you’ve experienced a bug, and you need to deploy an updated image that implements a fix. To do this, you update the same Deployment YAML file with the new image version and re-POST it to the API server. This registers a new desired state on the cluster, requesting the same number of Pods, but all running the new version of the image. To make this happen, Kubernetes creates a new ReplicaSet for the Pods with the new image.You now have two ReplicaSets – the original one for the Pods with the old version of the image, and a new one for the Pods with the updated version. Each time Kubernetes increases the number of Pods in the new ReplicaSet (with the new version of the image) it decreases the number of Pods in the old ReplicaSet (with the old versionof the image). Net result, you get a smooth rolling update with zero downtime.

**Deployments manifest files**

```yaml
apiVersion: apps/v1 #Older versions of k8s use apps/v1beta1
kind: Deployment
metadata:
name: hello-deploy
spec:
	replicas: 10
	selector:
		matchLabels:
		app: hello-world
	minReadySeconds: 10
	strategy:
		type: RollingUpdate
		rollingUpdate:
		maxUnavailable: 1
		maxSurge: 1
	template:
		metadata:
		labels:
			app: hello-world
		spec:
			containers:
			- name: hello-pod
			image: nigelpoulton/k8sbook:latest
			ports:
			- containerPort: 8080
```

- `The .spec` section is where most of the action happens. Anything directly below .spec relates to the Pod. Anything nested below
- `.spec.template` relates to the Pod template that the Deployment will manage. In this example, the Pod template defines a single container.
- `.spec.replicas` tells Kubernetes how may Pod replicas to deploy
- `.spec.selector` is a list of labels that Pods must have in order for the Deployment to manage them.
- `.spec.strategy` tells Kubernetes how to perform updatesto the Pods managed by the Deployment.Update using the RollingUpdate strategy
    - Never have more than one Pod below desired state ( maxUnavailable: 1 )
    - Never have more than one Pod above desired state ( maxSurge: 1 )
- `.spec.minReadySeconds` This is set to 10 , telling Kubernetes to wait for 10 seconds between each Pod being updated.(on new deployment)
- `kubectl apply -f deploy.yml` → will send the yml to API server to deploy
- `kubectl get deploy hello-deploy`

### Services

When newly pods created are scaled it will have new IP so if we have other pod communicating with it. it is unrealible.This is where Services come in to play. Services provide reliable networking for a set of Pods.

They operate at the TCP and UDP layer, Services do not possess application intelligence and cannot provide application-layer host and path routing. For that, you need an Ingress, which understands HTTP and provides host and path-based routing.

Services use labels and a label selector to know which set of Pods to load-balance traffic to. The Service has a label selector that is a list of all the labels a Pod must possess in order for it to receive traffic from the Service.

**Endpoint Objects**

Kubernetes is constantly evaluating the Service’s label selector against the current list of healthy Pods on the cluster. Any new Pods that match the selector get added to the `Endpoints object`, and any Pods that disappear get removed. This means the Endpoints object is always up to date. Then, when a Service is sending traffic to Pods, it queries its Endpoints object for the latest list of healthy matching Pods. Every Service gets its own Endpoints object with the same name as the Service.This object holds a list of all the Pods the Service matches and is dynamically updated as matching Pods come and go.

Types

- ClusterIP [default]
    
    - has a stable IP address and port that is only accessible from inside the cluster
    - Creating a new Service called “magic-sandbox” will trigger the following. Kubernetes will register the name“magic-sandbox”, along with the ClusterIP and port, with the `cluster’s DNS service`. The name, ClusterIP, and port are guaranteed to be long-lived and stable, and all Pods in the cluster send service discovery requests to the internal DNS and will therefore be able to resolve “magic-sandbox” to the ClusterIP. IPTABLES or IPVS rules are distributed across the cluster that ensure traffic sent to the ClusterIP gets routed to Pods on the backend
- Headless service → To directly communicate with master not with replica
    
- NodePort service →This builds on top of ClusterIP and enables access from outside of the cluster.[static ip]
    
    - Port range can be only between 30000 to 32767
        
    - Image
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e64b9889-d141-4df9-87a5-acae52ef24ab/57e57957-a7ea-4ca7-843b-96cc3bcb5836/Untitled.png)
        
- Loadbalancer
    

**Service YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
name: hello-svc
spec:
	ports:
	- port: 8080 #the port the service need to be listen
    targetPort: 8080 # the port the app is running
	selector:
		app: hello-world
		# Label selector
		# Service is looking for Pods with the label `app=hello-world` can be any key and vaule

deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: hello-deploy
spec:
	replicas: 10
	selector:
		matchLabels:
			app: hello-world
template:
	metadata:
		labels:
			app: hello-world
			# Pod labels
			# The label matches the Service's label selector
	spec:
		containers:
		- name: hello-ctr
```

- the Service has a label selector ( `.spec.selector` ) with a single value app=hello-world . This is the label that the Service is looking for when it queries the cluster for matching Pods.
- The Deployment specifies a Pod template with the same app=hello-world label ( `.spec.template.metadata.labels` )
- the Service will select all 10 Pod replicas and provide them with a stable networking endpoint and load-balance traffic to them.
- `kubectl get services` → Will list all service with IP get the IP from here

**How to use service**

To connect with service user the service name or ip by `kubectl get services`

```jsx
Redis(host="servicename",port="8080")
```

**Service discovery**

There are two major components to service discovery:

- Service registration
- Service discovery

**Ingress**

CMD

- `kubectl get pod -o wide` → will display the IP
- `kubectl apply -f svc.yml` →Tells Kubernetes to deploy a new object from a file called svc.yml . The .kind field in the YAML file tells Kubernetes that you’re deploying a new Service object.
- `kubectl get svc hello-svc` →Introspecting Services display the IP
- `kubectl get ep hello-svc` →the Endpoint controller’s

### Service Monitor

### Kubernetes storage

When we restart the pod the data stored in pod will go to solve this we using Kubernetes storage

**Storage for single pod**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: local-web
spec:
  replicas: 1
  selector:
    matchLabels: 
      app: local-web
  template:
    metadata:
      labels:
        app: local-web
    spec:
      containers:
      - name: local-web
        image: nginx
        ports:
          - name: web
            containerPort: 80
        volumeMounts:
          - name: local -> label must need to match with voulme
            mountPath: /usr/share/nginx/html #Path inside the container
      volumes:
      - name: local
        hostPath:
          path: /var/nginxserver
```

- When we store data in `/usr/share/nginx/html` in this dir it will be stored in `/var/nginxserver` this kind of storage cannot be shared among the pod and nodes

**Shared storage**

The three main resources in the persistent volume subsystem are:

- Persistent Volumes (PV)
- Storage Classes (SC) → Type of storage class ex NFS,AWS,azure..etc
- Persistent Volume Claims (PVC) → used to claim the storage that was alloacted using PV

1. First create PV with following
    
    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: nfs
    spec:
      capacity:
        storage: 500Mi
    	persistentVolumeReclaimPolicy: Retain
      accessModes:
    	    - ReadWriteMany 
      storageClassName: nfs #type of storage that we using
      nfs:
        server: 192.168.1.7
        path: "/srv/nfs"
    ```
    
    - `.spec.accessModes` defines how the PV can be mounted. Three options exist:
        - ReadWriteOnce (RWO)
        - ReadWriteMany (RWM)
        - ReadOnlyMany(ROM)
        - **ReadWriteOnce** defines a PV that can only be mounted/bound as R/W by a single PVC. Attempts from multiple PVCs to bind (claim) it will fail.
        - **ReadWriteMany** defines a PV that can be bound as R/W by multiple PVCs. This mode is usually only supported by file and object storage such as NFS. Block storage usually only supports RWO .
        - **ReadOnlyMany** defines a PV that can be bound by multiple PVCs as R/O. A couple of things are worth noting. First up, a PV can only be opened
    - `persistentVolumeReclaimPolicy` This tells Kubernetes what to do with a PV when its PVC has been released. Two policies currently exist:
        - Delete →This policy deletes the PV and associated storage resource on the external storage system
        - Retain
2. Deploy the PV by kubctl -f apply filename.yml
    
    - **kubctl get pv** > to get all PV
    - PV are not bound to namespace it can be accesed by any namspace
3. Create PVC to claim the pV
    
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mycustomvolume
    spec:
      accessModes:
        - ReadWriteMany
      storageClassName: nfs #we can create custom storage class 
      resources:
        requests:
          storage: 100Mi4
    ```
    
    - `.spec` section must match with the PV you are binding it with. In this example, access modes, storage class, and capacity must match with the PV. but the storage can be less then what we give in `PV`
    - It bound to namespace
    - `kubctl describe pvc` → to describe what’ going on helpfull to debug.
4. Use it in deployment
    
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nfs-web
    spec:
      replicas: 1
      selector:
        matchLabels: 
          app: nfs-web
      template:
        metadata:
          labels:
            app: nfs-web
        spec:
          containers:
          - name: nfs-web
            image: nginx
            ports:
              - name: web
                containerPort: 80
            volumeMounts:
              - name: nfs
                mountPath: /usr/share/nginx/html
          volumes:
          - name: mycustomvolume #Label need to match
            persistentVolumeClaim:
              claimName: nfs
    ```
    

### Config Maps

Kubernetes provides an object called a ConfigMap (CM) that lets you store configuration data outside of a Pod. It also lets you dynamically inject the data into a Pod at run-time. ConfigMaps are a map of key/value pairs and we call each key/value pair an entry.

**Example**

```yaml
#multimap.yml
kind: ConfigMap
apiVersion: v1
metadata:
	name: multimap
data:
	DB_URL: ""
	.env : | # these content stored in file name called .env on pod 
			env = plex-test
			endpoint = 0.0.0.0:31001
			char = utf8
			vault = PLEX/test
			log-size = 512M
```

- `kubectl apply -f multimap.yml`
- Use Pipe (|) to create a file

**Injecting ConfigMap data into Pods and containers**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: registry.k8s.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      env:
        # Define the environment variable
        - name: SPECIAL_LEVEL_KEY
          valueFrom:
            configMapKeyRef:
              # The ConfigMap containing the value you want to assign to SPECIAL_LEVEL_KEY
              name: multimap
              # Specify the key associated with the value
              key: DB_URL
  
```

**ConfigMaps and volumes**

Using ConfigMaps with volumes is the most flexible option. You can reference entire configuration files as well as make updates to the ConfigMap and have them reflected in running containers.

1. Create the ConfigMap
2. Create a ConfigMap volume in the Pod template
3. Mount the ConfigMap volume into the container
4. Entries in the ConfigMap will appear in the container as individual files

### Secretes

Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods. Kubernetes, and applications that run in your cluster, can also take additional precautions with Secrets, such as avoiding writing sensitive data to nonvolatile storage.

Kubernetes Secrets are, by default, stored unencrypted in the API server's underlying data store (etcd). Anyone with `API access can retrieve or modify a Secret, and so can anyone with access to etcd`. Additionally, anyone who is authorized to create a Pod in a namespace can use that access to read any Secret in that namespace; this includes indirect access such as the ability to create a Deployment.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: dotfile-secret
data:
  .secret-file: dmFsdWUtMg0KDQo=
---
apiVersion: v1
kind: Pod
metadata:
  name: secret-dotfiles-pod
spec:
  volumes:
    - name: secret-volume
      secret:
        secretName: dotfile-secret
  containers:
    - name: dotfile-test-container
      image: registry.k8s.io/busybox
      command:
        - ls
        - "-l"
        - "/etc/secret-volume"
      volumeMounts:
        - name: secret-volume
          readOnly: true
          mountPath: "/etc/secret-volume"
```

### StatefulSets

StatefulSets in Kubernetes are a resource type used to manage stateful applications, particularly those that require unique identities, stable network identifiers, and stable persistent storage. They are used for applications like databases where each instance needs a specific identity and state.

### Without StatefulSets

Without StatefulSets, deploying MongoDB in Kubernetes might lead to challenges:

- **Pod Identity:** Pods get random names, and their identities might change upon restarts or rescheduling. MongoDB instances require consistent and predictable identities for proper replication setup and cluster coordination.
- **Storage:** MongoDB needs persistent storage for its data files. Without StatefulSets, ensuring each pod has its own persistent volume becomes complex. If a pod is rescheduled or fails, data integrity can be compromised.

### With StatefulSets

Using StatefulSets for MongoDB:

- **Pod Identity:** StatefulSets assign stable, predictable identities (like **`mongo-0`**, **`mongo-1`**, etc.) to each MongoDB instance. This ensures consistency and stability crucial for proper replication and cluster coordination.
- **Storage:** StatefulSets facilitate the use of persistent volumes, ensuring that each MongoDB instance has its dedicated and persistent storage. Even if a pod fails or needs to be rescheduled, the data remains intact and can be reattached to a new pod with the same identity.

**StatefulSet Pod naming**

All Pods managed by a StatefulSet get predictable and persistent names. These names are vital, and are at the core of how Pods are started, self-healed, scaled, deleted, attached to volumes, and more. The format of StatefulSet Pod names is <StatefulSetName>-<Integer> . The integer is a zero-based index ordinal, which is just a fancy way of saying “number starting from 0”

**Ordered creation and deletion**

StatefulSets create one Pod at a time, and always wait for previous Pods to be running and ready before creating the next. This is different from Deployments that use a ReplicaSet controller to start all Pods at the same time,causing potential race conditions.

**Deleting StatefulSets**

Firstly, deleting a StatefulSet does not terminate Pods in order. With this in mind, you may want to scale a StatefulSet to 0 replicas before deleting it. You can also use terminationGracePeriodSeconds to further control the way Pods are terminated. It’s common to set this to at least 10 seconds to give applications running in Pods a chance to flush local buffers and safely commit any writes that are still in-flight.

### Kubectl

Kubectl is the main Kubernetes command-line tool and is what you will use for your day-to-day Kubernetes management activities.

kubectl configuration file is called config and lives in $HOME/.kube . It contains definitions for:

• Clusters • Users (credentials) • Contexts

**Clusters** is a list of clusters that kubectl knows about and is ideal if you plan on using a single workstation to manage multiple clusters. Each cluster definition has a name, certificate info, and API server endpoint.

**Users** let you define different users that might have different levels of permissions on each cluster. For example, you might have a dev user and an ops user, each with different permissions. Each user definition has a friendly name, a username, and a set of credentials.

**Contexts** bring together clusters and users under a friendly name. For example, you might have a context called deploy-prod that combines the deploy user credentials with the prod cluster definition. If you use kubectl with this context you will be POSTing commands to the API server of the prod cluster as the deploy user.

### Resources

1. [Give a idea about and overview of the architecture of that](https://youtu.be/s_o8dwzRlu4)
2. [Design principle](https://youtu.be/ZuIQurh_kDk)
3. [https://dev.to/leandronsp/kubernetes-101-part-i-the-fundamentals-23a1](https://dev.to/leandronsp/kubernetes-101-part-i-the-fundamentals-23a1)
4. [kubernetes 101 series](https://dev.to/mattiasfjellstrom/kubernetes-101-pods-part-1-mn2)
5. [https://github.com/jatrost/awesome-kubernetes-threat-detection](https://github.com/jatrost/awesome-kubernetes-threat-detection)
6. [https://openai.com/research/scaling-kubernetes-to-7500-nodes](https://openai.com/research/scaling-kubernetes-to-7500-nodes)
7. [https://github.blog/2019-11-21-debugging-network-stalls-on-kubernetes/](https://github.blog/2019-11-21-debugging-network-stalls-on-kubernetes/)
8. [https://iximiuz.ck.page/posts/ivan-on-containers-kubernetes-and-backend-development](https://iximiuz.ck.page/posts/ivan-on-containers-kubernetes-and-backend-development)
9. [https://dev.to/therubberduckiee/explaining-kubernetes-to-my-uber-driver-4f60](https://dev.to/therubberduckiee/explaining-kubernetes-to-my-uber-driver-4f60)
10. **[kubernetes -o architecture](https://aws.plainenglish.io/kubectl-get-kubernetes-o-architecture-6d4bd97dcaaf)**
11. [https://towardsdev.com/understanding-control-pane-and-nodes-in-k8s-architecture-5572018f7624](https://towardsdev.com/understanding-control-pane-and-nodes-in-k8s-architecture-5572018f7624)

### Products

1. [To cut down the cost of kubernetes](https://cast.ai/)
2. **[The first cloud native service provider powered only by Kubernetes](https://www.civo.com/)**