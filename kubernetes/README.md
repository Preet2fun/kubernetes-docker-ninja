# kubernetes-docker-ninja

#check the version of the cluster that you are running:
$ kubectl version

#simple diagnostic for the cluster. This is a good way to verify that your cluster is generally healthy:
$ kubectl get componentstatuses

#you can list out all of the nodes in your cluster:
$ kubectl get nodes

#You can use the kubectl describe command to get more information about a specific node, such as node-1:
$ kubectl describe nodes node-1

#If your cluster runs the Kubernetes proxy with a DaemonSet, you can see the proxies by running:
$ kubectl get daemonSets --namespace=kube-system kube-proxy

#The DNS service is run as a Kubernetes deployment, which manages these replicas:
$ kubectl get deployments --namespace=kube-system coredns

#There is also a Kubernetes service that performs load balancing for the DNS server:
$ kubectl get services --namespace=kube-system coredns

#If you want to see what the command will do without actually making the apply
#changes, you can use the flag to print the objects to the terminal without --dry-run actually sending them to the server.
kubectl apply -f namespace_obj.yaml --dry-run

#to add the color=red label to a Pod named bar, you can run:
$ kubectl label pods bar color=red

#You can use the following to see the logs for a running container:
$ kubectl logs <pod-name>

#You can also use the exec command to execute a command in a running container:
$ kubectl exec -it <pod-name> -- bash

#You can also copy files to and from a container using the cp command:
$ kubectl cp <pod-name>:</path/to/remote/file> </path/to/local/file>
exp : kubectl cp nginx:/tmp/abc.txt /root/kubernetes/def.txt

#you can use the top command to see the list of resources in use by either nodes or Pods
$ kubectl top nodes
$ kubectl top nodes



######################################### POD ##################################################

#The simplest way to create a Pod is via the imperative kubectl run command
$ kubectl run pod-manu --generator=run-pod/v1 --image=preet2fun/simple-node:v1
$ kubectl run alpaca-prod \
--image=gcr.io/kuar-demo/kuard-amd64:blue \
--labels="ver=1,app=alpaca,env=prod"

# belwo command will show labels along with its basic information
kubectl get pods --show-labels
kubectl get pods --selector="app=bandicoot,ver=2"
kubectl get pods --selector="app in (alpaca,bandicoot)"


# to add label to specific pod after its creation use below command
kubectl label pod alpha-prod "color=red"

#You can remove a label by applying a dash suffix:
$ kubectl label pod alpaca-prod "color-"

# to take configuration of current running pod in yaml file for edit it use below command and then edit that file using VIM editor
kubectl get pod resource-example -o yaml >> resource-request-limits-pod.yaml


########################################## Deployments ###########################################

#This is the simplest way to create deployment via imperative kubectl run command


########################################## ReplicaSets ###########################################
#imperative commands for scale up/down of replicaset
kubectl scale replicaset <replicaset-name> --replicase=3

#Scaling based on CPU usage is the most common use case for Pod autoscaling and you can use below commad for same
kubectl autoscale rs kuard --min=2 --max=5 --cpu-percent=80

# to see horizontal pod autoscaling(hpa) use below commnd
kubectl get hpa

#If you don’t want to delete the Pods that are being managed by the ReplicaSet, you
#can set the --cascade flag to false to ensure only the ReplicaSet object is deleted and not the Pods:
kubectl delete rs kuard --cascade=false
