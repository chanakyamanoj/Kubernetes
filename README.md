What is a Pod 
Pods are the smallest deployable units which contain one or more containers that share the same network & storage resources.
Pods run inside nodes. Each node can have many pods running inside it.

Pod Creation
Imperative Way	(Instant pod creation with single cmd)
k run  <pod-name>     - - image=<image name>
Declarative way
k apply -f <filename.yaml>		👉 modify the yaml file and apply changes
k get pods				👉 Pod details
k get pods -o wide  			👉 Wide information about Pod
k get pods  - - watch			👉 it watches for changes & prints updates as they happen
## vim redis.yaml						## pod with priority class
     	  		
k create -f  <filename.yaml>			👉 Declarative way pod creation 
k get pod - -show-labels			👉 get labels associated to pod
k explain pod 					👉 Description about Pod version etc.,
k apply -f <filename.yaml>			👉 modify the yaml file and apply changes
k get replicaset (or) rs				👉 Get list of the replicaset’s
k get pods 					👉 Details about all pods 
k describe pod <pod name>			👉 Details about specific pod
k edit pod <pod_name>			👉 Edit the pod (yaml file)
k exec -it <pod_name>  /bin/bash		👉 Enter into the Pod for troubleshoot
k delete pod <pod_name>			👉 Delete a Pod
k delete pod <pod name>   - - now		👉 Delete a pod immediately
k delete pod <pod name> - - grace-period=0  - - force  -n <name space>   👉 Force delete Pod
k logs  <pod_name>				👉 to get logs of the Pod
K logs <pod name> -c sidecar			👉 to get the logs for 2nd container
k get pod -o yaml				👉 Get all Pod in yaml file
k run <name of pod> <- - image=image name> - - dry-run=client -o yaml | tee <file name.yml>							👉 
ssh <Node name>  ping -c 3 <ip address of pod>	👉 check the ping status
{ cat nginx.yml; echo “---’;   cat ubuntu.yml; } | tee combined.yml
👉 concatenate 2 yml files with a YAML separator (---) & save the result to another yml file
<podname>_IP=$(kubectl get pods -o wide | awk ‘/<pod name>/ { print $6 }’); echo $<pod name>_IP								👉 it will print pod IP
Pod Limitations

Single vs Multi-container Pods
Single
The “one-container-per-Pod” model is the most common Kubernetes use case.
In this case, you can think of a Pod as a wrapper around a single container.
Kubernetes manages Pods rather than managing the containers directly.
Multi
Multi-container pods are like roommates sharing an apartment—each container has its own role but lives in the same space, collaborating to get a job done.
Multi-container pods will share the resources of the pod. 
for ex: IP Address,Hostname,Ports,Sockets,Memory,Volumes, etc
Types of container
Main Container - > is your primary app or space
Init container - > is like a helper container that runs before your main app starts and only once. If it fails, the pod will not proceed to the main containers.
Side Car Container - > logs, metrics collection,config updates
Proxy container - > Handles external traffic or API routing
Life Cycle of a Pod
They’re created, they live, and they die. When a pod dies, K8s does not try to bring it back, Instead it starts a new pod with a new IP Address.

The pod lifecycle starts when a user posts a POD creation YAML manifest file to the API server, and the Pod is scheduled to one of the healthy nodes by the Scheduler.
Pending
Pod enters the pending state until the container runtime on the node downloads images and starts the containers (or) Volume claims being provisioned (or) Node resource availability check (or) misconfiguration in yaml
The Pod remains in the pending state until all of its resources are up and ready.
Running
Once everything’s up and ready, the Pod enters the running state.

Succeded
Once it has completed all of its tasks, it gets terminated & enters the succeeded state.
Typically applies to cronjobs, data migration, backup scripts.
Failed
It can go from Pending to Failed state, if the pod creation fails due to some reason
It can go from Running to Failed state, if the pod/app crashes due to some reason.
Unknown
If the control plane can’t reach the node where the pod was running due to network issues or node failure - the status is unknown.
